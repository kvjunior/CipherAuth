# CipherAuth literature evidence matrix

Date: 2026-10-29  
Corpus: 29 supplied papers, read from their full PDFs  
Purpose: calibrate the paper's novelty, security language, construction choices, bibliography, and executable artifact.

## Executive decision

The literature review requires **manuscript corrections but no implementation or numerical-result changes**.

1. The modern PRE comparison must center honest-reencryption-attack (HRA) security, not stop at CPA/CCA. CipherAuth's `G-CONF-CT` is CCA-like and permits a modified target ciphertext to a corrupt delegatee; the concrete point-shifting attack defeats it. `G-CONF-CLASS` refuses every target-class transformation to corrupt recipients and is therefore intentionally **weaker than HRA**. The manuscript must not market the candidate theorem as general-purpose modern PRE security.
2. The computational target `(b/a)P` is the established Divisible Computational Diffie--Hellman (DCDH) problem. CipherAuth additionally gives the reduction a fixed-key decision oracle, so the exact assumption is a **gap-DCDH variant**. The artifact name `GDivDH` is retained only for reproducibility.
3. Signed ElGamal and signatures on randomizable ciphertexts clarify two repair directions. Neither repair can be claimed by the present construction: a proxy-valid proof of encryption randomness would need a separate per-record knowledge-soundness argument after compression.
4. Recent aggregate-signature work improves other axes—constant size, interaction, plain-model security, or cross-domain composition—but does not remove the mutable-component validity problem identified here.
5. The closest combined application, ID-based aggregate signcryption with re-encryption for UAV networks, has a different trust/dataflow: its UAV verifies and decrypts reports before aggregate re-encryption. CipherAuth's proxy remains plaintext-oblivious. This supports a precise distinction, not a first-of-kind claim.
6. The positive `G-CONF-CLASS` and scope reductions remain candidate theorems pending independent cryptographic review. Literature does not close that proof obligation. The artifact must continue to report `security_proved: false`.

## Paper-by-paper extraction

