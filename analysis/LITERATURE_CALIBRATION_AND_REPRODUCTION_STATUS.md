# Literature calibration and reproduction status

Date: 2026-10-29

## Literature review

- Full PDFs reviewed: 29
- Core manuscript changes: HRA security hierarchy; gap-DCDH nomenclature; aggregate-signature landscape; precise IDASR comparison
- Bibliographic corrections: Chen--Zhao, Choi--Jeong--Byun, Schnorr entry type, Maya Farber Brodsky, HRA journal publication
- Runtime/code changes required: none
- Frozen artifact modified: no

## Paper build

- LaTeX build: PASS
- Undefined citations/references: none
- Overfull boxes: none
- Pages: 15, including references and appendix
- Protocol/security-boundary figure: added, referenced in the construction section, and visually verified
- Positive security reductions: candidate, independent review still required

## Artifact reproduction

Command: `python3 scripts/reproduce_all.py`

- Status: PASS
- Current tests: 270
- Preserved historical diagnostics: 16
- Total unique tests: 286
- Trace replays: 33
- Python files compiled: 266
- Failures: 0
- Errors: 0
- Skipped: 0
- `security_proved`: false

The artifact's frozen summary still lists “novelty decision” as missing because the literature report is intentionally outside the source-manifest-protected codebase. The present package supplies that decision without modifying the artifact.

## Submission gate

The literature and integration gates are complete. The remaining blocking gate is independent, line-by-line cryptographic review of the candidate `G-CONF-CLASS-v1` and `G-SCOPE-v1` reductions. Do not remove the candidate labels or change `security_proved` before that review.
