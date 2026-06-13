---
layout: post
title: "More on the Sad State of Checked Exceptions"
date: 2016-11-11
tags: [java, exceptions]
blogger_url: https://johnlonergan.blogspot.com/2016/11/more-on-sad-state-of-checked-exceptions_11.html
---

Following on from [my previous post](/blog/2016/11/11/sad-state-of-java-lambdas-and-checked-exceptions/) about Java Lambdas and checked exceptions, I did some more research.

There's a revealing analysis of what GitHub programmers actually do when handling exceptions: [Exception Handling Practices in Java](http://plg.uwaterloo.ca/~migod/846/current/projects/09-NakshatriHegdeThandra-report.pdf). It goes a long way to demonstrating the failure of Java's approach to checked exceptions.

Well worth a read. Most developers, it turns out, don't respect checked exceptions at all — they're either ignored entirely or met with nothing more than a log statement and a swallow.

Look at figures 7 and 8 in particular. A lot of catch blocks are entirely empty. But even the ones that do some printing or logging? When the researchers looked more closely, logging or printing was typically the *only* operation — barely better than an empty block.

It's hard to believe that in the vast majority of cases it's actually acceptable for a program to carry on without taking any meaningful action. But that's what the data shows across GitHub.

Quality software.
