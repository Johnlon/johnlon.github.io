---
layout: post
title: "Sad State of Java Lambdas and Checked Exceptions"
date: 2016-11-11
tags: [java, lambda, exceptions]
blogger_url: https://johnlonergan.blogspot.com/2016/11/sad-state-of-java-lambdas-and-checked.html
---

Functional programming in Java 8 is incompatible with checked exceptions. Which is a strange position for a language that built its entire exception philosophy on forcing you to handle them.

The tension is right there in the standard library. `UncheckedIOException` exists specifically to wrap checked `IOException`s so that they can pass through lambda expressions without blowing up the type system. Oracle added it to their own IO libraries as a workaround for a problem they created.

The implication is clear: if you want to write idiomatic Java 8 with lambdas and streams, you end up converting checked exceptions to unchecked ones. You're implicitly abandoning checked exceptions the moment you go functional.

Brian Goetz wrote about [exception transparency](https://blogs.oracle.com/jrose/exception-transparency-in-jvm-languages) back in 2010 — Oracle knew this was a problem years before Java 8 shipped. They didn't fix it.

Perhaps it's the end of the road for checked exceptions. The language kept the mechanism but the ecosystem is quietly walking away from it.
