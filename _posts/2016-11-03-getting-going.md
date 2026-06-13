---
layout: post
title: "Getting Going (with Go)"
date: 2016-11-03
tags: [go, golang, java]
blogger_url: https://johnlonergan.blogspot.com/2016/11/getting-going.html
---

After years in the Java/JVM world, I've started picking up Go. First impressions: I like it. But two things tripped me up early.

## Dependency Management

Go doesn't have a Maven Central. There's no centralized artifact repository where you pull compiled JARs. Instead, `go build` downloads dependency *source code* and compiles everything in the right order itself.

Dependencies are declared in import statements, typically as GitHub paths:

```go
import "github.com/user/stringutil"
```

Which is elegant in one sense — the import *is* the location. But it takes some adjusting if you're used to a world where your build tool resolves named, versioned artifacts.

## Reproducible Builds

The bigger surprise: basic `go build` has no explicit version pinning. In Maven or Gradle you declare exact versions and get reproducible builds for free. Go doesn't do that out of the box.

To get reproducible builds you either commit third-party source to your own repository (the "vendor" approach), or you use external tooling to lock dependency versions. Go modules eventually solved this properly, but it wasn't the default when I started.

## The Good Bits

The way functions map to data structures is clean. The language feels deliberately minimal without feeling sparse. I'm looking forward to digging into the concurrency model, immutability patterns, and the I/O primitives.

More to follow.
