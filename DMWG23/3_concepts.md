# Core Concepts: Fiat-Shamir, Adaptivity, and Soundness

---

## What is the Fiat-Shamir Transform?

- A method to convert an **interactive proof** into a **non-interactive proof** using a hash function.
- Typically used in cryptographic protocols to eliminate the need for an interactive verifier.

### Mechanism:
1. Prover computes a commitment: `a`
2. Challenge is derived as: `e ← H(a)` where `H` is a hash function
3. Prover computes response: `z`
4. Verifier checks `(a, e, z)` using public input

### Security:
- Security (soundness & zero-knowledge) is provably preserved in the **Random Oracle Model** (ROM).
- See: Bellare & Rogaway, 1993

---

## Weak vs. Strong Fiat-Shamir

| Variant | Challenge Computation | Risk |
|--------|------------------------|------|
| **Weak FS** | `e = H(a)` | Prover can fix `a`, then later choose public input to match proof |
| **Strong FS** | `e = H(a, public_input)` | Public input is bound to the proof, prevents forgery |

- Weak FS allows **adaptive attacks**, where public inputs are chosen *after* the proof is created.

---

## What is an Adaptive Attack?

- A malicious prover chooses the public input **after** generating (or partially generating) the proof.
- Breaks the assumption that the statement is fixed *before* the proof begins.
- Violates **knowledge soundness**: a valid proof might be accepted even though it corresponds to a *false* statement.

---

## What is Soundness?

- Soundness ensures that **only valid statements** can be proven.
- In a sound system:
  > If a prover convinces the verifier, then the statement must be true.
- Adaptive soundness: even if the prover picks inputs adaptively, they still cannot cheat.

---

## Example: FS on Schnorr

- Classical Schnorr protocol: prove knowledge of `x` such that `X = g^x`.
- FS transforms it to non-interactive by computing `c = H(A)`.
- **If public input `X` is not included in `H`**, attacker can choose `X` after fixing `A` — breaks soundness.

---

## Why This Matters

- Many real-world ZK systems rely on FS transform.
- Weak implementations can result in **maliciously forged proofs** that appear valid.
- Correct handling of **transcript initialization** and **hash input design** is crucial.
