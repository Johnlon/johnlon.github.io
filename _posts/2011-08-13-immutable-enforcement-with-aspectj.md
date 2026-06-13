---
layout: post
title: "Immutable Enforcement with AspectJ"
date: 2011-08-13
tags: [java, aspectj, immutability, annotation]
blogger_url: https://johnlonergan.blogspot.com/2011/08/immutable-enforcement-with-aspectj.html
---

Java has no built-in immutability enforcement. You can mark fields `final`, but the compiler won't stop you putting a mutable object in one. I wrote an AspectJ aspect that actually enforces it.

## The Approach

Classes annotated with `@Immutable` get validated at load time. The aspect checks two rules:

- **Primitive fields** must be declared `final`
- **Object fields** must themselves be typed as `@Immutable` classes

To avoid annotating everything, I maintain a trusted list of known-immutable types — `String`, `Float`, and so on — that are treated as implicitly immutable.

## The Annotation

Existing JSR annotations at the time only supported type-level annotation, not field-level. I wrote a custom `@Immutable` annotation applicable to both:

```java
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.FIELD})
public @interface Immutable {}
```

## The Aspect

The aspect uses pointcuts to intercept field mutations and validates the two rules above. A valid class looks like:

```java
@Immutable
public class Point {
    private final int x;
    private final int y;
    private final String label; // trusted immutable type
}
```

Violations — a non-final field, or a field typed as a mutable class — throw at load time rather than silently allowing broken immutability assumptions.

## Limitations

I didn't finish extending enforcement to superclasses. A complete implementation would also require that any superclass either carries `@Immutable` or is `Object` directly — otherwise a subclass could inherit mutable fields from an unannotated parent. That remains an open gap.
