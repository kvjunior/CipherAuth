# Cross-file integration report

## Outcome and scope

The implemented chain passes: CEA sender creation → canonical 02F record mapping →
separate authentication/intake compression → compressed-only proxy verification →
authorized CEA delegation → canonical serialization → delegatee opening.
Origin and delegated opening return the same ordered statements.

This is an integration pass over the available code through 02N. It does not claim
that every historical document is mutually consistent: 02E explicitly supersedes
errors in 02A–02D and earlier CLASS drafts. It does not claim all planned batches,
the complete security game, or the security proof are finished.

## Changes in this release

1. Recovered the original packages and expanded the nested CEA/02F/02G sources
   into named component directories. Original source and archived inputs remain
   unchanged; generated local results are allowed to refresh.
2. Added `integration/bridge.py`, which carries full KeyRef encodings, the complete
   encrypted body, commitment, point A, tag and epoch into the existing 02F proof.
   It requires the proof's ordered record list to equal the CEA record list.
3. Added 14 cross-component tests. The proxy consumes no sender witness and no
   individual proof responses. Distinct group implementations interoperate through
   compressed point encodings, not incompatible Python Point instances.
4. Added one fail-fast runner with explicit current/historical counts, deterministic
   output checks, common-source equality checks and compilation of all Python files.
   Included two legacy integration tests omitted by the earlier unit-test runner.
5. Added this report, source lineage and a current status document so superseded
   claims cannot be mistaken for the latest conclusion.
6. Added Batch 02I with transactional adaptive challenge installation, an exact
   conflict ledger, 27 tests, 8 public traces and a restricted proof note. Claude's
   analysis is retained under `review/`; it is input to this batch, not a review of
   the completed implementation.
7. Added Batch 02J after reconciling D1 with the actual scalar `Token.Omega` type.
8. Added Batch 02K with a multi-grant Gap-Divisible-DH embedding for the exact
   context-bound Hmask exposure event, including prequery handling.
   It tests exact challenge transformation, same/cross-grant consistency, simulated
   DecO bookkeeping and the uniform-C distribution against the frozen CEA code.
9. Added Batch 02L with a versioned G-CONF-CLASS interface and dependency closure.
10. Added Batch 02M with the candidate final hybrid and quantitative loss ledger.
11. Added Batch 02N with the exact fixed-target G-SCOPE-v1 composition.

## Validation evidence

| Component | Distinct tests |
|---|---:|
| Frozen CEA unit tests | 39 |
| Frozen CEA integration tests | 2 |
| Batch 01r2 | 29 |
| Batch 02E | 13 |
| Batch 02F | 34 |
| Batch 02G | 23 |
| Batch 02H | 27 |
| Batch 02I | 27 |
| Batch 02J | 12 |
| Batch 02K | 13 |
| Batch 02L | 14 |
| Batch 02M | 9 |
| Batch 02N | 14 |
| New bridge | 14 |
| Current subtotal | 270 |
| Historical 02A/02B/02C/02D | 4 / 4 / 3 / 5 |
| Total | 286 |

The supplied environment has zero failed, errored or skipped tests. Nested reruns
exercise some tests more than once; the table counts each distinct test once.
02G's 12, 02H's 13 and 02I's 8 traces replay. The runner requires saved 02E–02N JSON results
to reproduce byte-for-byte, and checks common code copies and frozen game bytes.
Three older research probes supplement these test counts; they are not new proofs.
Environment-dependent metadata and test duration logs are not byte-identity targets.

## Negative controls and boundary checks

- An E-only mutation retains CEA sender authentication but invalidates intake;
  the wrapper refuses it before calling the transformation method.
- A valid proof for an unrelated record is rejected, preventing independently
  valid authentication/proof objects from being combined across record lists.
- Reordered records with freshly recompressed CEA authentication reject the old
  intake proof. Recompressing the matching intake proofs restores acceptance.
- Modified responses, framing, proof context, redundant A/commitment/sender fields,
  grant signatures, tokens and sender authenticators are rejected.
- Delegated records are not accepted as fresh origin intake. Transformed-body
  tampering is rejected by recipient commitment checking, even where public sender
  authentication alone still succeeds.
- Exact replay remains functionally valid. Challenge refusal is a game policy, not
  silently added to this bridge.
- Existing unweighted attacks and restricted exceptional-event fixtures retain
  their negative-control meanings; weighted rejection of one attack is not a proof.

## Remaining work

Batch 02I now handles challenge issuance after registered phase-1 queries inside
the restricted two-point experiment, including programming and representation
conflicts. Batch 02J supplies a candidate correction to D1: the simulator samples
the scalar Omega and transforms every honest-delegatee input exactly after replacing
C by uniform data. Batch 02K reduces the associated Hmask(T) exposure event under
Gap-Divisible-DH with an `N_H` loss. Batch 02L shows that complete algebraic
opening extraction is unnecessary for the CLASS target because honest transforms
are exact and corrupted target service is forbidden. Batch 02M supplies the
candidate decryption simulation, hybrid order and complete symbolic loss. Batch
02N supplies the exact fixed-target G-SCOPE composition. The blocking security
task is independent review; the literature gate remains separate unfinished work.

The legacy pure-Python arithmetic, token design and prototype API constraints are
unchanged. This release packages a reproducible research checkpoint, not a
production cryptographic implementation or a submission-ready theorem.
