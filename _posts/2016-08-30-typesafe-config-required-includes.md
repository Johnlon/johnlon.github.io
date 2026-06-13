---
layout: post
title: "Typesafe Config: Contributing \"Required Includes\""
date: 2016-08-30
tags: [scala, java, open-source, config]
blogger_url: https://johnlonergan.blogspot.com/2016/08/typesafe-config-contribution-of-new.html
---

I contributed a new feature to [Typesafe Config](https://github.com/lightbend/config) (PR #421): the ability to mark an included config file as *required*, so that a missing file causes an explicit failure rather than a silent no-op.

## The Problem

By default, Typesafe Config silently ignores missing includes. If you have:

```hocon
include "database.conf"
```

...and `database.conf` doesn't exist, the application starts up without that config and may blow up later in an obscure way, or silently use defaults. There was no way to say "this include is mandatory — fail fast if it's missing."

## The Solution

A new `required` keyword:

```hocon
include required("foo.conf")
include required(file("foo.conf"))
include required(classpath("foo.conf"))
include required(url("http://localhost/foo.conf"))
```

All four include variants support it. If the resource is missing, Config throws an exception immediately rather than carrying on.

This is a small thing but it makes config errors much easier to diagnose. Fail fast, fail clearly.

Took longer than expected to get across the line, but the PR is merged. Full details in the [pull request](https://github.com/lightbend/config/pull/421).
