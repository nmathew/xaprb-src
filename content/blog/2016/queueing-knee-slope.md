---
title: "The Queueing Knee, Part 2"
date: 2016-12-04T20:53:00-05:00
url: /blog/queueing-knee-slope/
description: "Part 2: what other definitions of queueing knee are there?"
image: /media/2016/11/unsplash-photo-SwIQu6xODrI.jpg
thumbnail: /media/2016/11/unsplash-photo-SwIQu6xODrI.tn-500x500.jpg
credit: https://unsplash.com/photos/SwIQu6xODrI
categories:
- Scalability
- Math
---

Last week I wrote about the so-called ["knee" in the M/M/m queueing theory
response time curve](/blog/queueing-knee-tangent/). In that post I examined one
definition of the knee; here is my analysis of the others, including the idea
that there is no such thing as the knee.

<!--more-->

There are potentially several ways to think about the "knee" in the queueing
curve. In the previous post I dug into Cary Millsap's definition: the knee is
the point where a line tangent to the queueing curve passes through the origin:

![knee](/media/2016/11/knee-1.png)

Here are a few others to consider:

1. The knee is where the curve has a slope of 1, that is, it's growing as fast
	upwards as it is rightwards.
2. The knee is the utilization at which you can expect requests to spend as much
	time waiting as being serviced, i.e. {{< math >}}2S{{< /math >}}.
3. The knee is related to tail latency, for example where the 99th percentile of
	the latency is double the median.
4. The knee is where the curve's derivative starts to change quickly, i.e. the
	wait time changes greatly with only a small change in utilization.

Let's look at each of these in turn.

### The Curve Has Slope=1

The first definition is the point at which the queueing curve has a slope of 1,
at which point response times are increasing directly proportional to
utilization {{< math >}}\rho{{< /math >}}. Past this point, response times increase faster than
utilization, so this is a reasonable point to say the system "becomes
superlinear" in the sense that linearity is where slope is 1.

Using the heuristic formula for the queueing theory response time curve, which
again is exact for {{< math >}}m=1{{< /math >}} and {{< math >}}m=2{{< /math >}} service channels, and slightly
underestimates qeueuing wait time for greater values of {{< math >}}m{{< /math >}} but still serves
as a reasonable approximation, we proceed as follows.

{{< math >}}
R = \frac{1}{1-\rho^m}
{{< /math >}}

The derivative of that equation is

{{< math >}}
\frac{d}{d\rho} R = \frac{m \rho^{m-1}}{(1-\rho^m)^2}
{{< /math >}}

So the solution for {{< math >}}\rho{{< /math >}} as a function of {{< math >}}m{{< /math >}} is the real root
between 0 and 1 of

{{< math >}}
\frac{m \rho^{m-1}}{(1-\rho^m)^2} = 1
{{< /math >}}

Solving this for {{< math >}}\rho{{< /math >}} turned out to be an interesting challenge which I
did not complete. Among other things, this equation has very different behavior
at {{< math >}}m=1{{< /math >}}. *In a single-server queueing system, the "knee" where the slope is
1 is at utilization of 0*.

