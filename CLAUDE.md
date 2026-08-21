# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with
code in this repository.

## What this is

An academic paper (work in progress), *"The terminal through the lens of
variation theory: misconceptions and critical aspects of the Unix shell"*
(Bosk), cataloguing learners' misconceptions of the Unix shell — command
model, file system, composition (pipes/scripts), and SSH — and analysing how
to teach against them using phenomenography and variation theory. Companion
to vt-prog-misconceptions (same method, introductory programming) and
vt-debug (debugging); it grows out of the datintro course-revision work in
the introtools repository (see `introtools:evaluation/improvements.md` and
`introtools:literature/mental-models.md`).

## Build

```sh
make            # builds both article.pdf and slides.pdf
make article.pdf
make clean
```

Build machinery comes from the `makefiles/` and `didactic/` git submodules;
after a fresh clone:

```sh
git submodule update --init --recursive
```

Requires a TeX distribution with `minted` (Pygments, `-shell-escape`),
`pythontex`, and `biber`. Outputs land in `ltxobj/`; `article.pdf` and
`slides.pdf` at the root are symlinks.

## Architecture: one source, two outputs

The content `.tex` files are compiled twice from the same source — once as a
prose article (`article.tex`, memoir + beamerarticle) and once as Beamer
slides (`slides.tex`). Both input the same `preamble.tex` and the same
content files (`introduction.tex`, `background.tex`, `method.tex`,
`shell.tex`, `filesystem.tex`, `composition.tex`, `ssh.tex`,
`related-work.tex`, `conclusions.tex`, `search-protocol.tex`).

Consequences when editing content files:

- Every content file starts with `\mode*`; slide-only material goes in
  `\mode<presentation>{...}` or `\begin{frame}...`; article-only prose is the
  default outside frames.
- Research questions live in `restatable` environments (thm-restate) —
  stated once in `introduction.tex`, restated with `\rqmisconceptions*` etc.
  in `method.tex` and `conclusions.tex`. Never write literal "RQ1".
- Citations use `\autocite` (biblatex verbose style; didactic places them in
  the margin/footnotes). No `\parencite`/`\textcite` except deliberate
  in-prose mentions.

## Bibliography discipline

`misconceptions.bib` entries carry provenance blocks (CLAIM / FOUND-VIA /
PICKED / QUOTE / VERIFIED) per the backing-claims skill; do not add a
citation without one, and do not cite beyond what the VERIFIED line
supports. Entries with `VERIFIED: TODO` (du Boulay 1981, Ben-Ari 1998,
Dillon 2012) are tracked as introtools issues #124–#126 — check there
before quoting their inner claims. `theory.bib` holds the variation-theory
base (NCOL etc.), shared with the companion papers.

## State of the paper

Scaffold stage: research questions, method skeleton, seeded literature, and
preliminary aspect/pattern analyses per chapter are in place. The systematic
literature-search phase (`search-protocol.tex`) has run in several rounds,
including a measurement round (has learners' shell understanding been
measured? — one phenomenographic study, Doyle & Lister 2006; no shell
concept inventory). `% TODO`/`% XXX` comments mark the open work.

## The instrument (`quiz.nw`)

`quiz.nw` is a literate program (noweb; `make programs` tangles
`quiz-knowledge-{start,end}.json` and `analyze_quiz.py`, all gitignored).
Each quiz has an opener (consent / preparation, position 1), five open
essay items (positions 3–7, the phenomenographic accounts — placed *before*
the closed items and shown one at a time without backtracking so the
distractors cannot seed the accounts) and the closed knowledge items
(positions 11–24, one per candidate critical aspect, in chapter order;
item 11 — the closed twin of open item 4 — is in the end quiz only, so
the start quiz has 13 closed items and the end quiz 14).
Items are keyed by title in the Canvas report; `analyze_quiz.py` reads the
answer key from the tangled start-quiz JSON, filters by consent, prints
per-item facility and distractor counts, paired pre/post gains
(`--quiz both --results start.csv end.csv`) and writes a long-format
coding sheet for the accounts (optional `--llm` pre-coding in separate
`suggested_*` columns). Activate the `literate-programming` skill before
editing `quiz.nw`.
