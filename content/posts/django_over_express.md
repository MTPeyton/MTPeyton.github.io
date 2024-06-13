+++
title = 'I Still Prefer TS over Python… But also Django over Express.'
date = 2024-06-12T22:40:03-07:00
date_modified = 2024-06-12T22:40:03-07:00
+++

In a previous post I wrote about how [I’ve been reaching for Typescript over Python lately](https://mpeyton.com/posts/typescript_over_python/). I still prefer Typescript over Python, for all of the reasons mentioned in that post. I think its typing system is better, it runs faster, the package management is (somewhat) less bad, etc. However, I’ve realized there’s a notably exception that’s led me to still pick Python over Typescript for some of my latest side projects.

**There’s no equivalent to Django in Typescript… and Django saves me a LOT of time.**

I’ve realized (through trial and error) that I just don’t have enough time to reinvent the wheel when working on a side project. I need to build off of something that comes with 90% of what I need prebuilt, and has community plugins for the next 9% of what I need. Then I can focus on just the last 1% that I ACTUALLY need to write from scratch.

The fact that Django handles things like my database schema, migrations, ORM, forms, auth, server-side rendering, admin site, and more is indispensable. The fact that it takes care of a lot of common security issues (CSRF, SQL/HTML/JS injection, etc.) is even better. It also has SO MANY community plugins for doing common things. (e.g. [django-pivot](https://github.com/martsberger/django-pivot) for creating histograms or [django-import-export](https://github.com/django-import-export/django-import-export) for importing CSV data to my database)

Sure, Javascript/Typescript has an untold number of NPM packages that could do each of these things. However, it makes a huge difference to have these features not just pre-built, but already wired together for you. I found the majority of the time I spent working on Typescript side projects was on writing glue code between different modules. Writing code that could connect Express with Prisma with Nunjucks… writing custom form handling code… probably not handling certain security practices in the process… With Django all of this works out-of-the-box (or at least with minimal configuration), INCLUDING when I add extra community plugins.

There are still many things I’d change about Django though. Some of its configuration and defaults are STILL a bit too heavy handed in my opinion. Trying to go off the script and do certain things (e.g. using just emails instead of usernames and emails for users) is frustrating. (Whereas it’s easy to do with something like Express or Flask + your ORM of choice)

Above all, I wish there was a Django equivalent built in Typescript. I really want all of the pre-built goodies that Django has, with the performance + great typing system of Typescript. I know there are some contenders ([NestJS](https://nestjs.com/) and [TEO](https://teodev.io/) among others…) but it seems like nothing has truly hit the mark and gained widespread adoption yet.

So for now, I think I’ll stick with Python… but only because it has Django. Also, only for side projects. For any sort of large project, like a true startup or company, I’d probably pick Typescript. (And some combination of Express + Next.js) Fortunately, my day job is mostly Typescript! (Although I DO use some Python as well for computer vision stuff 🤷‍♂️)