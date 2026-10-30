# CipherAuth: 02I analysis + a correction to my earlier G-CONF-CT review

Reviewer: Claude, 26 Oct 2026. Reviewer memo, **not** a reduction and **not** a theorem. I
reproduced the integrated package in a clean copy first (below).

## Integration verified

`scripts/reproduce_all.py` on a fresh extraction: **197 unique tests PASS, 0 failed/errored/
skipped**; **25 trace replays** (02G 12 + 02H 13); byte-for-byte JSON reproduction and frozen-
game-bytes checks pass; `security_proved: false`; `missing: [02I adaptive challenge timing, full
AGM/game transfer, final reduction and loss, novelty decision]`. STATUS.md and
INTEGRATION_REPORT.md are accurate and appropriately scoped. 02E–02H are honest, carefully
restricted written lemmas pending independent review. Nothing is over-claimed.

## 1. 02I (adaptive challenge timing): looks routine, and is not the real blocker

02H installs the challenge intake proof **before** any adversarial query ("setup-first"), and
explicitly excludes a challenge issued **after** phase-1 queries. 02I must handle the IND-CCA
reality: phase-1 adaptive queries, then the challenge in phase 2, with possible (a) programming
conflicts (the challenge's hash input already queried) and (b) representation collisions (R*
already registered).

This is the **standard high-entropy-commitment adaptive-ROM-programming** argument, the same one
used for adaptive Fiat–Shamir / Schnorr-signature simulation:

- The simulator picks `c*, z*` uniformly in phase 2 and sets `R* = z*K − c*X`. `R*` is therefore
  a fresh (near-)uniform point, determined before programming.
- **Pre-query collision:** the adversary queried `H_c` at exactly `(ctx, Rec*, R*)` during phase 1.
  Since `R*` is fresh in phase 2, this is `≤ q_Hc/q`.
- **Representation collision:** `R*` equals an already-registered encoded point. `≤ N_reg/q`
  (02H already aborts on `R* ∈ {O, K, X}`; 02I generalizes the abort to the whole registry).
- After programming, `A* = X` and `R*` may appear in later prefix/aggregate hashes the adversary
  forms; these are answered lazily with `R*` registered at its known representation `(z*, −c*)`,
  exactly as 02H already permits for "new, non-exempt" entries. No new conflict.

So 02I adds only those two negligible terms to 02H's setup-first bound; the setup-distribution
coupling (02H §2, the `(r,c) ↦ (c, z)` bijection + the `2/(q−1)` abort) carries over. **My read:
02I is provable, not attackable, and not novel** — it is the textbook adaptive-programming step.
It is the *routine* remaining piece, not the obstacle.

I could not find a half-aggregation-specific wrinkle that breaks this: the challenge's freshness
is what defeats both pre-query and representation collisions, and half-aggregation only adds
prefix/weight hashes that are answered after `R*` is fixed.

## 2. Correction to my 24–25 Oct G-CONF-CT review: D1 is **not** avoided

In my G-CONF-CT review I wrote that the challenge "is never transformed," so the reduction avoids
D1. **That was incomplete, and 02H §6 is right to flag it.** The reasoning covered only the
*corrupted*-delegatee path (there the adversary can't prove knowledge of `t*`, so the proxy
refuses). But the frozen G-CONF-CT game is "as G-CONF-CLASS, except …", so it **inherits** the
honest-delegatee `ReEncO`, and:

- In the real game the challenge **has** a valid intake proof (the honest challenger knows `t*`).
- So the adversary can submit the challenge to `ReEncO(target class, honest ℓ, ·)`; the proxy
  serves it and returns `Ā* = ω⁻¹Q*`, registering it as a derivative.
- In the reduction the simulator embeds `A* = X` and does **not** know `Q* = (b/a)P`, so it still
  cannot compute `Ā*`. The intake proof does not help here — the challenge legitimately passes
  intake. This is exactly **D1**, re-entering through the inherited honest-ℓ path.

So the intake-proof augmentation fixes G-CONF-CT's *defining* attack (corrupted-delegatee
A-mauling) but does **not** remove D1 from either game. My earlier "augmented scheme clears D1 in
both granularities" synthesis was too optimistic; 02H's §6 statement ("exempting its opening does
not simulate that reply or close D1") is the correct one. I'm flagging this because it changes
what "the full proof" must still do.

## 3. What the full proof therefore still owes (independent of 02I)

1. **D1 / honest-delegatee challenge transformation** — simulate the visible `Ā* = ω⁻¹Q*` reply.
   This is the hard, still-open obstacle, and it is the decisional/no-DDH-oracle conflict from the
   G-CONF-CLASS thread, now confirmed present in G-CONF-CT too. Candidate routes remain: a
   trapdoor/twin-DH reformulation (likely a construction change), a game that does not return
   honest-ℓ challenge transformations (a weaker, differently-scoped notion), or a counterexample.
2. **Full algebraic input space** — the 02G/02H lemmas allow only symbolic `{K, X}`; the real AGM
   has `P`, oracle-output points (`Ā_j`, grant data `C, γ, W`), and registry aliases. Extraction
   must be re-established over that larger basis.
3. **Grant/admissibility ambiguity** (02H `game_compatibility.md`) — resolve how the proxy
   obtains/authorizes a target-class token for a corrupted recipient, and which fields each oracle
   reveals, before a full CT reduction exists.
4. **Novelty / IDASR26** — still unsettled; the extraction core still looks like a known
   composition (AGM Schnorr extraction + CZ prefix aggregation + DHIES confirmation).

## Bottom line

02I is the easy part and looks provable by standard means. The thing that actually gates a
G-CONF-CT theorem is D1 (now confirmed un-avoided) plus the full-basis transfer and the grant
ambiguity — and, above all, the novelty/IDASR gate, which decides whether any completed proof is
a STOC contribution or a known composition. None of that is closed by more passing tests.
