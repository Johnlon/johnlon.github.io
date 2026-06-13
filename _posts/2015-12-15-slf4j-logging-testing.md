---
layout: post
title: "SLF4J Logging Testing"
date: 2015-12-15
tags: [java, testing, logging, slf4j]
blogger_url: https://johnlonergan.blogspot.com/2015/12/slf4jlogging-testing.html
---

Testing log output in Java is more annoying than it should be. Most existing approaches rely on singletons and `StaticLogBinder` — which means they break under concurrent test execution. You end up with flaky tests and logging assertions that pass in isolation and fail under parallelism.

I got tired of writing the same log mocking code over and over, so I wrote a utility library instead.

**[slf4jtesting](https://github.com/portingle/slf4jtesting)**

The key difference from other approaches: it's designed for dependency injection rather than static configuration, so each test gets its own isolated logger instance. No shared state, no interference between concurrent tests.

If you're testing code that logs — and you should be, because logs are part of your observable behaviour — and you're running tests in parallel, this is worth a look.
