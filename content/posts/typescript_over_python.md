+++
title = 'I’ve Been Reaching For Typescript Over Python Lately'
date = 2024-04-15T22:46:29-07:00
date_modified = 2024-04-15T22:46:29-07:00
+++

Lately, I’ve found myself reaching for Typescript more than I reach for Python. It’s not that I dislike Python… it’s just that Typescript has felt more pleasant to work with lately.

I think it boils down to a few key things.

First is the typing system. It feels like the perfect in-between of Java’s types and Python’s type hints. I never feel like I’m fighting it, and type inferences work well. The fact that JSON, which I end up using almost everywhere, is a first class citizen is the cherry on top.

Second is environments and the package manager. After many years of setting up Python on many types of machines… I feel confident in my ability to use some combination of pip / pyenv / virtualenv / poetry to manage Python environments. However, that doesn’t change the fact that I ALWAYS run into issues when seting up a new machine. [It’s not just me either.](https://news.ycombinator.com/item?id=40045318) Again, it’s not that I can’t fix the issues… I almost always can. It’s just that I got tired of dealing with those issues.

NPM for all its flaws is honestly pretty pleasant to use. I use NVM to set the version of Node I want, then `npm install` then `npm run whatever`. That’s it. I’ve never had to debug anything related to this. I think a lot of annoyance around NPM stems more from the culture of just installing packages willy-nilly to solve even trivial problems. (pip has a bit of that culture as well, but not to the same extend as npm due to Python’s better internal library) Either way, I’ve found that I don’t need to install that many packages to achieve what I want. Generally `express`, `nunjucks`, and `prisma/drizzle` are all I need. Even Javascript's built-in `async` is very comfortable to use.

Third is speed. Typescript (AKA the Javascript it’s transpiled to) isn’t the fastest language… but it’s WAY faster than Python. For some rough comparisons, look at [here](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/javascript.html) compared to [here](https://benchmarksgame-team.pages.debian.net/benchmarksgame/fastest/python.html). It might not seem like this makes a difference in the real world, BUT it was the difference between me being able to run an Express app on Digitalocean’s cheapest app platform plan… and being unable to run an equivalent Flask app on that same plan.

Again, I still use Python for certain one-off scripts on machines where I already have pyenv/virtualenv setup perfectly. And I would be totally fine jumping in to work on a pre-existing Django or Flask app’s codebase. But for now, all of my side projects will be built with Typescript.