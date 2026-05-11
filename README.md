# mention

`mention` adds co-author targeted comments on top of the LaTeX
[`phfcc`](https://ctan.org/pkg/phfcc) package.

It is meant for collaborative drafts where comments should stay inline, while
only the addressed co-author sees matching margin highlights and a per-viewer
unresolved-comment count in the page header.

## Files

- `mention.sty`: the package
- `mention.tex`: CTAN-style manual source
- `mention.pdf`: built package manual
- `targeted-comments-demo.tex`: minimal example document
- `mention-local.cfg.example`: optional per-user viewer config template

The package depends on `phfcc`, `xparse`, `marginnote`, and `xcolor`.
Automatic viewer detection can also use `catchfile` and `texosquery` when they
are available, but manual viewer selection works without them.

## Basic Use

Put `mention.sty` next to your main `.tex` file, or install it somewhere
TeX can find it. Then load it after your usual document setup:

```tex
\usepackage{mention}
```

Define author comment commands:

```tex
\phfMakeTargetedCommentCommand[
  initials={AL},
  color=blue,
  handle=alice,
  userid=alice-laptop
]{Alice}

\phfMakeTargetedCommentCommand[
  initials={BO},
  color=orange,
  handle=bob,
  userid=bob
]{Bob}
```

Use comma-separated recipient handles in the optional argument:

```tex
\Alice[bob]{Please check this lemma.}
\Alice![bob]{Please add the missing citation.}
\Alice[bob, resolved]{This has already been handled.}
```

Semantics:

- `\Alice[bob, claire]{...}` targets Bob and Claire.
- `\Alice![bob]{...}` marks the comment as important.
- `resolved` keeps the comment visible inline but makes it dormant: no margin
  highlight and no unresolved-count contribution.

Enable the header count with:

```tex
\phfUseTargetedCommentHeader
```

The count is written through the `.aux` file, so compile twice after changing
comments or viewer selection.

## Viewer Selection

Viewer selection order:

1. `\phfSetViewer{...}` in the document
2. `\phfSetViewerUser{...}` in the document
3. `mention-local.cfg`, if present in the compile directory
4. best-effort automatic user detection

For shared projects, the most predictable setup is to keep viewer selection out
of the tracked source and let each collaborator create an ignored
`mention-local.cfg`. Start from the tracked template:

```bash
cp mention-local.cfg.example mention-local.cfg
```

Then edit it to select your viewer:

```tex
\phfSetViewer{bob}
```

or:

```tex
\phfSetViewerUser{bob}
```

Automatic detection tries to infer the compiling user id and map it through
registered `userid=...` values. If detection helpers or shell access are not
available, the package still loads; use `mention-local.cfg` or an explicit
viewer command.

## Demo

From the repository root:

```bash
latexmk -pdf targeted-comments-demo.tex
```

If automatic detection is unavailable on your system, create
`mention-local.cfg` with:

```tex
\phfSetViewer{ashwani}
```

and compile again.

## Manual

Build the package manual with:

```bash
latexmk -pdf mention.tex
```

The generated `mention.pdf` is intended to be distributed with the package.

## License

Copyright (C) 2026 Ashwani Anand.

This work may be distributed and/or modified under the conditions of the LaTeX
Project Public License, either version 1.3c of this license or any later
version.

This work has the LPPL maintenance status `maintained`.

The Current Maintainer of this work is Ashwani Anand.

This work consists of the files `README.md`, `mention.tex`, `mention.sty`,
`mention-local.cfg.example`, and `targeted-comments-demo.tex`, and the derived
file `mention.pdf`.
