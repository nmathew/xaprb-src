---
title: Aspersa, a new opensource toolkit
date: "2010-04-21"
url: /blog/2010/04/21/aspersa-a-new-opensource-toolkit/
categories:
  - Databases
  - Open Source
  - Operations
---
Some of the utilities we were adding to [Maatkit](http://www.maatkit.org/) really did not belong there. Yes, this included some of the functionality in the now-retired mk-audit tool. We really learned a lesson about what it's possible to support, design, spec, code, and test in a single tool.

I've moved those tools to a new project, [Aspersa](http://code.google.com/p/aspersa/). Some folks are revolting and calling it Asparagus, because apparently that's easier to say. Aspersa is the name of the common garden snail, which turns out to be a fascinating creature. It is also slow. Draw your own conclusions.

This project is more of a home for simple scripts and snippets---a simple place I can grab all the little utilities I use to make my life easy. There is a "summary" tool that largely replaces mk-audit's functionality outside the database, and I plan to add a "mysql-summary" tool to summarize the database.

I don't plan to make "releases." You get the tools with wget directly from SVN trunk. There is no separate website, and little to no documentation, but there is a mailing list, and you're invited to join and contribute.


