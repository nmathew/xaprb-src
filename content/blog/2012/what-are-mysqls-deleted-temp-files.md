---
title: "What are MySQL's deleted temp files?"
date: "2012-09-12"
url: /blog/2012/09/12/what-are-mysqls-deleted-temp-files/
categories:
  - Databases
---
If you've ever looked at the `lsof` or listing of `/proc/$pid/fd` for a running MySQL server, you've probably seen files like these:

    # ls -l /proc/$(pidof mysqld)/fd/* | grep tmp
    lrwx------ 1 root root 64 Sep 12 10:21 /proc/17222/fd/18 -> /var/lib/mysql/tmp/ibDOy0eD (deleted)
    lrwx------ 1 root root 64 Sep 12 10:21 /proc/17222/fd/323 -> /var/lib/mysql/tmp/MLhfWsbz (deleted)
    lrwx------ 1 root root 64 Sep 12 10:21 /proc/17222/fd/6 -> /var/lib/mysql/tmp/ib65H6A5 (deleted)
    lrwx------ 1 root root 64 Sep 12 10:21 /proc/17222/fd/7 -> /var/lib/mysql/tmp/ibllu2yi (deleted)
    lrwx------ 1 root root 64 Sep 12 10:21 /proc/17222/fd/8 -> /var/lib/mysql/tmp/ib9yRYwv (deleted)
    lrwx------ 1 root root 64 Sep 12 10:21 /proc/17222/fd/9 -> /var/lib/mysql/tmp/ibhUCeRO (deleted)
    

What are those? It's not hard to find out, actually. Just open them and look at them! The `ib*` files are InnoDB's temporary files, and the `ML*` (and sometimes `MY*`) files are binary log cache files.

I'm not sure if the file descriptor numbers are always guaranteed to be the same for the `ib*` files, but on the server I'm looking at right now, they are as follows:

6 = InnoDB's status monitor---the same thing you see in `SHOW ENGINE INNODB STATUS`.

7 =  What appears to be an InnoDB cache file for some foreign key definition statements---perhaps the most recent foreign keys that InnoDB has parsed and created? That's just a guess.

8 = An empty file.

9 = The most recent InnoDB deadlock.

18 =  An empty file.

Does anyone else have more information to add?


