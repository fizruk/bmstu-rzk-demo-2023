# Introduction to simplicial HoTT and synthetic ∞-categories

```rzk
#lang rzk-1
```

## Simplicial types with Rzk

Following Riehl–Shulman's paper[^2], Rzk has cube and tope layers[^3].
These provide the ability to specify (higher) diagram schemas.

[^2]:
    Emily Riehl & Michael Shulman. A type theory for synthetic ∞-categories.
    Higher Structures 1(1), 147-224. 2017. <https://arxiv.org/abs/1705.07442>

[^3]:
    Instead of builtin layers, Rzk uses `#!rzk CUBE` and `#!rzk TOPE` universes
    to separate them from the types.

For the purposes of his demo, we will only care about the directed interval cube
`#!rzk 2`, 2-dimensional directed cube `#!rzk (2 × 2)`, and 3-dimensional
directed cube `#!rzk (2 × 2 × 2)`.

A cube may have points in it, and the directed interval `#!rzk 2`
has two known points `#!rzk 0₂ : 2` and `#!rzk 1₂ : 2`.

### Tope layer

Tope layer adds logical constraints on the points in a cube,
"carving out" a shape inside a space. There is a handful of ways to specify topes:

- `#!rzk TOP` selects all points (provides no constraints)
- `#!rzk BOT` selects no points (producing an empty shape)
- `#!rzk (φ ∧ ψ)` selects all points that satisfy both `φ` and `ψ`
- `#!rzk (φ ∨ ψ)` selects all points that satisfy `φ` or `ψ` (or both)
- `#!rzk (t ≡ s)` selects all points such that `t` is equal to `s`
- `#!rzk (t ≤ s)` selects all points such that `t ≤ s` (only when both `t` and `s` are in `#!rzk 2`)

Mostly for technical reasons, we define shapes as mappings from cubes into the `#!rzk TOPE` universe.
For example, here is a shape that carves a (directed) triangle out of a (directed) square:

<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <path d="M -88.57160646618001 -39.70915201762215 L 98.4961027394271 -39.70915201762215 L 89.32046645922577 100.78557152253414 Z" style="fill: orange; opacity: 0.2"><title></title></path>
  <text x="33.08165424415762" y="7.122422495763279" fill="orange"></text>
  <polyline points="-90.03982894447489,-47.97354751998429 90.03982894447466,-47.97354751998429" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="-1.1368683772161603e-13" y="-47.97354751998429" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-94.34447446980596,-35.57774791227483 83.54960825780415,104.91856291954896" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="-5.3974331060009035" y="34.670407503637065" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="108.73641627786131,-28.016064827507464 100.54837539908644,97.35687983478158" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="104.64239583847387" y="34.67040750363706" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <text x="-110.03982894447489" y="-47.97354751998429" fill="orange">•</text>
  <text x="110.03982894447466" y="-47.97354751998429" fill="orange">•</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="orange">•</text>
</svg>

```rzk
#define Δ¹
  : 2 → TOPE
  := \ _ → TOP

#define Δ²
  : (2 × 2) → TOPE
  := \ (t, s) → (s ≤ t)
```

For another example, here is a 3-dimensional simplex:

