---
title: "Pop quiz: when will a filesystem copy be an incomplete backup?"
date: "2010-09-25"
url: /blog/2010/09/25/pop-quiz-when-will-a-filesystem-copy-be-an-incomplete-backup/
categories:
  - Databases
---
Let's suppose that your backup process looks like this: you stop a replication replica, shut down MySQL, and copy away the data directory. Assume that the replica is perfect and has the same data as the master. Nothing is broken, nothing is wrong, everything is working fine. In most cases, this should work, right?

Under what kinds of circumstances will you **not** get all your data back if you restore the file copy and start MySQL?


