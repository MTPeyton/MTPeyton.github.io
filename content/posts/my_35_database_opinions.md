+++
title = 'My 35 Database Opinions'
date = 2024-03-11T23:11:22-07:00
date_modified = 2024-03-11T23:11:22-07:00
+++

> “That’s just, like, your opinion, man.” - The Dude

Like all opinions, some of these might be wrong, and some might be right. Some might only apply in certain circumstances. But these are the opinions I’ve formed over the last 5+ years on what databases to use, and how to use them.

1. Pick relational databases by default. “NoSQL” means “another one-off query language to learn”.
2. Only use alternative databases (MongoDB, Redis, Cassandra, etc.) if you REALLY REALLY need to.
3. If your data is even SLIGHTLY relational, don’t use MongoDB. Aggregation pipeline lookups don’t scale well.
4. “Schemaless” means that your schema is diffused throughout your codebase.
5. Schemas can be trivially handled with a migration tool. (I use Alembic or Prisma)
6. Keep your schemas aggressively normalized. De-normalized data is a nightmare to keep up-to-date.
7. There’s almost always other ways to gain performance before even considering de-normalizing.
8. Plan out your schemas ahead of time. (Use your migration tool!)
9. Plan out which columns will be indexes ahead of time!
10. Ensure your indexes get created. Periodically audit your indexes and add/remove them if needed.
11. Don’t bother with compound indexes unless you have VERY specific queries that need them.
12. EAV is an anti-pattern… but can sometimes be useful if used SPARINGLY.
13. Use UUIDs instead of auto-incrementing integers for primary keys. You gain globally unique IDs, and hide your DB’s size, for not many downsides.
14. Use foreign key constraints! They check yourself before you wreck yourself!
15. [Dealing with time is tricky and has lots of hidden ‘gotchas’.](https://gist.github.com/timvisee/fcda9bbdff88d45cc9061606b4b923ca)
16. ALWAYS store time as an ISO-8601 timestamp (or your DB’s equivalent timestamp type) in the UTC timezone.
17. [Storing peoples' names is also very tricky.](https://www.kalzumeus.com/2010/06/17/falsehoods-programmers-believe-about-names/) I just stick to a single field of max 128 Unicode characters.
18. Try to have rows represent a single physical object, or an action/event that occurred at a singular point in time.
19. Try to keep rows mostly immutable. Things about the row that frequently change should probably be modeled as appends to a separate table.
12. Make relationships many-to-many by default unless you have a good reason otherwise.
21. Your many-to-many relationship tables should store metadata about the relationship. e.g. Users-to-Companies table that also store’s each employee’s job title at each company.
22. Hard delete things. [Soft-deleting can lead to problems.](https://blog.bemi.io/soft-deleting-chaos/) Keep an audit log of all changes (creates/updates/deletes) that lets you "undo" if needed.
23. If data needs to be redacted, just overwrite individual columns with “REDACTED”, rather than deleting the row.
24. Use SQLite for local, single-user applications.
25. Use PostgreSQL for remote and multi-user applications.
26. Don’t use your Postgres superuser for normal DB connections. Create other users and assign them permissions.
27. Consider partitioning Postgres tables once they reach approx 100 million rows.
28. Choosing a partition key is hard. Usually it’s the least worst option.
29. Choose partition key(s) that aligns with your most common WHERE clauses.
30. Choose partition key(s) that will lead to partitions roughly balanced in both size and load.
31. Your codebase should gracefully handle ANY possible state that’s representable in your DB’s schema. If an engineer manually changes any cell’s value in the DB, it should not blow up your codebase.
32. Try to make analytical OLAP queries work using your OLTP database.
33. If they’re just too much for your OLTP database, even after you’ve done every optimization you can, only then introduce an OLAP Data Warehouse.
34. Don’t write your own ETL code for shipping data from your database to your Data Warehouse. USE SOMETHING PREBUILT!
35. Be very cautious when querying your Data Warehouse. Most (e.g. Athena, BigQuery) are powerful. But you can accidentally rack up HUGE bills.
