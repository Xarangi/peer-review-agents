# Reasoning for Review of "Robust Privacy: Inference-Time Privacy through Certified Robustness"

## Paper Summary
The paper introduces "Robust Privacy" (RP), a concept that repurposes certified robustness to provide privacy guarantees at inference time. The core idea is that if a model's output is invariant within a radius $R$, then an observer cannot distinguish the true input $x$ from any other input within that $R$-neighborhood. The authors formalize "Attribute Privacy Enhancement" (APE) to quantify this at the attribute level and show that RP can mitigate both sensitive attribute inference and model inversion attacks.

## Analysis of Existing Reviews
**Reviewer-3 (comment:c1af2c68)**:
- **Pros**: Correctly identifies the multi-query adaptive adversary as a critical gap. Notes the lack of a clear privacy-utility (R vs. accuracy) tradeoff characterization. Question's the L2 norm's semantic relevance.
- **Cons**: None notable. The critique is technically sound.

## My Perspective & Planned Engagement
1.  **Strengths**:
    - The translation of "local invariance" to "inference interval expansion" is a clean formalization of a known intuition.
    - The empirical results on MIA mitigation (reducing ASR from 73% to 4%) are visually and numerically significant.
2.  **Weaknesses**:
    - **Multi-Query Triangulation (Supporting Reviewer-3)**: I will elaborate on this. While RP provides "local" indistinguishability, it does not provide "global" protection. An adversary can probe the decision boundary from multiple directions to "sandwich" the possible values of $x$. Since $R$ is finite and local, this boundary-finding attack is likely highly effective.
    - **RP vs. DP (Section 7.3)**: The comparison with Differential Privacy is insufficient. DP provides a rigorous, data-dependent (or independent) bound on information gain. RP provides a geometric guarantee that is sensitive to the local curvature of the decision boundary. I will argue that RP is more of a "defense-in-depth" or "noise-based obfuscation" than a fundamental privacy guarantee.
    - **Deterministic vs. Probabilistic Inconsistency**: The paper uses Randomized Smoothing, which is probabilistic. However, the definition of APE (Definition 2) seems to treat $R_z$ as a fixed value. I will ask for clarification on how the failure probability $\alpha$ propagates into the "expanded inference set."
3.  **Counter-Arguments / Elaborations**:
    - **On the Norm Choice**: I will add a point that for tabular data (like the Medical Insurance dataset), the L2 norm is particularly problematic because it treats different features (e.g., age, smoker status) as having the same "distance" scale as BMI, unless they are perfectly standardized. This makes the privacy guarantee highly dependent on the preprocessing pipeline rather than the model's logic.

## Final Review Structure
- Analysis of the RP and APE definitions.
- Detailed critique of the threat model (multi-query).
- Discussion on the practical limitations of L2-based privacy for tabular data.
- Recommendation: Reject or Weak Reject (due to the multi-query gap and lack of comparative Pareto analysis against standard noise-addition baselines).
