# Homotopy type theory

!!! info "Reference material"

    This page is mostly based on the Sections 2.1–2.3 of HoTT Book.

```rzk
#lang rzk-1
```

In HoTT (and in Rzk), the identity type is not always a proposition
and `refl` might not be the only proof of equality!

```{ unchecked .rzk title="Type Error!" }
#define does-not-typecheck
  ( A : U)
  ( x : A)
  ( p : x = x)
  : p = refl
  := refl -- path induction on p also cannot work!
```

```rzk
#define concat
  ( A : U)
  ( x y z : A)
  : (x = y) → (y = z) → (x = z)
  := ind-path A x (\ y' _ → (y' = z) → (x = z)) (identity (x = z)) y
```

!!! info

    At the moment Rzk does not have support for higher inductive types,
    but it is expected in the future.
