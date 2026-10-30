# CipherAuth novelty and venue-positioning decision

Date: 29 October 2026

## Decision

The defensible paper is a security-analysis paper, not a new-primitive paper.
The exact aggregate/delegate commutation property is achieved by a generic
composition of commitment signing, single-hop proxy re-encryption, and Schnorr
half-aggregation. It cannot support a priority claim.

The paper's strongest contribution is the security boundary for that
composition:

1. G-CONF-CT, the per-ciphertext proxy notion, is false by a concrete A-mauling
   attack.
2. G-CONF-CLASS-v1 remains a plausible positive notion and has an explicit
   candidate ROM reduction under a gap variant of Divisible Computational
   Diffie--Hellman (DCDH). It is strictly weaker than HRA because it refuses
   every target-class transformation to a corrupt recipient.
3. Fixed-target G-SCOPE-v1 composes exactly from G-CONF-CLASS-v1 with no new loss.
4. In-class proxy/delegatee collusion recovers the class key, so revocation and
   in-class collusion resistance are non-goals.

The CLASS and SCOPE reductions have not received independent cryptographic
review. They must not be described as established theorems until that review is
complete.

## Prior-work comparison

| Line of work | What it already supplies | What remains specific here |
| --- | --- | --- |
| An-Dodis-Rabin (EUROCRYPT 2002; ePrint 2002/046) | Signing a commitment to separate authentication from encryption | No novelty for the invariant signed object |
| Canetti-Hohenberger (CCS 2007; ePrint 2007/171) | Chosen-ciphertext PRE games, derivatives, registration restrictions | The exact CLASS/CT separation for this comparator |
| Cohen (PKC 2019) and Ling et al. (ASIACRYPT 2025) | HRA as a modern minimum PRE target; tightly adaptive multi-challenge HRA | Establish that G-CONF-CLASS is below HRA, while G-CONF-CT is the attacked CCA-like target |
| Canetti-Krawczyk-Nielsen (CRYPTO 2003; ePrint 2003/174) | RCCA-style replay masking | The vector/delegation adaptation used by the frozen game |
| Chen-Zhao (ePrint 2022/222) | Incremental Schnorr half-aggregation in AGM+ROM | No novelty for signature compression or commutation |
| Chalkias-Garillot-Kondi-Nikolaenko (ePrint 2021/350) | Noninteractive half-aggregation and evidence about further compression | No formal lower bound is established here |
| Fuchsbauer-Plouviez-Seurin (EUROCRYPT 2020; ePrint 2019/877) | AGM treatment of Schnorr and signed ElGamal | A likely repair direction for proxy-checkable ciphertext validity, not part of the current scheme |
| Bao-Deng-Zhu (ICICS 2003) | DCDH and its equivalence to CDH variants at high granularity | Standard lineage for the `(b/a)P` target; CipherAuth still needs a decision oracle |
| Nick-Ruffing-Seurin (EUROCRYPT 2026) and Zhao-Wang-Li (ICICS 2026) | Constant-size interactive aggregation and cross-domain transcript rebinding | Recent alternatives, neither of which validates the mutable PRE point |
| Choi-Jeong-Byun (Information Sciences 2026) | Pairing-based identity aggregate signcryption with re-encryption and collusion analysis | Direct application overlap; construction, model, and security boundary differ |

No absence-of-results claim is made. The supplied 29-paper corpus was reviewed
in full and the IDASR construction/dataflow was compared directly; a broader
systematic search could still reveal additional adjacent work.

## Venue ranking

### 1. ACNS 2027 Cycle 2 - recommended

ACNS explicitly welcomes modelling, analysis, security proofs, attacks,
cryptographic protocols, and implementations. That scope matches a paper whose
main result is an attack plus a carefully delimited positive theorem. Cycle 2 is
due 21 January 2027 at 23:59 AoE. The submission must be anonymous, use the
unmodified LNCS format, and fit within 20 pages excluding references. The
camera-ready limit is 30 pages including references and appendices.

Use the time before January to obtain independent proof review, verify the
Chen-Zhao loss transcription if aggregate authenticity remains in the paper,
and compare the full IDASR security model.

Official call: https://acns2027.isg.rhul.ac.uk/calls/papers/

### 2. STOC 2027 - do not submit the current result

STOC's deadline is 2 November 2026 at 23:59 AoE. Submissions are public and
non-anonymous, must contain proofs sufficient to verify the main claims, and
must present new and original theory. The current manuscript does not clear that
bar: commutation is a known composition, the positive reductions are unreviewed,
and the remaining contribution is better matched to applied cryptographic
analysis. A rushed STOC submission would overstate both novelty and proof status.

STOC 2027 also requires a 20-30 minute author video by 16 November, public
posting consent, and disclosure of substantive generative-AI assistance.

Official call: https://acm-stoc.org/stoc2027/stoc2027-cfp.html

### 3. Top IACR theory venues - hold pending a stronger result

PKC, TCC, EUROCRYPT, CRYPTO, and ASIACRYPT would require a stronger and cleaner
cryptographic theorem than the current known-composition-plus-boundary result.
A credible submission would need at least one of the following: a standard-
assumption reduction, a proxy-checkable aggregate-validity construction with a
new extraction theorem, or a general separation/lower bound beyond this
concrete comparator.

### 4. Journal fallback

If independent review finds that the CLASS proof is correct but too specialized
for ACNS, a cryptographic engineering or security journal is a reasonable
fallback. The executable artifact, explicit attacks, and full proof ledger are
assets for that route. The paper should still avoid a novelty claim for exact
commutation.

## Submission gate

The ACNS package is ready for technical review, not upload. Release for
submission requires all of the following:

- an independent line-by-line review of the G-CONF-CLASS-v1 hybrid;
- an independent check of the zero-loss G-SCOPE-v1 composition;
- final human verification of every bibliography entry and the IDASR comparison;
- confirmation that the main paper remains within 20 LNCS pages excluding references;
- an anonymous artifact URL, if the artifact is linked from the paper;
- a final human-authored disclosure statement consistent with venue policy;
- a rerun of the packaged artifact with zero failures, errors, or skips
  (completed on 29 October 2026: 286 unique tests, 33 trace replays, zero skips).

Until those items are complete, retain `security_proved: false`.
