# Univalence

```rzk
#lang rzk-1
```

Given two functions `#!rzk f : A → B` and `#!rzk g : A → B`, we define a homotopy from `#!rzk f` to `#!rzk g`
as point-wise equality:

```rzk
#define homotopy
  ( A B : U)
  ( f g : A → B)
  : U
  := (x : A) → f x = g x
```

Following HoTT Book, we say that a function `#!rzk f : A → B`
is an _equivalence_ if it has both a retraction (left inverse) and a section (right inverse):

```rzk
#define is-equiv
  ( A B : U)
  ( f : A → B)
  : U
  := prod
      ( Σ (r : B → A), homotopy A A (\ a → r (f a)) (identity A))
      ( Σ (s : B → A), homotopy B B (\ b → f (s b)) (identity B))
```

Two types `#!rzk A` and `#!rzk B` are equivalent if there exists an equivalence `#!rzk f : A → B`:

```rzk
#define Equiv
  ( A B : U)
  : U
  := Σ (f : A → B), is-equiv A B f
```

An example of an equivalence is an identity function:

```rzk
#define is-equiv-identity
  ( A : U)
  : is-equiv A A (identity A)
  := ((identity A , \ _ → refl)
     , (identity A , \ _ → refl))
```

We can show that `#!rzk Equiv` is an equivalence relation:

```rzk
-- Equiv is reflexive
#define Equiv-identity
  ( A : U)
  : Equiv A A
  := (identity A, is-equiv-identity A)

-- #define Equiv-sym
--   (A B : U)
--   : Equiv A B → Equiv B A
--   := \ (f , ((r, rf-eq), (s, fs-eq))) →
--        (s , ((f, fs-eq), (f, _exercise)) )
```

Univalence axiom (HoTT Book Axiom 2.10.3) states that equality of types
is equivalence to equivalence of types[^1]:

[^1]: Here we postulate the equivalence, although, it is sufficient to
  say that a function `#!rzk idtoequiv : (A = B) → (Equiv A B)` is
  an equivalence.

```rzk
#define UnivalenceAxiom
  : U
  :=
    ( A : U)
  → (B : U)
  → Equiv (A = B) (Equiv A B)

#postulate ua : UnivalenceAxiom

#define inv
  ( A B : U)
  : Equiv A B → (B → A)
  := \ (f, ((r, er), (s, es))) → r

#define eq-from-Equiv
  ( A B : U)
  : Equiv A B → (A = B)
  := inv (A = B) (Equiv A B)
        ( ua A B)
```