If this is a surprise to you, take another look at the queueing
"hockey stick" curve. You'll usually see it plotted with a stretched
horizontal axis, because {{< math >}}0<\rho<1{{< /math >}}, and compressed vertical axis. That
fools your eye! But when [plotted](https://www.desmos.com/calculator/0y4tillfys)
without distortion, it's quite clear that the queueing curve is on a diagonal
right away when {{< math >}}m=1{{< /math >}}.  Here's the queuing curve for 1, 2, and 8 servers
(red, blue, and green lines).

![Queuing Delay Plotted With Square Aspect Ratio](/media/2016/12/knee-1.png)

For {{< math >}}m=2{{< /math >}} and greater, the solution is a lot of algebra. I resorted to using
Wolfram Alpha. For 2 service channels, the equation reduces to

{{< math >}}
\frac{2\rho}{(\rho^2-1)^2} = 1
{{< /math >}}

The solution of this is

{{< math >}}
\rho = \frac{1}{2} \left( \sqrt{2} \sqrt{\lambda+2} + \sqrt{2} \sqrt{ \frac{ -2 \lambda - \lambda^2 + \sqrt{2} \sqrt{\lambda+2}}{\lambda+2}} \right)
{{< /math >}}

where

{{< math >}}
\lambda = \frac{1}{6} \left( -8 + \sqrt[3]{118-6\sqrt{273}} + \sqrt[3]{2(59+3\sqrt{273})}\right)
{{< /math >}}

Or {{< math >}}\rho \approx 0.371507 {{< /math >}}. This gets worse for {{< math >}}m=3{{< /math >}} and above, and
there's no single solution for all {{< math >}}m{{< /math >}}. It's not too bad to plot, though;
here's an interactive [Desmos
calculator](https://www.desmos.com/calculator/yqdi14cruv) where you can see the
queueing curve in blue, and its derivative in orange, as you vary {{< math >}}m{{< /math >}}. Where
the derivative crosses the line {{< math >}}y=1{{< /math >}} is the "knee" by the definition I'm
using in this section.

![Knee Defined As Derivative=1](/media/2016/12/derivative-y-1.png)

The behavior, although I can't plot it with an equation, is not different from
what you've seen previously: as you increase the number of service channels, the
knee moves to the right. Notice how the orange line jumps when you set
{{< math >}}m=1{{< /math >}}.

I'd conclude this section by saying that I think this isn't the right direction
to take this discussion; the "knee" is supposed to be a kind of Guideline.
It's the point above which performance becomes "bad" by some definition.
Clearly, "performance is bad above utilization of 0" isn't useful.

### Requests Spend As Much Time Waiting As In Service

The second definition is simply the point where {{< math >}}R=2S{{< /math >}}, or {{< math >}}R/S=1{{< /math >}}; the
solution of

{{< math >}}
\frac{1}{1-\rho^m} = 2
{{< /math >}}

The solution in terms of {{< math >}}\rho{{< /math >}} turns out to be not too difficult:

{{< math >}}
\rho = 2^{-1/m}
{{< /math >}}

Which is not too different, in fact, than the solution to Cary Millsap's
definition. Here are both
[plotted](https://www.desmos.com/calculator/dljpiank18) together, showing
similar behavior. Blue is "wait equals service time" and red is Cary Millsap's
definition.

![Knee Defined As Wait Equals Service Time](/media/2016/12/knee-2.png)

This definition is reasonable, in some sense, if you think "bad performance" is
roughly equivalent to "my wait is longer than my job, that's a waste." But it's
still arbitrary. (More on this later.)

### Define The Knee In Terms Of Tail Latency

As you can see, Cary Millsap's definition is much more pessimistic, and the
"wait equals service time" definition is more optimistic. But as I often say,
tail latency is much more important than average latency, and the M/M/m queueing
curve is in terms of averages. What if we define the knee as something like "the
utilization at which the 99th percentile latency is twice the average service
time?"

Although this would be possible to do, it would be tedious, because latency
quantile calculations for M/M/m queueing systems are a bit intricate.  Neil
Gunther's book delves into that a bit if you're curious. Solving for {{< math >}}\rho{{< /math >}}
when quantile latency equals some multiple of service time, as a function of the
number of service channels, would probably be harder than solving for the slope
of the queueing curve.

Note also that the behavior in general would remain the same as the curves
we've been seeing---as service channels increase, so too does the maximum
utilization the system can bear before causing bad performance.

### Define The Knee As Rapid Change In Residence Time

The final definition I proposed is that the knee might be where the curve's
derivative starts to change rapidly, showing that the wait time starts to be
very sensitive to changes in utilization. The change in the curve's derivative,
of course, is its second derivative. When does this become large?

To answer this formally, I'd have to define "large" and I'd also have to solve
the second derivative of the queueing curve for {{< math >}}\rho{{< /math >}} as a function of
{{< math >}}m{{< /math >}} as usual. Here's the second derivative:

{{< math >}}
-\frac{m\rho^{\left(m-2\right)}\left(m\rho^m+\rho^m+m-1\right)}{\left(\rho^m-1\right)^3}
{{< /math >}}

This looks hard to solve, and I'm not sure what value I'd pick for "large;" it
would necessarily be arbitrary. It turns out it is easy to avoid finding this
answer by simply [plotting](https://www.desmos.com/calculator/5acirbvfvt) the
derivatives and observing that they all increase monotonically as utilization
approaches 1. In other words, there's no "turning point" where you can point at
the graph and say "beyond this, things *really* get even more worser even more faster."

This supports Neil Gunther's view that [there is no queueing
knee](https://www.cmg.org/publications/measureit/2009-2/mit62/measureit-issue-7-08-mind-your-knees-and-queues/).
And if you really want to dig deeper into math and see even more potential
definitions of the queueing knee, you should read that article. It goes into
different types of considerations than I've done here.

### Conclusions

This two-part series was in some sense an effort to point out that there's no
specific definition of knee in the M/M/m response time curve. Why was this worth
doing? Put simply, because it continues to persist even after being analyzed and
discussed by many performance experts. Not long ago a self-styled performance
expert told me "as you know, past the knee at 75% utilization, the system starts
to perform badly" and I had to bite my tongue. (To rub salt into the wound, this
was a server with many CPU cores. What a waste.)

All knee definitions turn out to be arbitrary, *but* examining different
definitions, inasmuch as they lend themselves to the algebra required,
*consistently* points out that the "knee" grows larger as the number of service
channels increases. (The exact shape and rate of that growth varies, but it
always grows. This follows directly from Erlang's formulas.)

In the end, that principle is the only takeaway from this. There's no knee, but
the more CPUs (or servers) you have, the more fully utilized you should expect to
run that system. You're foolish to do otherwise.