| # | Paper | Essential result for CipherAuth | Exact use in the paper | Limitation / calibration |
|---:|---|---|---|---|
| 1 | Boneh, Gentry, Lynn, Shacham, *Aggregate and Verifiably Encrypted Signatures from Bilinear Maps* (EUROCRYPT 2003) | Introduces aggregate signatures for distinct users/messages; pairing-based constant aggregate and verifiably encrypted signatures. | Fundamental aggregate-signature baseline. | Pairing-based and not PRE; does not address authentication under recipient-changing transformation. |
| 2 | Fuchsbauer, Plouviez, Seurin, *Blind Schnorr Signatures and Signed ElGamal Encryption in the AGM* (EUROCRYPT 2020) | Proves Schnorr-signed ElGamal CCA2 secure in AGM+ROM; public proof of encryption randomness validates the ciphertext. | Supports the concrete repair discussion for the unsigned `A` component. | A per-ciphertext proof does not automatically remain knowledge-sound after CipherAuth's compressed intake. |
| 3 | Canetti, Hohenberger, *Chosen-Ciphertext Secure Proxy Re-Encryption* (CCS 2007) | Formal CCA-secure PRE with derivative restrictions and key-registration issues. | Main CCA PRE baseline and source of derivative-aware comparison. | `G-CONF-CLASS` is substantially more restrictive than a general CCA PRE notion. |
| 4 | Zhao, Wang, Li, *Composable Aggregation of Schnorr Signatures via Transcript Rebinding* (ICICS 2026) | Rebinds interactive transcripts into aggregation-ready Schnorr relations; cost scales with domains rather than signers. | Very recent aggregation landscape and cross-domain comparator. | Requires certified transcript structure and does not establish PRE component validity. |
| 5 | Nick, Ruffing, Seurin, *DahLIAS* (EUROCRYPT 2026) | First constant-size pairing-free aggregate directly from discrete logarithms; two-round signing; algebraic one-more-DL in ROM. | Recent top-venue alternative to half aggregation. | Interactive and based on a different assumption; not a drop-in replacement for asynchronous origin records. |
| 6 | Zheng, *Digital Signcryption* (CRYPTO 1997) | Introduces signcryption as joint confidentiality/authentication more efficiently than sign-then-encrypt. | Historical origin of the combined functionality. | No delegation or aggregation. |
| 7 | Blaze, Bleumer, Strauss, *Divertible Protocols and Atomic Proxy Cryptography* (EUROCRYPT 1998) | Introduces atomic proxy re-encryption and the semi-trusted proxy model. | Foundational PRE citation. | Early security notions predate modern CCA/HRA requirements. |
| 8 | Bauer, Fuchsbauer, *Efficient Signatures on Randomizable Ciphertexts* (SCN 2020) | Authenticates plaintext-equivalence classes while allowing signatures to adapt to ciphertext rerandomization; strong unforgeability in GGM. | Closest conceptual comparison to transform-aware authentication. | Rerandomization preserves recipient/encryption relation; it is not recipient-changing PRE. Uses bilinear groups. |
| 9 | Schnorr, *Efficient Signature Generation by Smart Cards* (J. Cryptology 1991) | Canonical Schnorr signature relation underlying sender/grant authentication and half aggregation. | Fundamental primitive citation. | Original proof/model is not the exact chosen-key aggregate theorem used here. |
| 10 | Zhou, Han, Liu, Huang, *Fine-Grained Re-encryptions Between Different Encryption Systems* (ASIACRYPT 2025) | Fine-grained re-encryption between heterogeneous systems with controlled functionality. | Context for current advanced PRE functionality. | Different goal and machinery; not needed for the core theorem. |
| 11 | Anthoine, Balbás, Fiore, *Fully-Succinct Multi-key Homomorphic Signatures from Standard Assumptions* (CRYPTO 2024) | Succinct authentication of multi-key homomorphic computations under standard assumptions. | Optional broader context for transformation-compatible authentication. | Much stronger/different functionality and heavy machinery; citing it in the core comparison may distract. |
| 12 | Chen, Zhao, *Half-Aggregation of Schnorr Signatures with Tight Reductions* (ESORICS 2022) | Tight AGM+ROM proof and incremental half-aggregation construction. | Direct authentication compressor and source theorem for future exact-loss transcription. | Does not authenticate CipherAuth's mutable encryption point. Correct second author is **Yunlei Zhao**. |
| 13 | Cohen et al., *HRA-Secure Homomorphic Lattice-Based PRE with Tight Security* (IACR CiC 2025) | Tight concrete HRA security for RLWE homomorphic PRE; demonstrates an attack on a prior claimed HRA scheme. | Shows HRA is an active concrete-security benchmark and motivates conservative claim language. | Lattice/homomorphic construction; not directly comparable in efficiency or assumptions. |
| 14 | Choi, Jeong, Byun, *ID-Based Aggregate Signcryption with Re-encryption for Secure UAV Networks* (Information Sciences 2026) | Pairing-based sensor–UAV–base-station scheme; UAV verifies/decrypts then aggregate re-encrypts; includes a collusion model. | Closest combined application-level functionality. | Proxy is not plaintext-oblivious; security goals and trust boundary differ. Correct authors are **Jae Hyun Choi, Ik Rae Jeong, Jin Wook Byun**. |
| 15 | Green, Ateniese, *Identity-Based Proxy Re-encryption* (ACNS 2007) | Identity-based PRE construction and security treatment. | Same-venue historical PRE context; useful in extended bibliography. | Identity-based key management is not CipherAuth's main novelty axis. |
| 16 | Ateniese, Fu, Green, Hohenberger, *Improved PRE Schemes with Applications to Secure Distributed Storage* (ACM TISSEC 2006) | Unidirectional PRE and practical access-control/storage application. | Foundational unidirectional PRE and deployment motivation. | Predates HRA and does not address aggregate authentication. |
| 17 | Farber Brodsky et al., *Monotone-Policy Aggregate Signatures* (EUROCRYPT 2024) | Aggregate signatures supporting monotone policies over signer/message sets. | Optional recent aggregate-signature landscape. | Policy aggregation is not required by CipherAuth; omit from a space-constrained core paper. Correct first author is **Maya Farber Brodsky**. |
| 18 | Chalkias, Garillot, Kondi, Nikolaenko, *Non-interactive Half-Aggregation of EdDSA and Variants of Schnorr Signatures* (CT-RSA 2021) | Compresses `n` Schnorr responses into one scalar while retaining `n` commitment points; discusses limits of Fiat–Shamir compression. | Direct structural origin of the batch format. | The simple construction's reduction is not the tightest available; Chen–Zhao supplies the preferred proof baseline. |
| 19 | An, Dodis, Rabin, *On the Security of Joint Signature and Encryption* (EUROCRYPT 2002) | Formalizes joint security and shows composition order/commitment details matter. | Central conceptual source for signing a commitment rather than a mutable ciphertext representation. | No PRE and no aggregation. |
| 20 | Hohenberger, Waters, Wu, *Pairing-Based Aggregate Signatures Without Random Oracles* (ASIACRYPT 2025) | Plain-model constant aggregate using pairings and long public parameters/individual signatures. | Recent top-venue comparison on the ROM-versus-setup tradeoff. | Not pairing-free; long setup and pre-aggregation signatures. |
| 21 | Bellare, Rogaway, *Random Oracles Are Practical* (CCS 1993) | Introduces the random-oracle design paradigm. | Fundamental model citation for the candidate reductions. | Does not justify replacing random oracles with SHA-256; the paper correctly disclaims a concrete SHA-256 theorem. |
| 22 | Canetti, Krawczyk, Nielsen, *Relaxing Chosen-Ciphertext Security* (CRYPTO 2003) | Defines replayable CCA and challenge-equal masking ideas. | Supports CipherAuth's conservative decryption-mask rule. | RCCA is not PRE HRA and cannot substitute for a PRE-specific comparison. |
| 23 | Boudgoust, Takahashi, *Sequential Half-Aggregation of Lattice-Based Signatures* (ESORICS 2023) | Sequential half aggregation for lattice signatures. | Optional post-quantum aggregation context. | Different assumptions and signature structure; no effect on the current curve-based artifact. |
| 24 | Fuchsbauer, Kiltz, Loss, *The Algebraic Group Model and Its Applications* (CRYPTO 2018) | Formalizes AGM and algebraic reductions. | Required model background for aggregate-authentication theorem inheritance. | AGM extraction cannot be silently imported into the PRE confidentiality reduction; the current class proof avoids that premise. |
| 25 | Abdalla, Bellare, Rogaway, *Oracle Diffie–Hellman Assumptions and an Analysis of DHIES* (CT-RSA 2001) | Develops oracle-DH assumptions and DHIES analysis. | Background for hashed-ElGamal-style encryption and decision-oracle reductions. | Not the exact gap-DCDH assumption used here. |
| 26 | Cash, Kiltz, Shoup, *The Twin Diffie–Hellman Problem and Applications* (EUROCRYPT 2008) | Gives trapdoor-test/twin-DH techniques that can remove some decision-oracle dependence. | Identifies a plausible future route for replacing the gap oracle. | No such reduction has been established for CipherAuth; it is a research direction, not a claimed theorem. |
| 27 | Ling et al., *Tightly, Adaptively Secure PRE in Multi-challenge Setting* (ASIACRYPT 2025) | First tightly adaptive multi-challenge HRA-secure PRE in the standard model; treats HRA as the minimal acceptable target. | Most important recent PRE benchmark for venue positioning. | Makes clear that CipherAuth's `G-CONF-CLASS` is below the modern HRA bar. |
| 28 | Libert, Vergnaud, *Unidirectional Chosen-Ciphertext Secure PRE* (PKC 2008) | Unidirectional CCA-secure PRE construction and model. | Strong classical PRE baseline. | Pairing-based classical PRE; no aggregate-authentication layer. |
| 29 | Cohen, *What About Bob? The Inadequacy of CPA Security for PRE* (PKC 2019) | Shows CPA can allow a recipient to learn the delegator's secret from one honest reencryption; introduces HRA. | Mandatory modern security-positioning citation. | Directly prevents describing class-level refusal of corrupt recipients as robust PRE security. |

