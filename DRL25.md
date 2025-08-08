# How to Prove False Statements: Practical Attacks on Fiat-Shamir

**Authors**: Dmitry Khovratovich, Ron D. Rothblum, Lev Soukhanov
**Link**: https://eprint.iacr.org/2025/118  
**Year**: Jan 30, 2025

---

## Purpose

This paper shows that even *strong* Fiat-Shamir (FS) transformations—where the hash challenge includes public inputs—can be insecure when combined with certain proof systems like GKR protocols and polynomial commitment schemes. The authors construct concrete attacks that break adaptive soundness without relying on hash function weaknesses.

## Systems Targeted

- $\text{FS}_h(\Pi_\text{comm,d})$ (a GKR-based protocol with MLPCS commitments)

---

## Key Contributions

- Prove that for *any* hash function `h`, one can construct a (false) proof accepted under FS_h.
- Introduce a minimal proof system that is provably insecure under Fiat-Shamir.
- Demonstrate that attacks apply even when public inputs and circuit descriptions are included in the FS challenge.
- Provide constructions (e.g., `C*`) that allow the prover to bias the verifier’s challenge without breaking the hash.
- Show that the attack generalizes to any circuit `C` by constructing a `C*` with identical functionality but tailored soundness failure.

---

## Why These Attacks Matter

- The attack defeats **adaptive soundness**, meaning the prover can choose public inputs *after* crafting the proof.
- It highlights that **implementation-level details** (e.g., the structure of the circuit) can undermine cryptographic proofs.
- It demonstrates that FS soundness does **not necessarily follow** from simulation soundness or non-adaptive guarantees.

---

## Insight

The work goes beyond classical weak FS attacks by showing that even **semantically equivalent circuits** can be manipulated to forge proofs. It also resolves technical challenges like circular dependencies by embedding the circuit digest directly as witness input.

---

## Mitigations Suggested

- Rethink the FS transformation when used with recursive or GKR-based proofs.
- Carefully audit the *input binding* and *circuit structure* in soundness-critical applications.
- Use interactive proofs or alternative NIZK constructions that don’t rely on FS transformations.