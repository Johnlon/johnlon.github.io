---
layout: post
title: "Recommended Pattern for Shared State in GoDog"
date: 2024-05-17
tags: [go, godog, cucumber, bdd, testing]
blogger_url: https://johnlonergan.blogspot.com/2024/05/recommented-pattern-for-shared-state-in.html
---

Clarity, Concurrency, Convenience.

I'm currently making some changes to Cucumber godog which is a behaviour driven development tool for golang. Whilst writing some tests I've struggled to find a nice pattern for shared state across steps.

What is the recommendation when we have a collection of values that we need to share between steps?

A common anti-pattern I've seen in godog usage is just to have a bunch of global variables. This approach has at least one shortcoming in that this pattern doesn't lend itself to parallel execution — global vars would be shared across all threads and so the threads would trample on each other.

Godog, like Cucumber JVM, does provide a better method to share state across just the steps in a single scenario and this approach isn't susceptible to the issues that globals would. You can see examples of this in the godog documentation where `context.Context` is threaded through the steps and necessary shared state is communicated by adding it to the context.

However, the godog docs don't provide evolved examples of the practices around the use of the context, and only show storing a single primitive value into the context.

But, what do I do when I have a collection of state values that I want to share?

My preferred solution is to create a struct that encapsulates all these shared values and which can be added to the context. This is somewhat similar to what we always do in Cucumber JVM.

I then provide a utility function to access the shared state. The pattern I illustrate below is the most convenient I've come up with. It is also convenient in that we can easily trace where the various shared state fields are accessed.

```go
// Struct to encapsulate the shared state into ordinary typed fields
type SharedState struct {
    Name string
    Url  http.Url
}

// key used to store/retrieve the shared state from the context
var shareStateKey = SharedState{}

// utility function to retrieve the state object from the context.
// adds the state object to the context if it's not already present.
// because we're returning a reference to the shared state this is effectively
// a mutable object, which leads to convenient syntax in the steps below.
func getSharedState(ctx context.Context) (context.Context, *SharedState) {
    v := ctx.Value(shareStateKey)
    if v == nil {
        v = &SharedState{}
        ctx = context.WithValue(ctx, shareStateKey, v)
    }
    return ctx, v.(*SharedState)
}

// example usage
func someStepDef(ctx context.Context, stepArg string) (context.Context, error) {
    ctx, state := getSharedState(ctx)
    state.Name = stepArg
    return ctx, nil
}

// another example
func someOtherStep(ctx context.Context) (context.Context, error) {
    ctx, state := getSharedState(ctx)
    fmt.Println(state.Name)
    return ctx, nil
}
```
