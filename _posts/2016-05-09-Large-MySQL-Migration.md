---
title: Large MySQL Table Migrations
updated: 2016-05-09
---

## Problem

Suppose you have a really large MySQL table, like millions of rows, and you need to make an ```ALTER``` statement that effects all the rows, such as:

```
ALTER TABLE `table_name` ADD COLUMN `new_column_name` varchar(255) NULL DEFAULT NULL;
```

And also suppose that this table is being actively queried, what should you be worried about?

Under the hood MySQL will actually create a temporary table, copy everything over to the temp table, and do some renaming operations to point everything to the newly created table. The copy operation is costly, both in I/O and time. When the table is really large, this can take minutes and hours. This operation will also cause update requests to be put on hold. And it's likely that whatever server framework you're running will probably run out of maximum concurrent connections before the query is complete, therefore causing the service to break.

## Work Around 1

Use tools! There are tools that helps with exactly these sort of large migrations, but they're all doing essentially the same thing:
1. Create a new table
1. Create trigger that copies new entries from the existing table to the new table
1. Slowly backfill the existing entries to the new table
1. Once the backfill is complete, rename the new table to the old table
1. Success

Of course there are some error checking to ensure that no entries are lost, duplicated, or altered. 

## Work Around 2

Use a seperate table! Create another table that just uses the primary/foreign key relationship to link the new columns of the new table to the entries in the existing table. And use the same backfill strategy as work around 1.

If the backfilling operation happens all at once or too quickly, you can make queries that immediately follow slower than usual because you will have force cleared the cache with the backfill operations. 