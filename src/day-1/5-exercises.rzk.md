# Exercises for Day 1

```rzk
#lang rzk-1
```

## Dependent types

**Exercise 1.1.** _(HoTT Book Exercise 1.1)_
Given functions `#!rzk f : A → B` and `#!rzk g : B → C`,
define their _composite_ `#!rzk compose A B C f g : A → C`.
Show that composition is associative (definitionally)
and unital (w.r.t. `#!rzk identity`).

??? abstract "Solution for Exercise 1.1"

    ```rzk
    #define compose
      (A B C : U)
      (f : A → B)
      (g : B → C)
      : A → C
      := \ x → g (f x)

    #define compose-assoc
      (A B C D : U)
      (f : A → B)
      (g : B → C)
      (h : C → D)
      : compose A B D f (compose B C D g h) = compose A C D (compose A B C f g) h
      := refl

    #define compose-left-unit
      (A B : U)
      (f : A → B)
      : compose A B B f (identity B) = f
      := refl

    #define compose-right-unit
      (A B : U)
      (f : A → B)
      : compose A A B (identity A) f = f
      := refl
    ```

**Exercise 1.2.** _(HoTT Book Exercises 1.2 and 1.3)_
Derive recursion and induction principles for products `#!rzk rec-prod A B`
using only the projections `#!rzk pr₁` and `#!rzk pr₂`,
and verify that the definitional equalities are valid.
Do the same for Σ-types.

**Exercise 1.3.**
Show that Σ-types are associative in the following sense:

```rzk
#define concat-assoc
  ( A : U)
  ( x y z u : A)
  : (p : x = y)
  → (q : y = z)
  → (r : z = u)
  → concat A x y u p (concat A y z u q r)
    =_{x = u}
    concat A x z u (concat A x y z p q) r
  := ind-path A x (\ w p →
      (q : w = z)
      → (r : z = u)
      → concat A x w u p (concat A w z u q r)
      =_{x = u}
      concat A x z u (concat A x w z p q) r) (\ _ _ → refl) y
```

```{exercise .rzk}
#define reassoc-left-Σ
  ( A : U)
  ( B : A → U)
  ( C : (a : A) → B a → U)
  : (Σ (a : A), Σ (b : B a), C a b)
  → (Σ (ab : Σ (a : A), B a), C (pr₁ A B ab) (pr₂ A B ab))
  := _exercise

#define reassoc-right-Σ
  ( A : U)
  ( B : A → U)
  ( C : (a : A) → B a → U)
  : (Σ (ab : Σ (a : A), B a), C (pr₁ A B ab) (pr₂ A B ab))
  → (Σ (a : A), Σ (b : B a), C a b)
  := _exercise

#define assoc-left-Σ
  ( A : U)
  ( B : A → U)
  ( C : (a : A) → B a → U)
  ( abc : Σ (a : A), Σ (b : B a), C a b)
  : reassoc-right A B C (reassoc-left A B C abc) = abc
  := _exercise

#define assoc-right-Σ
  ( A : U)
  ( B : A → U)
  ( C : (a : A) → B a → U)
  ( abc : Σ (ab : Σ (a : A), B a), C (pr₁ A B ab) (pr₂ A B ab))
  : reassoc-left A B C (reassoc-left A B C abc) = abc
  := _exercise
```

## Propositions as types

**Exercise 1.4.** _(HoTT Book Exercise 1.11)_
Show that for any type `#!rzk A`, we have `#!rzk ¬ (¬ (¬ A)) → ¬ A`.

??? abstract "Solution for Exercise 1.3"

    ```rzk
    #define exercise-1.3
      (A : U)
      : ¬ (¬ (¬ A)) → ¬ A
      := \ nnna a → nnna (\ na → na a)
    ```

**Exercise 1.5.** _(HoTT Book Exercises 1.12 and 1.13)_
Using propositions as types interpretation, prove the following tautologies:

1. If A, then (if B then A).
2. If A, then not (not A).
3. If (not A or not B), then not (A and B).
4. (*) For any P, double negation of the law of excluded middle holds: not (not (not P or P)).

## Identity types

**Exercise 1.6.** _(HoTT Book Exercise 1.14)_
Why do the induction principles for identity types not allow us to construct a
the following function?

```{unchecked .rzk}
#define bad
  : (x : A)
  → (p : x = x)
  → p = refl_{x}
  := ind-path A x _ refl_{refl_{x}}
```

## Homotopy type theory

**Exercise 1.7.**
Formalize both proofs of HoTT Book Lemma 2.1.1.
Show that both proofs are equal.

```rzk
#define path-inv₁
  (A : U)
  (x y : A)
  : (x = y) → (y = x)
  := ind-path' A (\ a b _ → b = a) (\ _ → refl) x y

#define path-inv₂
  (A : U)
  (x y : A)
  : (x = y) → (y = x)
  := ind-path A x (\ w _ → w = x) refl y

#define eq-path-inv₁-path-inv₂
  (A : U)
  (x y : A)
  (p : x = y)
  : path-inv₁ A x y p = path-inv₂ A x y p
  := refl
```

**Exercise 1.8.**
Formalize both proofs of HoTT Book Lemma 2.1.2.
Show that both proofs are equal.
Consider a third proof and show that it is equal to the other two proofs:

```rzk
-- a different proof for transitivity
#define concat₃
  (A : U)
  (x y z : A)
  : (x = y) → (y = z) → (x = z)
  := \ p → ind-path A y (\ w _ → x = w) p z
```

**Exercise 1.9.**
Formalize the proofs of HoTT Book Lemma 2.1.4.
Show that different proofs for each point in the lemma are equal.

??? abstract "Solution for Lemma 2.1.4 (i)"

    ```rzk
    #define concat-left-unit
      (A : U)
      (x y : A)
      (p : x = y)
      : concat A x x y refl p = p
      := refl

    #define concat-right-unit₁
      (A : U)
      (x y : A)
      (p : x = y)
      : concat A x y y p refl = p
      := ind-path' A (\ a b p' → concat A a b b p' refl = p') (\ _ → refl) x y p

    #define concat-right-unit₂
      (A : U)
      (x y : A)
      (p : x = y)
      : concat A x y y p refl = p
      := ind-path A x (\ w p' → concat A x w w p' refl = p') refl y p

    #define eq-concat-right-unit₁-concat-right-unit₂
      (A : U)
      (x y : A)
      (p : x = y)
      : concat-right-unit₁ A x y p = concat-right-unit₂ A x y p
      := refl
    ```

### Loop space

Consider the definitions of loop space and 2-loop space (HoTT Book Definition 2.1.8):

```rzk
#define Ω
  ( A : U)
  ( a : A)
  : U
  := a =_{A} a

#define Ω²
  ( A : U)
  ( a : A)
  : U
  := Ω (Ω A a) refl_{a}

#define concat-Ω
  ( A : U)
  ( a : A)
  : Ω A a → Ω A a → Ω A a
  := concat A a a a
```

**Exercise 1.10 (difficult).**
Formalize the proof of HoTT Book Theorem 2.1.6 (Eckmann-Hilton):

```{exercise .rzk}
#define Eckmann-Hilton
  ( A : U)
  ( a : A)
  ( α β : Ω A a)
  : concat-Ω A a α β = concat-Ω A a β α
  := _exercise
```
