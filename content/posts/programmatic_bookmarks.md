+++
title = 'How I Programmatically Bookmark, Archive and Share Websites'
date = 2024-02-19T22:31:19-08:00
date_modified = 2024-02-19T22:31:19-08:00
+++

# How I Programmatically Bookmark, Archive and Share Websites

For the last decade plus, I’ve been tossing links to interesting pages in all sorts of random places. Firefox bookmarks, Apple Notes, Apple Reminders, Markdown files, emails to myself…

I figured it was finally time to organize all of that. Plus I’ve also wanted to add more content to my personal website for awhile. With my bookmarked links now organized in a single place, it was easy to auto-generate my [bookmarks page](/bookmarks).

To organize the links, I first dumped all of them into a single [Notion](https://www.notion.so) database. I chose Notion because it’s an easy way to setup a database, for free, with a custom schema that can have content added via a browser extension ([Notion web clipper](https://www.notion.so/help/web-clipper)) and the iOS share sheet. Each entry included its URL, title, a category, and notes. The title was auto-grabbed from the webpage by Notion’s web clipper. The category and notes (if any) were filled in manually by me.

Each category is a single-select column of the form “Software/Databases/PostgreSQL”. That way I could easily nest categories, while still pre-defining which categories are available.

From there, I downloaded a backup dump of my Notion workspace. This is something I already do weekly for backup purposes. (Hopefully I’ll fully automate it soon!)

I wrote a Python script (with some help from [Phind](https://www.phind.com/search?home=true)) that opens the ZIP file, find the Bookmarks database’s CSV, and converts the entries to a Markdown page. It’s even able to parse the nested categories and output nice header elements for them. I wrote the script in about 20 minutes, so it's not the prettiest, but it gets the job done.

```
# Python Script
import os
import zipfile
import csv
from pprint import pprint

def find_most_recent_zip(directory):
    # List all files in the directory
    files = os.listdir(directory)
    # Filter out only ZIP files
    zip_files = [file for file in files if file.endswith('.zip')]
    # If there are no ZIP files, return None
    if not zip_files:
        return None
    # Find the most recent ZIP file by sorting the list of ZIP files by modification time
    zip_files.sort(key=lambda file: os.path.getmtime(os.path.join(directory, file)))

    # Return the most recent ZIP file
    return os.path.join(directory, zip_files[-1])

def data_from_zip_file(zip_file_path):
    raw_bookmarks = []
    with zipfile.ZipFile(zip_file_path, 'r') as zip_ref:
        for file in zip_ref.namelist():
            with zip_ref.open(file) as f:
                if file.endswith('.zip'):
                    # If it is, recursively list its contents
                    return data_from_zip_file(f)
                elif file.endswith('.csv') and "Bookmarks 3d71628870ce46c1a2cbd54ceb00ca67_all.csv" in str(file):
                    csv_reader = csv.reader(f.read().decode('utf-8').splitlines())
                    for row in csv_reader:
                        raw_bookmarks.append(row)
    return {
        "raw_bookmarks": raw_bookmarks
    }

def raw_bookmarks_to_md(raw_bookmarks):
    md = ''

    raw_bookmarks = raw_bookmarks[1:]
    raw_bookmarks.sort(key=lambda b: b[0]) # alphabetized by title

    categorized_bookmarks = {}
    for bookmark in raw_bookmarks:
        category = bookmark[2]
        if not category:
            category = "Uncategorized"
        if category not in categorized_bookmarks:
            categorized_bookmarks[category] = []
        categorized_bookmarks[category].append(bookmark)


    category_parts = set()

    for category in sorted(categorized_bookmarks):
        parts = category.split("/")
        for i, part in enumerate(parts):
            # Check if the part has already been added
            if str(parts[:i+1]) not in category_parts:
                md += "\n"
                for _ in range(i+1):
                    md += "#"
                md += " " + " > ".join(parts[:i+1])
                md += "\n"
                category_parts.add(str(parts[:i+1]))
        for bookmark in categorized_bookmarks[category]:
            title = bookmark[0]
            url = bookmark[1]
            notes = bookmark[3]
            recommended = bookmark[5]
            if notes:
                md += f"- [{title}]({url}) - {notes}\n"
            else:
                md += f"- [{title}]({url})\n"

    return md

#
# Main Code
#

# Find the most recent ZIP file in the current directory
most_recent_zip = find_most_recent_zip('.')
# If a ZIP file was found, list its contents
if most_recent_zip:
    data = data_from_zip_file(most_recent_zip)
    md = raw_bookmarks_to_md(data["raw_bookmarks"])
    print(md)
else:
    print("No ZIP files found in the current directory.")
```

After I run that script, I just copy-paste the Markdown output into the /bookmarks page in my website’s files. I’m using Hugo to build my website, so it’s just a plain Markdown file that I paste the output into.

Finally, I build the Hugo site, push it up to GitHub, and GitHub pages handles deploying the latest version of the site.

It’s simple, easy, scalable… and it just works. I'll repeat this roughly once-per-week to keep the links updated.

One extra step I take at the end is I dump all of the links into a local [ArchiveBox](https://archivebox.io/) instance. I have the config file set to only download: favicons, single files, screenshots and the title. I found this was a good balance between saving space and preserving the sites in a futureproof manner.

```
# ArchiveBox.conf
[SERVER_CONFIG]
SECRET_KEY = 1K7hRg0vFdNNQ6KK5lPSqOJdOZGVUPA40d1sxOHmGmFYEXqviK

TIMEOUT=120

SAVE_WGET=False
SAVE_WARC=False
SAVE_PDF=False
SAVE_DOM=False
SAVE_READABILITY=False
SAVE_GIT=False
SAVE_MEDIA=False
SUBMIT_ARCHIVE_DOT_ORG=False
SAVE_MERCURY=False
SAVE_HTMLTOTEXT=False
```

Of course there are a lot of improvements I could do. I could automate the downloading of the Notion dump, the running of the Python script, the outputting into the Markdown file, the committing and pushing up to GitHub, the ArchiveBox archiving… For now though I’m content with this system as a v0.1. I’m also OK with using Notion since it allows downloading all of your data in non-proprietary formats… meaning that lock-in and potential switching costs aren't issues.
