# CipherAuth

Anonymous manuscript for review.

This repository contains the paper, complete LaTeX source, the two final figures, the reproducibility artifact, and the supporting analysis used to prepare the submission.

## Paper

- `CipherAuth_STOC2027_Final_Paper.pdf` — compiled paper with the final figures.
- `source/main.tex` — full paper source.
- `source/references.bib` — bibliography.
- `source/figures/` — embedded vector figures.

## Reproducibility artifact

`artifact/CipherAuth_Integrated_02N_2026-10-29.zip` contains the executable code, frozen interfaces, tests, deterministic traces, integration report, and proof-status materials.

The artifact reports 286 unique tests and 33 deterministic trace replays with no failures, errors, or skips. The executable checks establish the functional identities and the algebraic point-shifting attack. The class-granular and fixed-target scope reductions are explicitly labelled candidate results pending independent cryptographic review.

## Build the paper

From the repository root:

```sh
mkdir -p build
pdflatex -interaction=nonstopmode -halt-on-error -output-directory=build source/main.tex
BIBINPUTS=source: bibtex build/main
pdflatex -interaction=nonstopmode -halt-on-error -output-directory=build source/main.tex
pdflatex -interaction=nonstopmode -halt-on-error -output-directory=build source/main.tex
```

The source uses a letter-paper, single-column review layout. The conference’s submission system and current call for papers control the final page and format limits.

## Artifact execution

```sh
unzip -q artifact/CipherAuth_Integrated_02N_2026-10-29.zip -d artifact_run
cd artifact_run/CipherAuth_Integrated_02N_2026-10-29
```

The component README files and `INTEGRATION_REPORT.md` describe the available runners and expected outputs. The source manifest is intentionally retained and checked; no files are added to the frozen codebase during verification.

## Supporting material

- `analysis/NOVELTY_AND_VENUE_POSITIONING.md` — calibrated novelty and venue analysis.
- `analysis/CipherAuth_literature_evidence_matrix_2026-10-29.md` — literature evidence matrix.
- `analysis/LITERATURE_CALIBRATION_AND_REPRODUCTION_STATUS.md` — calibration and reproduction status.
- `analysis/STOC_SUBMISSION_READINESS.md` and `analysis/SUBMISSION_GATE.md` — submission-readiness records.

## Anonymous artifact link

The anonymous artifact landing page is:

https://anonymous.4open.science/r/CipherAuth-5354/

This README is intended to be published at:

https://anonymous.4open.science/r/CipherAuth-5354/README.md

