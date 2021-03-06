---
title: Introducing MySQL Replica Delay
date: "2007-08-04"
url: /blog/2007/08/04/introducing-mysql-slave-delay/
categories:
  - Databases
---

MySQL Replica Delay implements an oft-requested replication feature: the ability to make a replica lag its master, a.k.a scheduling binlog events for some time in the future. This will probably be [built into MySQL replication someday](http://bugs.mysql.com/bug.php?id=28760), but in the meantime, you can use this tool. A delayed replica is great if disaster strikes and propagates through replication, because it lets you stop the delayed replica before it gets corrupted, restart the replica until just before the offending statement, and recover quickly. This is a lot faster than restoring last night's full backup and replaying a day's worth of binary logs on the master.

This is the first tool largely contributed by someone else. Sergey Zhuravlev sent me the original program over email, and I tweaked it to watch the replica's relay logs so it doesn't need to connect to the master, added documentation, and released it.

### What it does

MySQL Replica Delay watches the replica's relay log positions and makes the SQL thread lag the I/O thread by starting and stopping it as needed. It can also watch the master's log positions, but this is usually not needed unless the replica's I/O thread delay is large (typically it's just milliseconds, but it depends on the network speed).

Since you can't tell how far behind the replica is unless it's running, MySQL Replica Delay samples and remembers the binlog positions. Later, when it needs to start the replica, it recalls the master's position at the desired time ago, and makes the replica run until it hits that position.

Another way to think about it is that it schedules replication events for sometime in the future, instead of letting them be applied immediately.

All in all, a simple and easy way to accomplish delayed replication.

### About MySQL Toolkit

[MySQL Toolkit](http://code.google.com/p/maatkit) is a set of essential tools for MySQL users, developers and administrators. The project's goal is to make high-quality command-line tools that follow the UNIX philosophy of doing one thing and doing it well. They are designed for scriptability and ease of processing with standard command-line utilities such as `awk` and `sed`.


