# Weak Fiat-Shamir Attacks on Modern Proof Systems

**Authors**: Quang Dao, Paul Grubbs, et al.  
**Link**: https://eprint.iacr.org/2023/691  
**Year**: 2023

---

## Purpose

This paper investigates the consequences of using *weak* Fiat-Shamir (FS) transformations—where the hash challenge is generated without binding public inputs. The authors demonstrate that modern non-interactive proof systems become vulnerable to adaptive attacks under such settings.

## Systems Analyzed

- **Bulletproofs**
- **Plonk**
- **Spartan**
- **Wesolowski’s VDF**

---

## Key Contributions

- Construct concrete adaptive attacks against four modern proof systems using weak FS.
- Show that these attacks break *knowledge soundness* in a non-black-box way.
- Survey real-world repositories (e.g., Monero, MimbleWimble) and confirm they use strong FS, hence not vulnerable.
- Provide a case study showing how money could be forged in protocols like MimbleWimble if weak FS were used.
- Propose mitigations: Strong FS, improved transcript initialization (e.g., via Merlin).

---

## Why These Attacks Matter

- All attacks rely on the **adaptive choice of public inputs**, which becomes possible when using a **weak FS transform**.
- The authors not only demonstrate these attacks empirically, but also **rigorously prove** that each breaks a **well-defined soundness property**.
- Especially for Plonk and Spartan:
    - Soundness fails even if the constraint system satisfies only slightly more than worst-case hardness
    - This suggests **theoretical limitations** under weak FS

---

## Insight

While previous FS attacks targeted classical sigma protocols, this paper generalizes the issue to modern proof systems with richer constraint models, such as polynomial IOPs and inner product arguments.

---

## Mitigations Proposed

- **Strong FS**: Include public inputs in the FS hash challenge.
- **Transcript Guarding**: Enforce correct transcript initialization with tools like Merlin.
- **Information Flow Checking**: Use static analysis to ensure shared values are bound in both prover and verifier views.
