# Other Targeted Proof Systems under Weak Fiat-Shamir

This section summarizes the remaining three proof systems analyzed in the paper and how weak Fiat-Shamir compromises their soundness.

---

## Plonk

- **System Type**: Polynomial IOP-based zero-knowledge proof
- **Attack Strategy**: 
    - Prover computes proof before fixing the public inputs or instance polynomials.
    - Challenge values are computed via weak FS (not binding to instance).
    - Prover adaptively selects public input that satisfies polynomial identity post hoc.
- **Impact**: Violates soundness — prover can forge valid-looking proofs for false statements.
- **Comment**: Proofs in practice often use strong FS or transcripts (e.g., via Merlin), mitigating this.

---

## Spartan

- **System Type**: Succinct polynomial delegation with interactive oracle proofs
- **Attack Strategy**:
    - Similar to Plonk: prover commits to proof early, then chooses instance adaptively.
    - Since challenge is decoupled from the input, constraints can be bypassed.
- **Impact**: Soundness breaks in non-adaptive proofs if FS is weak.
- **Comment**: Highlights the need for correct transcript initialization and domain separation.

---

## Wesolowski’s VDF

- **System Type**: Verifiable Delay Function (time-based cryptography)
- **Attack Strategy**:
    - Prover creates proof for a short delay `t ≪ T`
    - Then sets public input `T` (claimed delay) **after** proof generation
- **Impact**: Breaks time delay assumption (i.e., soundness of the VDF)
- **Comment**: Important in systems that rely on delay guarantees (e.g., randomness beacons)

---

## Summary Table

| System | FS Vulnerability | Attack Mechanism | Mitigation |
|-------------|--------------------------|-----------------------------------------|----------------------|
| Plonk | Challenge ignores input | Adaptive polynomial identity | Strong FS / Merlin |
| Spartan | Weak instance binding | Post hoc constraint satisfaction | Strong FS |
| VDF (Weso.) | Delay chosen adaptively | Short-time computation with false claim | Bind FS to delay |