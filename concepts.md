# What are state machines?

The point of state machines is to really nail down what something means, and
how it works.

However, FSMs have been around for decades, and have been extended by a wide
range of people in a wide range of languages.  These individuals have used a
diversity of language for similar or identical features.

As such, let's start with a vocabulary.



<br/><br/>

# Concepts

![](https://github.com/StoneCypher/fsl/blob/master/assets/horiz%204-stop%20traffic%20light%20with%20code.png?raw=true)

* `state`
    * The nodes of the graph.  
    * Here, there are four `state`s: `Red`, `Green`, `Yellow`, and `Off`
    * The finite state machine is **always** in *exactly one* `state`.
    * Other than during hooks, where the past and future state may be
      validated in parallel, there is no "in-between" or overlap.  To the
      outside world, the transition between states is atomic.
* `transition`
    * The edges of the graph.
    * Here, there are seven edges, though one may not be obvious:
        1. From `Red` to `Green`
        1. From `Green` to `Yellow`
        1. From `Yellow` to `Red`
        1. From `Green` to `Off`
        1. From `Yellow` to `Off`
        1. From `Red` to `Off`
        1. From `Off` to `Red` (this one can be hard to notice, since it's between the same two edges as #6 - it's visible on the graph as a doubled edge)

<br/><br/>