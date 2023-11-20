# Identity types

!!! info "Reference material"

    This page is mostly based on the Section 1.12 of HoTT Book.

```rzk
#lang rzk-1
```

## Examples, `refl`, and (based) induction principle

For each type `#!rzk (A : U)` and elements `#!rzk (a b : A)`
we have the identity type `#!rzk a =_{A} b` of equalities (identities, paths)
between `a` and `b`.

```rzk
#define FunExt
  : U
  := (A : U) → (B : U)
    → (f : A → B) → (g : A → B)
    → ((x : A) → f x =_{B} g x)
    → (f =_{A → B} g)
```

Rzk can figure out the type indices for identity types and we can omit them:

```rzk
#define FunExt₁ : U
  := (A : U) → (B : U)
    → (f : A → B) → (g : A → B)
    → ((x : A) → f x = g x)
    → (f = g)
```

One way to prove an equality that exists for all types,
is the proof by reflexivity. For example,

```rzk
#define identity-x-eq-x
  (A : U)
  (x : A)
  : (identity A x = x)
  := refl_{x : A}
```

Indeed, `identity A x` computes to `x`,
and is therefore definitionally (computatioally) equal to it
allowing for the use of `refl_{x : A}`.
Again, Rzk can figure out indices accepting `refl_{x}` or just `refl`.

Using a value of an identity type requires the path induction,
which we can define via the built-in version of it, `#!rzk idJ`:

```rzk
#define ind-path
  (A : U)
  (a : A)
  (C : (x : A) -> (a = x) -> U)
  (d : C a refl)
  (b : A)
  : (p : a = b) → C b p
  := \ p → idJ (A, a, C, d, b, p)
```

As an example, we can show symmetry for equality types
by induction on the argument of type `a = b`:

```rzk
#define inverse
  (A : U)
  (a b : A)
  : (a = b) → (b = a)
  := ind-path A a (\ x _ → x = a) refl b
```

## Indiscernibility of identicals

```rzk
#define indiscernibility-of-identicals
  ( A : U)
  ( C : A → U)
  ( a x : A)
  : (a = x)
  → C a → C x
  := ind-path A a
    ( \ z _ → (C a → C z))
    ( \ ca → ca)
    x
```

## Not-based path induction

```rzk
#define ind-path'
  ( A : U)
  ( C : (x : A) → (y : A) → (x = y) → U)
  ( c : (x : A) → C x x refl_{x})
  ( x y : A)
  : (p : x = y) → C x y p
  := ind-path A x (C x) (c x) y
```

## Dependent sums with identity types

The type `#!rzk Σ (x : A), B x` is a type of (dependent) pairs
where the first component (named `x` here) is of type `A`,
and the second component is of type `B x`.

For example, preimages (fibers) of functions can be defined
using `#!rzk Σ`-types:

```rzk
#define preimage
  (A B : U)
  (f : A → B)
  (y : B)
  : U
  := Σ (x : A), (f x = y)
```

```rzk
#define product
  ( A B : U)
  : U
  := Σ (_ : A), B
```
