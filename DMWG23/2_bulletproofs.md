# Attack on Bulletproofs via Weak Fiat-Shamir

**Section**: 4 of the paper  
**Target System**: Bulletproofs – Aggregate Range Proof (BP-ARP)  
**Goal**: Break soundness by exploiting weak Fiat-Shamir (FS), where the challenge does **not** depend on public input.

---

## High-Level Idea

If the FS challenge is computed *before* fixing the public inputs (i.e., commitments), the prover can craft a valid-looking proof and then forge commitments afterwards to match it — violating soundness.

---

## Attack Overview

1. **Prover Setup**:
   - Samples random vectors $( \mathbf{a}_L, \mathbf{s}_L, \mathbf{s}_R \in \mathbb{F}^{mn} )$
   - Samples blinding scalars $( \alpha, \rho \in \mathbb{F} )$
   - Computes commitments $( A, S )$

2. **FS Challenge Sampling** (Weak FS):
   - Prover sets $( x, y, z )$ without including public inputs (e.g., $( \mathbf{V} )$)

3. **Polynomial Construction**:
   - Constructs $( \mathbf{l}(x), \mathbf{r}(x) )$ vectors
   - Computes inner product $( \hat{t} = \langle \mathbf{l}, \mathbf{r} \rangle )$
   - Computes BP-IPA proof $( \pi_{\text{IPA}} )$ for the inner product

4. **Forging Public Inputs**:
   - Finds $( v_i, \gamma_i )$ to satisfy:
     $$
     \sum_{i=1}^m v_i z^{i+1} = \hat{t} - t_1 x - t_2 x^2 - \delta(y, z) \\\\
     \sum_{i=1}^m \gamma_i z^{i+1} = \tau_x - \tau_2 x^2 - \tau_1 x
     $$
   - Computes forged commitments $( V_i = g^{v_i} h^{\gamma_i} )$

5. **Submission**:
   - Sends forged $( \mathbf{V} )$ and $( \pi )$ to verifier

---

## Why This Works

- FS challenge $( x )$ is not bound to $( \mathbf{V} )$, so the prover has full freedom to manipulate $( v_i )$
- Verifier checks consistency with the challenge and proof only — not the input structure
- All equations match ⇒ verifier outputs OK

---

## Key Takeaways

| Element | Controlled by Prover? |
|---------|-----------------------|
| `g`, `h`, `u'`, `h'`, `x`, `z`, `y` | No |
| `a_L`, `s_L`, `s_R`, `t`, `r`, `l(x), r(x)` | Yes |
| `P'`, `A`, `S`, `T_1`, `T_2` | Yes |
| `v_i`, `γ_i` | Yes (solved from equations) |

---

## Complexity of Real-World Attack

- When **each output coin** has a **separate range proof**:
  - Prover has many degrees of freedom
  - Attack reduces to a **generalized birthday problem (k-sum)**
  - With 30 outputs and 2⁹ candidates each, brute-force attack possible in ~2⁴³ time

- When using **aggregate range proofs** (as in Monero):
  - All values must be simultaneously consistent
  - Attack becomes **computationally infeasible**

---

## Conclusion

This attack demonstrates that **weak FS allows the prover to choose public inputs adaptively**, violating the soundness of Bulletproofs. Though Monero and other systems use strong FS, the vulnerability remains theoretically significant and applicable to other systems if FS is not handled correctly.