+++
title = 'Why I Switched From Local Plaintext Accounting to SaaS'
date = 2024-02-25T23:08:10-08:00
date_modified = 2024-02-25T23:08:10-08:00
+++

Before 2017, I was a user of [Mint](https://mint.intuit.com/) for tracking my personal finances. However, around that time, I realized that it didn't offer me much functionality. Worse, it was storing my banking and investment account credentials in a way that allowed server-side decryption. (In order to do browser automation and screen scraping of financial websites) So I decided to switch to something else.

2017 was also the year when I decided to start using more Free and Open Source Software in my life. I did some research on my options, and narrowed it down to two choices: [GnuCash](https://gnucash.org/) and [beancount](https://github.com/beancount/beancount). I eventually ruled out GnuCash because of its somewhat-archaic GUI. On the other hand, beancount is essentially a Python library and CLI tool that can read `.beancount` files. These files are plaintext files, where each line is a declarative statement regarding your personal financial ledger, hence the phrase ["Plaintext Accounting"](https://plaintextaccounting.org/). The specific beancount syntax is a spinoff of [ledger's](https://ledger-cli.org/) syntax: and it leads to a robust [double-entry accounting](https://en.wikipedia.org/wiki/Double-entry_bookkeeping) system.

I wrote several Python scripts that would take downloaded copies of my financial data (in CSV form) and convert them to beancount syntax. I would then append that output to my master beancount file. I could then run beancount's CLI tools to query my financial history... as well as use [fava](https://beancount.github.io/fava/) (a locally-runnable web-based GUI) to view charts of my data. I scheduled the 5th of every month to be my "finances day" where I would login to my financial services, download my data for the previous month, run my Python scripts on it, and view the results. I tracked things such as: expenses, net worth, investment performance, and more with this setup.

Each month, in addition to running the scripts, I would also make improvements to them. I wrote automatic expense categorizers, automatic balance assertions, automatic PDF data extraction for my paystubs, automatic stock and cryptocurrency historical price fetchers, etc.

I did all this like clockwork, once a month, every month, for 6 years.

Over those 6 years, a lot changed with my financial situation. I got an internship while I was a student. I graduated and got a job. I switched jobs a couple of times. I opened and closed investment accounts. I moved 401ks between employers. It was tough keeping my code up-to-date with my life. When I switched employers, I had to rewrite my paystub data extractors. I was never able to pull data from some 401k providers: so I manually entered it. My bank and investment broker changed their file formats several times, leading to script reworks. Keeping stock prices up-to-date was a huge pain, especially when trying to do it without paying for a stock data API. On and on... Some of these changes I were able to keep up with... others got filed as TODOs that never got done.

There were also issues with my homegrown system that had always been there since the start. Tracking cash was a pain. I would just write a note on my phone, and then manually dump those into my beancount file every month. Small errors would also accumulate and diverge the balance of my accounts in my ledger from their true balance. Automatic balance assertions helped identify this, but I was always playing whack-a-mole trying to revise entries to fix balance accumulations. I guess issues like this are present in ANY double-entry accounting system... but at the end of the day it just took too much time for me to deal with.

That's the overarching theme of my issues with my system: **it took way too much time.** After a few years, I began to DREAD the 5th of each month. It would always take me at least half a day to get everything done, and I would always end with more TODOs than I started with. Work on it began to bleed over into other days. At first it was a fun side project... but after 6 years it had turned from fun hobby to exhausting chore.

I started a new job in January 2024, and with that came the need to update a LARGE chunk of my code. Paystubs, 401ks, etc. Given that, plus all the other things piling up, I made the decision that this wasn't a project I wanted to spend any more time on. At this point, the benefits of local-first plaintext accounting were outweighed by the time investment required. I decided to look into a service that would do all this work for me.

After looking at several different options (Mint again, GnuCash again, [Firefly III](https://www.firefly-iii.org/), [YouNeedABudget](https://www.ynab.com/), [Personal Capital](personalcapital.com), [Beancount.io](https://beancount.io/)) I ultimately went with Monarch Money. The other options were either shutting down (Mint), destined to have the same problems (GnuCash, Firefly III, Beancount.io) or just not the right all-in-one app (YouNeedABudget, Personal Capital).

As it turns out, [Monarch was co-founded by a PM at Mint.](https://www.businessinsider.com/personal-finance/mint-users-switch-to-budgeting-app-monarch-money-2023-12) I'm guessing this is one of the reasons it's so solid. It takes everything Mint did, and does it WAY smoother and WAY more reliably. I'm not going to go deep on the specifics, but [you can read their website for a list of their features.](https://www.monarchmoney.com/features) It also solves essentially all of the problems I had with my homegrown solution. It directly syncs with my accounts in a secure way, it stays up-to-date hourly instead of monthly, it handles investments/budgets/balances/expenses super well, it has a mobile app I can log cash transactions to.

Most importantly, **it saves me time.** Now on the 5th of each month, I do about 20 minutes of expense categorization (confirming their automatic suggestions), and that's it. I'm done. I can even do it on the bus or the toilet.

When I first setup my beancount+fava+Python scripts solution, I knew for sure that I was saving myself money as well as building a secure, private and futureproof system. However, I totally failed to realize how much I was paying in time to gain all of that. Those things are still important to me, but my time trumps all of them. I was lucky to be able to find a new solution that saves me time while also (mostly) achieving the other things.

Secure? Yeah, reasonably so. It uses Plaid for financial connections, which while not perfect, is good enough for me.

Private? Eh... probably? They claim to never sell user data, as it's subscription based. I'm inclined to believe them. Even if they do eventually sell my data, I won't care too much, because all of that data is already being sold by my financial institutions anyways.

Futureproof? Maybe. Fortunately though, I can export all of my data to CSVs. Which I do, once a month, just to insure myself against them dissapearing for whatever reason. Worst case I can just (begrudingly) write code that converts those CSVs to beancount, and pick up with my old system where I left off.

So there it is... the culmination of 6 years of tracking my finances. It was a bumpy road, and I do somewhat regret not sticking with a Free-and-Open-Source solution... but ultimately I'm very happy to now have an extra free day or two every month!

*(Also, I have a referral link to Monarch [here](https://www.monarchmoney.com/referral/1o7b8u5y8f). If you want a non-referral link, you can click [here](https://www.monarchmoney.com/) instead.)*