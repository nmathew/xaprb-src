---
credit: http://www.flickr.com/photos/dwysiu/4985831624/
image: /media/2013/08/4985831624_fc6cc446c9.jpg
title: "Speaking at Strata NYC: Making Big Data Small"
date: "2013-08-18"
url: /blog/2013/08/18/speaking-at-strata-nyc-making-big-data-small/
categories:
  - Conferences
---

I'm off to my first [Strata](http://strataconf.com/stratany2013/) conference, and I'm speaking! I've always wanted to attend Strata. (OSCON too, but I haven't yet made it there.)



My session will be about [ways to make big data small](http://strataconf.com/stratany2013/public/schedule/detail/31435), in both the storage and processing dimensions, without losing much of the value. 

<!--more-->

If you're familiar with Bloom Filters, this is an example. Bloom Filters let you answer the question, 

> Is value X a member of this data set? Yes, or no?

by substituting the question, 

> Is value X a member of this data set? Probably yes, or definitely no?

You lose a small and quantifiable amount of precision in the "yes," and you gain massive savings in storage and processing cost. Bloom Filters are typically used when you need a definite answer, but only as a pre-filtering step, because if the answer happens to be No, you save the effort of looking through the set to try to find your data. 

That worldview or philosophy is a valuable thing to keep in your pocket when you're working with large amounts of data, and that's the topic of my Strata Conference / Hadoop World NYC 2013 talk. 