## Manuscript changes justified by the corpus

- Expanded related work into four focused threads: PRE/HRA hierarchy; joint authentication and encryption; aggregate authentication; closest combined functionality.
- Added a comparison table that records transformation type, authentication target, and the exact security boundary.
- Replaced the opaque description of `GDivDH` with “gap-DCDH,” while preserving the artifact identifier.
- Corrected the Chen–Zhao, Choi–Jeong–Byun, Schnorr, and monotone-policy bibliographic records.
- Narrowed the conclusion: the candidate class theorem is strictly weaker than HRA, and no general PRE-security claim is made.
- Kept the positive reductions explicitly provisional and `security_proved: false`.

## Code and result calibration

No algorithm, encoding, test, trace, byte count, or reported experiment needs to change as a result of the literature review. The discoveries concern naming, comparison classes, and the strength of the security claim. The correct action is therefore:

1. preserve the artifact codebase and source manifest byte-for-byte;
2. keep this review and the revised paper outside the frozen artifact tree;
3. rerun `scripts/reproduce_all.py` from the pristine artifact;
4. expect 286 unique tests, 33 deterministic traces, zero skips, and `security_proved: false`;
5. treat any different count or manifest warning as a packaging/integration regression, not as a literature-driven code update.

## Submission recommendation

The defensible ACNS-style contribution is a **negative composition result plus a precisely delimited surviving game**, supported by an executable artifact. It is not a new full-strength PRE scheme. The strongest honest title/abstract language is therefore “security boundary,” “concrete mauling attack,” and “candidate class-granular reduction.” Submission should remain gated on independent review of the class and scope reductions; the literature review itself does not resolve that gate.
