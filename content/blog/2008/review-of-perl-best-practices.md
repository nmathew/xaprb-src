---
title: Review of Perl Best Practices
date: "2008-04-01"
url: /blog/2008/04/01/review-of-perl-best-practices/
categories:
  - Reviews
  - Programming
---

In my opinion, every Perl programmer needs at least these two books: the [Camel]({{< amz 0596000278 >}}), and the [Dog (Perl Best Practices)]({{< amz B002L4EXI8 >}})

[![Perl Best Practices](/media/book-covers/perl-best-practices.jpg# 3dbook)]({{< amz B002L4EXI8 >}})

The Camel teaches you how to program Perl: the syntax and so on. But the Dog teaches you how to program Perl sanely, by recommending that you use only a subset of Perl's syntax and abilities.

The Camel tells you that there's more than one way to do it, which is supposed to be a strength of Perl. The Dog tells you which way is "best." And it tells you why the other ways are worth avoiding, and in which circumstances, and how badly you can get burned if you're not careful.

The Dog also shows you a lot of ways to do things that you might not otherwise think about. In that sense, it's not just stripping away. It's also adding things that you might not get elsewhere. Example: you might not know about [List::Util](http://perldoc.perl.org/List/Util.html).

The common complaint about Perl being a write-only language doesn't have to be true if you follow the Dog's suggestions. Perl can be a very readable language---and I mean readable for ordinary humans, so don't think I'm just another Perl programmer insisting that something is readable when it looks like static to you. The Dog encourages readable coding. In fact, in the absence of any other coding standard, if you try to "follow the Dog" you'll be doing a good job. (There's really no need to invent your own coding standard, in my opinion. Just follow the Dog.)

It's not perfect. I don't always agree with its suggestions. For example, I think the use of "magic comments" to automagically add interactive progress bars to command-line programs is just a bad idea. (Comments should **never** change the behavior of a program). However, the vast majority of its suggestions have come from long experience. If you don't agree with one of them, it's worth following it anyway until you consider yourself expert enough to disagree with authority.

I consider this an essential Perl book. No Perl programmer should be without it.


