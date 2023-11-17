# Homotopy type theory

```rzk
#lang rzk-1
```

In HoTT (and in Rzk), the identity type is not always a proposition
and `refl` might not be the only proof of equality!

```{ unchecked .rzk title="Type Error!" }
#define does-not-typecheck
  (A : U)
  (x : A)
  (p : x = x)
  : p = refl
  := refl -- path induction on p also cannot work!
```

!!! info

    At the moment Rzk does not have support for higher inductive types,
    but it is expected in the future.
