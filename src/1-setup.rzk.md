# Setup and Rzk syntax overview

## Disclaimers

!!! info

    This demo is for the HoTT seminar at Bauman Moscow State Technical University, Nov 20–21, 2023
    and contains a partial introduction to homotopy type theory and simplicial type theory in Rzk.

!!! warning

    This demo relies on [`rzk` version 0.6.7](https://rzk-lang.github.io/rzk/v0.6.7/)
    and might not be up-to-date, as the proof assistant is in active development
    and breaking changes should be expected with further releases.

## Installing Rzk

To check the formalisations in this demo you can:

1. Have `rzk` installed locally
   (the recommended way is to have [VS Code extension for Rzk](https://marketplace.visualstudio.com/items?itemName=NikolaiKudasovfizruk.rzk-1-experimental-highlighting) to handle it for you).
   and running `rzk typecheck` from the root of this project (or relying on VS Code extension to typecheck all files).

2. Use an online playground at <https://rzk-lang.github.io/rzk/v0.6.7/playground/>
   (and copy-paste code blocks there one by one)

### Formalisation project structure

Usually, formalisation projects in Rzk consist of multiple Rzk (or literate Rzk) files.
For example, this demo project has the following structure:

```
bmstu-rzk-demo-2023
│
...

└── src
    ├── 1-setup.rzk.md
    ├── day-1
    │   ├── 1-dependent-types.rzk.md
    │   ├── 2-propositions-as-types.rzk.md
    │   ├── 3-identity-types.rzk.md
    │   ├── 4-homotopy-type-theory.rzk.md
    │   └── 5-exercises.rzk.md
    ├── day-2
    │   ├── 1-univalence.rzk.md
    │   ├── 2-sets-and-logic.rzk.md
    │   ├── 3-hott-exercises.rzk.md
    │   └── 4-simplicial-types.rzk.md
    └── index.md
...
```

The formalisations are located in the `src/` directory and contain just the two
literate Rzk Markdown files.
For another example, [:material-github: rzk-lang/sHoTT](https://github.com/rzk-lang/sHoTT)
has its formalisations further split into subdirectories:

```
sHoTT
│
...
└── src
    ├── STYLEGUIDE.md
    ├── hott
    │   ├── 00-common.rzk.md
    │   ├── 01-paths.rzk.md
    │   ├── 02-homotopies.rzk.md
    │   ├── 03-equivalences.rzk.md
    │   ├── 04-half-adjoint-equivalences.rzk.md
    │   ├── 05-sigma.rzk.md
    │   ├── 06-contractible.rzk.md
    │   ├── 07-fibers.rzk.md
    │   ├── 08-families-of-maps.rzk.md
    │   ├── 09-propositions.rzk.md
    │   └── 10-trivial-fibrations.rzk.md
    ├── index.md
    └── simplicial-hott
        ├── 03-simplicial-type-theory.rzk.md
        ├── 04-extension-types.rzk.md
        ├── 05-segal-types.rzk.md
        ├── 06-2cat-of-segal-types.rzk.md
        ├── 07-discrete.rzk.md
        ├── 08-covariant.rzk.md
        ├── 09-yoneda.rzk.md
        ├── 10-rezk-types.rzk.md
        └── 12-cocartesian.rzk.md
```

### Running Rzk

To typecheck files, at the moment you have to run the following command in
the terminal:

```sh
rzk typecheck
```

The list of files to check can be specified in `rzk.yaml` (as in this project),
or listed explicitly as arguments to `rzk typecheck`:

```sh
rzk typecheck FILE-1 FILE-2 ... FILE-N
```

!!! warning

    It is important to pass formalisation files in the order
    you want them to be checked, as dependencies between files
    (in the form of imports) are not implemented yet.

!!! tip

    Starting filenames with numbers (as in the examples above) helps automatically
    achieve the desired order when using wildcars (e.g. `rzk typecheck src/*.rzk.md`), although in a slightly inelegant way.

### Literate Rzk

If you are familiar with Markdown, then the recommended approach is to use [literate](https://en.wikipedia.org/wiki/Literate_programming) Rzk Markdown,
so that conventional Markdown rendering tools can be used to produce
readable documentation from the formalisation files.

For instance, this file is a literate Rzk Markdown file!

## Syntax overview

Each file in Rzk (or literate Rzk) must start
with a declaration of the version/dialect used.
We will use (the only supported) `rzk-1` dialect:

```rzk
#lang rzk-1
```

The rest of the file contains primarily of `#!rzk #define`-statements,
each of which introduces a new definition into scope. For example,
consider the following definition:

```rzk
#define modus-ponens
  (A B : U)
  : (A → B) → A → B
  := \ f x → f x
```

Going line by line, we have

- `modus-ponens` as the name of a new definition
- two parameters — `A` and `B` (both of type `#!rzk U`, meaning `A` and `B` are types[^1])
- declared type of `modus-ponens` is `#!rzk (A → B) → A → B`
- declared term (value) of `modus-ponens` is `#!rzk \ f x → f x`

[^1]:
    technically, in Rzk currently `#!rzk U` contains also `#!rzk CUBE`,
    `#!rzk TOPE` universes, and itself!

Rzk typechecks the term against the declared type and,
if no type errors found, remembers this definition for future use.

### Unicode

Rzk supports both ASCII and Unicode versions of many syntactic constructions.
We will use the following Unicode symbols:

- `->` should be always replaced with `→` (`\to`)
- `|->` should be always replaced with `↦` (`\mapsto`)
- `===` should be always replaced with `≡` (`\equiv`)
- `<=` should be always replaced with `≤` (`\<=`)
- `/\` should be always replaced with `∧` (`\and`)
- `\/` should be always replaced with `∨` (`\or`)
- `0_2` should be always replaced with `0₂` (`0\2`)
- `1_2` should be always replaced with `1₂` (`1\2`)
- `I * J` should be always replaced with `I × J` (`\x` or `\times`)

We use ASCII versions for `TOP` and `BOT` since `⊤` and `⊥` do not read better
in the code.
