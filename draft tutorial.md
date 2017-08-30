# `FSL` Language Tutorial 0.1.0

`FSL`, or "Finite State Language," is a notation for writing finite state machines concisely and clearly.



<br/><br/>
## Quick Notation Tutorial

A complete piece of `FSL` code is referred to as a `machine`.  

A `FSL machine` is made from `state`s, `transition`s, sometimes `action`s or `annotation`s, and occasionally
other things.  A `FSL machine` must have at least one `transition`, and therefore at least one `state`.  Otherwise,
the entire language is optional.

`FSL` is designed to make most things implicit in well defined and common sense ways.  The most common example of
a state machine seems to be the traffic light, which has four `state`s: the three basic colors **Red**, **Yellow**, and
**Green**, and a `state` to represent when the machine is physically powered down, which we'll call **Off**.

### States and Transitions: a->b;
In `FSL`, the most obvious way to write a traffic light is as its three colors, which are this `machine`'s `state`s,
and the changes they can undergo (eg from **Green** to **Yellow**,) which are this `machine`'s `transition`s, in 
their correct order:

```fsl
Green -> Yellow;
Yellow -> Red;
Red -> Green;
```

### Chains: a->b->c;
You can save time, and add some clarity, by writing those as a chain:

```fsl
Green -> Yellow -> Red -> Green;
```

### Mixed transitions and chains: a->b->c; d->e;
You'd also want to do the stuff to **Off**:

```fsl
Green -> Yellow -> Red -> Green;

Off -> Red;

Green -> Off;
Yellow -> Off;
Red -> Off;
```

### Node lists: [a b c]
You could drop the redundancy to **Off** with a `list`:

```fsl
Green -> Yellow -> Red -> Green;

Off -> Red;
[Green Yellow Red] -> Off;
```

### Main Path: =>
Then it might be smart to mark the color cycle as a `main path` using the `=>` operator.  

This doesn't have a lot of impact; it gets drawn differently in the visualizer, and makes the machine 
easier to read, and some implementations such as [jssm](https://github.com/StoneCypher/jssm) carry more 
statistics for the different path types, but it does not have much of a language meaning.

```fsl
Green => Yellow => Red => Green;

Off -> Red;
[Green Yellow Red] -> Off;
```

### Actions: 'name'
Then we can add `'action names'`, such as `'Proceed'`, `'Enable'`, and `'Disable'`:

```fsl
Green 'Proceed' => Yellow 'Proceed' => Red 'Proceed' => Green;

Off 'Enable' -> Red;
[Green Yellow Red] 'Disable' -> Off;
```

This means that we can tell the light to **proceed**, instead of telling it to switch to **Yellow** specifically,
letting it handle events with internal intent, simplifying working with `machine`s.

### Forced-only paths: ~>
Next we mark the paths to **Off** as `forced-only` with `~>`:

```fsl
Green 'Proceed' => Yellow 'Proceed' => Red 'Proceed' => Green;

Off 'Enable' -> Red;
[Green Yellow Red] 'Disable' ~> Off;
```

### Named ordered sequences: &Foo

If we wanted, we could also declare the **Colors** as a `named ordered sequence`:

```fsl
&Colors: [Green Yellow Red];

Green 'Proceed' => Yellow 'Proceed' => Red 'Proceed' => Green;

Off 'Enable' -> Red;
[Green Yellow Red] 'Disable' ~> Off;
```

### Using named sequences in transitions: &Foo -> bar;

`Named ordered sequences` come out as a multiplexing in arrows, like usual:

```fsl
&Colors: [Green Yellow Red];

Green 'Proceed' => Yellow 'Proceed' => Red 'Proceed' => Green;

Off 'Enable' -> Red;
&Colors 'Disable' ~> Off;
```

### Using named sequences to make cycles: &Foo -> (+1);

Named sequences can also be used with the `cycle` operator `(N)` to simplify how we describe loops:

```fsl
&Colors: [Green Yellow Red];

&Colors 'Proceed' => (+1);

Off 'Enable' -> Red;
&Colors 'Disable' ~> Off;
```

Which is suddenly more readable when written in this order:

```fsl
&Colors: [Green Yellow Red];

&Colors 'Proceed' => (+1);
&Colors 'Disable' ~> Off;

Off 'Enable' -> Red;
```
