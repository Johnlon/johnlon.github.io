---
layout: post
title: "Introducing ClasspathHell — Classpath Collision Detector"
date: 2016-08-30
tags: [java, build, open-source, classpath]
blogger_url: https://johnlonergan.blogspot.com/2016/08/introducing-classpathhell-classpath.html
---

First release of [ClasspathHell](https://github.com/portingle/classpathHell) — a tool for detecting classpath collisions before they cause production incidents.

## The Problem

It's far too easy to end up with multiple copies of a class or resource on your classpath. When that happens, behaviour depends on which copy gets loaded first — and classpath ordering can be unstable across environments, meaning tests pass but production breaks, or the bug only surfaces late in the release cycle.

A concrete example: Dropwizard and Codahale Metrics share identical fully-qualified class names but have different interfaces and implementations. If both end up on your classpath, you have a time bomb.

These failures are hard to diagnose because they look like unrelated runtime errors. The collision happened at build time; the explosion happens somewhere else entirely.

## The Solution

ClasspathHell scans your classpath and detects duplicate classes and resources. It's designed to be added to your build so these collisions get caught early — at build time, not in production.

```
https://github.com/portingle/classpathHell
```

The apparent stability of a build that has classpath collisions is deceptive. You might be fine right up until you're not. Run this and find out.
