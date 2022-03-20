---
layout: post
title: Database Migrations - Lessons from Failure, and Tips for success
date: 2013-12-12 17:09:00.000000000 -08:00
categories: []
tags: []
status: publish
type: post
published: true
author: Mike Seid
---
**Late on sunday night, we tried to do a large database migration and failed due to migration performance, forcing a rollback.**

While this was wildly disappointing, we learned a lot about database migrations procedures, MySQL, and some good tips when working with any database. After prepping as much as we could, we had still had some things blind side us. Here are 5 tips to ensure a smooth migration, with minimum downtime. **pro-tip: number 5 was really the problem**

A little history: The tables in question were at least 4 years old at this point, and they needed to be refactored for both speed optimizations, and new uses of how we are using the data. After running the migration multiple times in our staging environments, we had a stable script were ready to go.

### Four Tips for Easier Migrations

1.  **Make sure staging/production hardware and software in in parity**  
     This one seems pretty simple and obvious, but it something that bit us pretty hard. We follow some good practices here, so it didn't dawn on us that we had different MySQL versions. Well, we did, and it didn't help.

2.  **Replication slows things down**  
     A jump off of number 1, we didn't account for replication and how it would impact the migrations. Even though replication on master/slave is asynchronous, it still has an impact on CPU usage and disk usage. Especially for a write heavy migration, it really impacted us a lot.

3.  **Have quick status checks, and take benchmarks**  
     After we wrote our migrations, we dumped them all into one large script to run. While this was easier for run on migration night, it was a kind of set it and forget it process. We had no idea what was going on under the hood, all we did know was that the whole process should take an hour. An hour and half after we kicked off the process, we had to poke around and see how far the migration had run. After checking, we saw it was only a quarter of the way done. Needless to say, action needed to be taken.

    I would recommend doing each one manually, and make sure that your time benchmarks are similar to what you expect. Start with small scripts that should only take a few minutes and see how long they take. If they take 5 minutes, you know there is a problem and can abort/fix it earlier.

4.  **Provide ample disk space**  
     We had some large complex queries which required a lot of data to be held in memory. Since RAM can only be so large, it used /tmp hard disk memory to store temporary tables and other information. Having limited space for these caches slow down queries, or even worse, breaks it.

5.  **Check to make sure OS fsync isn't enabled**  
     Here was the real bottle neck during our migrations. To our surprise, it wasn't a mysql setting, but an OS level setting. According to the manual:

    > fsync() transfers ("flushes") all modified in-core data of (i.e., modified buffer cache pages for) the file referred to by the file descriptor fd to the disk device (or other permanent storage device) where that file resides. The call blocks until the device reports that the transfer has completed. It also flushes metadata information associated with the file (see stat(2)).

    At the OS level, we configured synchronous writes, as this would ensure no data was lost in the buffer in the event of a power lose. While this safeguards us in production, it is a serious handcuff during migration. After every write to our database, we had to wait for the disk to catch up.

After a long night, and a stressful rollback, we finally got up and running again. I wouldn't quite say we failed, as we learned some valuable lessons. Hope you don't run into the same problems, and can have a relaxing weekend watching your databases get upgraded.