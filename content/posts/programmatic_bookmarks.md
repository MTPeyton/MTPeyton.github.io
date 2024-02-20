+++
title = 'How I Programmatically Bookmark, Archive and Share Websites'
date = 2024-02-19T22:31:19-08:00
date_modified = 2024-02-19T22:31:19-08:00
+++

DRAFT

# How I Programmatically Bookmark, Archive and Share Websites

For the last decade plus, I’ve been tossing links to interesting pages in all sorts of random places. Firefox bookmarks, Apple Notes, Apple Reminders, Markdown files, emails to myself…

I figured it was finally time to organize all of that. Plus I’ve also wanted to add more content to my personal website for awhile. With my bookmarked links now organized in a single place, it would be easy to auto-generate my ____link to bookmarks page____ page.

To organize the links, I first dumped all of them into a single Notion database. I chose Notion because it’s an easy way to setup a database, for free, with a custom schema that can have content added via a browser extension (Notion web clipper) and the iOS share sheet. Each entry included its URL, title a category, and notes. The title was auto-grabbed from the webpage by Notion’s web clipper. The category and notes (if any) were filled in manually by me.

Each category is a single-select column of the form “Software/Databases/PostgreSQL”. That way I could easily nest categories, while still pre-defining which categories are available.

From there, I downloaded a backup dump of my Notion workspace. This is something I already do weekly for backup purposes. (Hopefully I’ll fully automate it soon!)

I wrote a Python script (with some help from Phind) that opens the ZIP file, find the Bookmarks database’s CSV, and converts the entries to a Markdown page. It’s even able to parse the nested categories and output nice header elements for them.

<attach code>

After I run that script, I just copy-paste the Markdown output into the /bookmarks page in my website’s files. I’m using Hugo to build my website, so it’s just a plain Markdown file that I paste the output into.

Finally, I build the Hugo site, push it up to GitHub, and GitHub pages handles deploying the latest version of the site.

It’s simple, easy, scalable… and it just works.

One extra step I take at the end is I dump all of the links into a local ArchiveBox instance. I have the config file set to only download: favicons, single files, screenshots and the title. I found this was a good balance between saving space and preserving the sites in a futureproof manner.

<attach config>

Of course there are a lot of improvements I could do. I could automate the downloading of the Notion dump, the running of the Python script, the outputting into the Markdown file, the committing and pushing up to GitHub, the ArchiveBox archiving… For now though I’m content with this system as a v0.1. I’m also OK with using Notion since it allows downloading all of your data in non-proprietary formats… meaning the lock-in and potential switching costs are low.
