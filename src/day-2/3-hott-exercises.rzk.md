# Exercises for Day 2

```rzk
#lang rzk-1
```

## Sets

```{exercise . rzk}
#define is-set-if-Equiv
  ( A B : U)
  ( is-set-A : is-set A)
  : Equiv A B → is-set B
  := _exercise
```

```{exercise . rzk}
#define is-set-prod
  ( A B : U)
  ( is-set-A : is-set A)
  ( is-set-B : is-set B)
  : is-set (prod A B)
  := _exercise
```

```{exercise . rzk}
#define is-set-coprod
  ( A B : U)
  ( is-set-A : is-set A)
  ( is-set-B : is-set B)
  : is-set (coprod A B)
  := _exercise
```

```{exercise . rzk}
#define is-prop-if-is-contr-endo
  ( A : U)
  ( is-contr-endo : is-contr (A → A))
  : is-prop A
  := _exercise
```

```{exercise . rzk}
#define Equiv-is-prop-map-into-is-contr
  ( A : U)
  : Equiv (is-prop A) (A → is-contr A)
  := _exercise
```