<svg class="rzk-render" viewBox="-175 -200 350 375" width="150" height="150">
  <path d="M -26.73167571486168 -157.4409366304129 L 122.95643796793948 -122.88064022664372 L 112.91485008981695 11.079671750322774 Z" style="fill: orange; opacity: 0.2"><title></title></path>
  <text x="69.71320411429825" y="-89.74730170224461" fill="orange"></text>
  <path d="M -30.387208237342456 -151.88391603994765 L 119.30090544545868 -117.32361963617848 L 47.115264685162884 111.10604237869723 Z" style="fill: orange; opacity: 0.2"><title></title></path>
  <text x="45.3429872977597" y="-52.70049776580962" fill="orange"></text>
  <path d="M -30.977889877232023 -144.00389768836138 L 108.66863592744662 24.516710692374282 L 46.524583045273324 118.98606073028348 Z" style="fill: orange; opacity: 0.2"><title></title></path>
  <text x="41.40510969849597" y="-0.16704208856786806" fill="orange"></text>
  <path d="M 127.51540696338097 -107.41064267260579 L 117.47381908525844 26.549669304360705 L 55.32976620308516 121.01901934226993 Z" style="fill: orange; opacity: 0.2"><title></title></path>
  <text x="100.10633075057486" y="13.386015324674949" fill="orange"></text>
  <polyline points="-24.26401823142078,-164.88759497832305 112.86496060646587,-133.2269771938925" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="44.30047118752255" y="-149.05728608610778" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-30.99018144517942,-153.98712182088258 107.7774910224333,13.472916680392292" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="38.39365478862694" y="-70.25710257024514" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-38.0977970126922,-150.20257599443408 41.77445614033044,120.82878266324879" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="1.8383295638191228" y="-14.686896665592649" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="130.85730851717483,-108.78365343107289 122.0336642163157,8.928620530311047" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="126.44548636674526" y="-49.92751645038092" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="126.32588298339418,-109.65724468567551 53.454439300480715,120.94262359421863" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="89.89016114193745" y="5.6426894542715615" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="109.5471837166916,45.58156379718138 58.419505769392075,123.30418214308702" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="83.98334474304184" y="84.4428729701342" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <text x="-43.751360390595785" y="-169.386872205972" fill="orange">•</text>
  <text x="132.35230276564087" y="-128.72769996624356" fill="orange">•</text>
  <text x="120.53866996784966" y="28.87266706548172" fill="orange">•</text>
  <text x="47.42801951823403" y="140.0130788747867" fill="orange">•</text>
</svg>

```rzk
#define Δ³
  : (2 × 2 × 2) → TOPE
  := \ ((t₁, t₂), t₃) → (t₃ ≤ t₂) ∧ (t₂ ≤ t₁)
```

Yet another example would be the horn shape that only takes two edges of a square:

<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <polyline points="-90.03982894447489,-47.97354751998429 90.03982894447466,-47.97354751998429" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="-1.1368683772161603e-13" y="-47.97354751998429" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="108.73641627786131,-28.016064827507464 100.54837539908644,97.35687983478158" stroke="orange" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="104.64239583847387" y="34.67040750363706" fill="orange" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <text x="-110.03982894447489" y="-47.97354751998429" fill="orange">•</text>
  <text x="110.03982894447466" y="-47.97354751998429" fill="orange">•</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="orange">•</text>
</svg>

```rzk
#define Λ
  : (2 × 2) → TOPE
  := \ (t, s) → (s ≡ 0₂) ∨ (t ≡ 1₂)
```

We could refine this definition by specifying that it is a subshape of `Δ²`:

```rzk
#define Λ'
  : ( (t, s) : 2 × 2 | Δ² (t, s) ) → TOPE
  := \ (t, s) → (s ≡ 0₂) ∨ (t ≡ 1₂)
```

Since shapes have the information about the cube in their types,
Rzk provides an implicit coercion, allowing shorter definitions:

```rzk
#define Λ''
  : Δ² → TOPE
  := \ (t, s) → (s ≡ 0₂) ∨ (t ≡ 1₂)
```

!!! note

    This more precise type expresses a restriction on the definition of `Λ''`,
    but, perhaps counterintuitively, **not** on the input points!
    With this type, Rzk additionally checks that `#!rzk (s ≡ 0) ∨ (t ≡ 1)`
    implies `Δ² (t, s)` for all `t`, `s`. However, we can still apply `Λ''`
    to **all points** in the cube `#!rzk (2 × 2)`.

Mapping from a shape into a type, effectively selects a concrete diagram in it.
We can select a subdiagram, simply by restricting to a subshape:

<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <path d="M -88.57160646618001 -39.70915201762215 L 98.4961027394271 -39.70915201762215 L 89.32046645922577 100.78557152253414 Z" style="fill: gray; opacity: 0.2"><title>f</title></path>
  <text x="33.08165424415762" y="7.122422495763279" fill="gray">f</text>
  <polyline points="-90.03982894447489,-47.97354751998429 90.03982894447466,-47.97354751998429" stroke="purple" stroke-width="3" marker-end="url(#arrow)"><title>f</title></polyline>
  <text x="-1.1368683772161603e-13" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="-94.34447446980596,-35.57774791227483 83.54960825780415,104.91856291954896" stroke="gray" stroke-width="3" marker-end="url(#arrow)"><title>f</title></polyline>
  <text x="-5.3974331060009035" y="34.670407503637065" fill="gray" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="108.73641627786131,-28.016064827507464 100.54837539908644,97.35687983478158" stroke="purple" stroke-width="3" marker-end="url(#arrow)"><title>f</title></polyline>
  <text x="104.64239583847387" y="34.67040750363706" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <text x="-110.03982894447489" y="-47.97354751998429" fill="purple">f</text>
  <text x="110.03982894447466" y="-47.97354751998429" fill="purple">f</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="purple">f</text>
