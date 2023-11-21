# Propositions as types

!!! info "Reference material"

    This page is mostly based on the Section 1.11 of HoTT Book.

```rzk
#lang rzk-1
```

Here is a quick "cheat sheet" relating propositions and types:

English | Type theory | Rzk
----- | ----- | -----
True | $\mathbf{1}$ | `#!rzk Unit`
False | $\mathbf{0}$ | `#!rzk Void` [^1]
A and B | $A \times B$ | `#!rzk prod A B`
A or B | $A + B$ | `#!rzk coprod A B` [^2]
If A then B | $A \to B$ | `#!rzk A → B`
A if and only if B | $(A \to B) \times (B \to A)$ | `#!rzk prod (A → B) (B → A)`
Not A | $A \to \mathbf{0}$ | `#!rzk A → 0` [^1]
For all x : A, P(x) holds | $\prod_{x : A} P(x)$ | `#!rzk (x : A) → P x`
Exists x : A such that P(x) holds | $\sum_{x : A} P(x)$ | `#!rzk Σ (x : A), P x`[^3]

[^1]: Does not exist in Rzk as of v0.6.7, but can be postulated.
[^2]: Does not exist in Rzk as of v0.6.7, but can be postulated in a weak form.
[^3]: This is only for the constructive version of "exists".
  For the classical version, we need the propostional truncation of the Σ-type.
  See Section 3.7 of HoTT Book for more details.

```rzk
#define ¬ (A : U) : U
  := A → Void
```

## De Morgan laws

We can prove the following de Morgan laws:

> If not A and not B, then not (A or B).

```rzk
#define neg-prod
  (A B : U)
  : prod (¬ A) (¬ B)  →  ¬ (coprod A B)
  -- : prod (A → Void) (B → Void) → (coprod A B → Void)
  := \ ( na , nb ) → \ z →
    rec-coprod A B Void
      (\ a → na a)  -- case of (inl a)
      (\ b → nb b)  -- case of (inr b)
      z
    -- na : A → Void
    -- nb : B → Void
    -- z : coprod A B
```

> If not (A or B), then not A and not B.

```rzk
#define neg-coprod
  (A B : U)
  : ¬ (coprod A B)
  → prod (¬ A) (¬ B)
  := \ k → ( \ a → k (inl A B a) , \ b → k (inr A B b) )
  -- k : coprod A B → Void
  -- a : A
```

Not all classical tautologies are provable in HoTT:

```{unchecked .rzk}
#define classical-tauto
  (A B : U)
  : ¬ (prod A B) → coprod (¬ A) (¬ B)
  := \ k → ???
```

> If for all x : A, P(x) and Q(x) then (for all x : A, P(x)) and (for all x : A, Q(x)).

```rzk
#define forall-prod
  (A : U)
  (P Q : A → U)
  : ((x : A) → prod (P x) (Q x))
  → prod ((x : A) → P x) ((x : A) → Q x)
  := \ k → ( \ x → first (k x) , \ x → second (k x))
```
