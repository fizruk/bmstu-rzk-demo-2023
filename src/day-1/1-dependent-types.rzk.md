# Dependent types

```rzk
#lang rzk-1
```

We now proceed to look at the primitives in Rzk
for working with dependent types.

### Functions

The type `#!rzk (x : A) → B x` is the type of (dependent)
functions with an argument of type `A` and, for each input `x`,
the output type `B x`.

As a simple example of a dependent function,
consider the identity function:

```rzk
#define identity
  : (A : U) → (x : A) → A
  := \ A x → x
```

Since we are not using `x` in the type of `identity`,
we can simply write the type of the argument,
without providing its name:

```rzk
#define identity₁
  : (A : U) → A → A
  := \ A x → x
```

We can write this definition differently,
by putting `#!rzk (A : U)` into parameters (before `:`),
and omitting it in the lambda abstraction:

```rzk
#define identity₂
  (A : U)
  : A → A
  := \ x → x
```

We could also move `x` into parameters as well,
although this probably does not increase readability anymore:

```rzk
#define identity₃
  (A : U)
  (x : A)
  : A
  := x
```