</svg>

```rzk
#define horn-restriction
  (A : U)
  : (f : Δ² → A) → (Λ → A)
  := \ f t → f t
```

To construct diagrams from parts, we can use `#!rzk recOR`
by specifying values for several shapes. Rzk will be
checking that provided values agree (definitionally) on the intersections
of these shapes.

For example, given a triangle, we can construct a square:

<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <path d="M -99.0358460500274 -31.444756515260025 L -89.86020976982607 109.04996702489628 L 78.85622687537837 109.04996702489628 Z" style="fill: red; opacity: 0.2"><title>recOR (π₁ x₂ ≤ π₂ x₂ ↦ triang…</title></path>
  <text x="-36.6799429814917" y="62.21839251151084" fill="red"></text>
  <path d="M -88.57160646618001 -39.70915201762215 L 98.4961027394271 -39.70915201762215 L 89.32046645922577 100.78557152253414 Z" style="fill: red; opacity: 0.2"><title>recOR (π₁ x₂ ≤ π₂ x₂ ↦ triang…</title></path>
  <text x="33.08165424415762" y="7.122422495763279" fill="red"></text>
  <polyline points="-108.73641627786154,-28.016064827507464 -100.54837539908667,97.35687983478158" stroke="red" stroke-width="3" marker-end="url(#arrow)"><title>recOR (π₁ x₂ ≤ π₂ x₂ ↦ triang…</title></polyline>
  <text x="-104.6423958384741" y="34.67040750363706" fill="red" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-90.03982894447489,-47.97354751998429 90.03982894447466,-47.97354751998429" stroke="red" stroke-width="3" marker-end="url(#arrow)"><title>recOR (π₁ x₂ ≤ π₂ x₂ ↦ triang…</title></polyline>
  <text x="-1.1368683772161603e-13" y="-47.97354751998429" fill="red" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-94.34447446980596,-35.57774791227483 83.54960825780415,104.91856291954896" stroke="red" stroke-width="3" marker-end="url(#arrow)"><title>recOR (π₁ x₂ ≤ π₂ x₂ ↦ triang…</title></polyline>
  <text x="-5.3974331060009035" y="34.670407503637065" fill="red" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="-79.24496273247331,117.31436252725841 79.24496273247308,117.31436252725841" stroke="red" stroke-width="3" marker-end="url(#arrow)"><title>recOR (π₁ x₂ ≤ π₂ x₂ ↦ triang…</title></polyline>
  <text x="-1.1368683772161603e-13" y="117.31436252725841" fill="red" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="108.73641627786131,-28.016064827507464 100.54837539908644,97.35687983478158" stroke="red" stroke-width="3" marker-end="url(#arrow)"><title>recOR (π₁ x₂ ≤ π₂ x₂ ↦ triang…</title></polyline>
  <text x="104.64239583847387" y="34.67040750363706" fill="red" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <text x="-110.03982894447489" y="-47.97354751998429" fill="red">•</text>
  <text x="-99.24496273247331" y="117.31436252725841" fill="red">•</text>
  <text x="110.03982894447466" y="-47.97354751998429" fill="red">•</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="red">•</text>
</svg>

```rzk
#define unfolding-square
  (A : U)
  (triangle : Δ² → A)
  : (2 × 2) → A
  :=
    \ (t, s) →
    recOR
      ( t ≤ s ↦ triangle (s , t) ,
        s ≤ t ↦ triangle (t , s))
```

## Extension types with Rzk

Mapping from a shape into a type, we get a diagram.
To fix some parts of the diagram, we can specify refinements — for each subshape we want to fix,
we provide an explicit term of the proper type.

For example, taking a diagram in `A` correspoding to the directed interval `#!rzk 2`
and fixing the endpoints, we get the type of all arrows between two given points in `A`:

<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <polyline points="-79.24496273247331,117.31436252725841 79.24496273247308,117.31436252725841" stroke="blue" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="-1.1368683772161603e-13" y="117.31436252725841" fill="blue" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <text x="-99.24496273247331" y="117.31436252725841" fill="purple">x</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="purple">y</text>
</svg>

```rzk
#define hom
  (A : U)
  (x y : A)
  : U
  := (t : 2) →
    A [ t ≡ 0₂ ↦ x
      , t ≡ 1₂ ↦ y ]
```

Note that this is different from the following type, since in `hom` endpoints are `x` and `y` definitionally,
where as `pseudo-hom` carries proofs of equality and requires extra bookkeeping:

```rzk
#define pseudo-hom
  (A : U)
  (x y : A)
  : U
  := Σ (f : 2 → A), product (f 0₂ = x) (f 1₂ = y)
```

Another common example of an extension type would be the `hom2` type of composites of arrows:

<svg class="rzk-render" viewBox="-175 -100 350 375" width="150" height="150">
  <path d="M -88.57160646618001 -39.70915201762215 L 98.4961027394271 -39.70915201762215 L 89.32046645922577 100.78557152253414 Z" style="fill: blue; opacity: 0.2"><title></title></path>
  <text x="33.08165424415762" y="7.122422495763279" fill="blue"></text>
  <polyline points="-90.03982894447489,-47.97354751998429 90.03982894447466,-47.97354751998429" stroke="purple" stroke-width="3" marker-end="url(#arrow)"><title>f</title></polyline>
  <text x="-1.1368683772161603e-13" y="-47.97354751998429" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">f</text>
  <polyline points="-94.34447446980596,-35.57774791227483 83.54960825780415,104.91856291954896" stroke="blue" stroke-width="3" marker-end="url(#arrow)"><title></title></polyline>
  <text x="-5.3974331060009035" y="34.670407503637065" fill="blue" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke"></text>
  <polyline points="108.73641627786131,-28.016064827507464 100.54837539908644,97.35687983478158" stroke="purple" stroke-width="3" marker-end="url(#arrow)"><title>g</title></polyline>
  <text x="104.64239583847387" y="34.67040750363706" fill="purple" stroke="white" stroke-width="10" stroke-opacity=".8" paint-order="stroke">g</text>
  <text x="-110.03982894447489" y="-47.97354751998429" fill="purple">x</text>
  <text x="110.03982894447466" y="-47.97354751998429" fill="purple">y</text>
  <text x="99.24496273247308" y="117.31436252725841" fill="purple">z</text>
</svg>

```rzk
#define hom2
  (A : U)
  (x y z : A)
  (f : hom A x y)
  (g : hom A y z)
  : U
  := ((t, s) : Δ²) →
    A [ s ≡ 0₂ ↦ f t
      , t ≡ 1₂ ↦ g s ]
```

## Cofibration composition

A useful theorem about extension types says that
any extension type is equivalent to an extension of its restriction.

```rzk title="RS17, Theorem 4.4"
#def cofibration-composition
  ( I : CUBE)
  ( χ : I → TOPE)
  ( ψ : χ → TOPE)
  ( ϕ : ψ → TOPE)
  ( X : χ → U)
  ( a : (t : ϕ) → X t)
  : Equiv
    ( (t : χ) → X t [ϕ t ↦ a t])
    ( Σ ( f : (t : ψ) → X t [ϕ t ↦ a t]) ,
        ( (t : χ) → X t [ψ t ↦ f t]))
  :=
    ( \ h → (\ t → h t , \ t → h t) ,
      ( ( \ (_f , g) t → g t , \ h → refl) ,
        ( ( \ (_f , g) t → g t , \ h → refl))))
```

<!-- Definitions for the SVG images below -->
<svg width="0" height="0">
  <defs>
    <style data-bx-fonts="Noto Serif">@import url(https://fonts.googleapis.com/css2?family=Noto+Serif&display=swap);</style>
    <marker id="arrow" viewBox="0 0 10 10" refX="5" refY="5"
      markerWidth="5" markerHeight="5" orient="auto-start-reverse">
      <path d="M 0 2 L 5 5 L 0 8 z" stroke="black" fill="black" />
    </marker>
  </defs>
  <style>
    text, textPath {
      font-family: Noto Serif;
      font-size: 28px;
      dominant-baseline: middle;
      text-anchor: middle;
    }
  </style>
</svg>
