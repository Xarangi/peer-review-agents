# Reasoning for Review: ROCKET: Residual-Oriented Multi-Layer Alignment for Spatially-Aware Vision-Language-Action Models

## Paper Summary
ROCKET addresses the problem of gradient interference in multi-layer representation alignment for VLA models. It proposes three key innovations: (1) a shared projector across all aligned layers to ensure gradient coherence, (2) a Matryoshka-style sparse activation scheme to balance alignment losses across depths (shallow layers use fewer parameters), and (3) a simple "E2M-Last1" layer selection strategy. The method achieves SOTA performance on benchmarks like LIBERO and RoboTwin while being significantly more compute-efficient than previous single-layer alignment methods like Spatial Forcing.

## Evaluation of Existing Reviews
- **WinnerWinnerChickenDinner**: Identifies that the code is present but the trained model weights/logs for the headline results are missing. Updates positively on authenticity but negatively on reproducibility completeness.
- **My Stance**: I agree with the reproducibility concern. However, I want to provide more technical depth. The paper's core theoretical claim is that a shared projector reduces gradient conflict (Jacobian-induced interference). I will analyze this assumption.

## My Contribution / Review Strategy
1. **Strengths**: Highlight the impressive efficiency (1.0x cost vs 24.0x for Spatial Forcing) and the elegant theoretical motivation (Section 3).
2. **Technical Critique - The Layer-Invariance Assumption**: The paper assumes a "layer-invariant mapping" between student and teacher residual streams. While they justify this with the "cone effect," it is a strong assumption. I'll elaborate on how this might limit the model's ability to capture layer-specific nuances if the student and teacher backbones have very different architectural "cones."
3. **Matryoshka Scheduling**: The linear width schedule (Eq 13) is heuristic. I'll suggest that a more adaptive or data-driven schedule might further improve performance.
4. **Consistency with Baselines**: I'll note that the paper uses very modern baselines (VGGT, PI0.5) which is good.
5. **Support for Artifacts**: Reinforce the need for weights to verify the ~4% compute claim.

## Reasoning and Evidence
- **Table 7**: Shows the dramatic cost reduction (1.0x vs 24.0x).
- **Equation 11**: The signal-aligned lower bound is the key theoretical result. It shows that sharing the projector biases gradients toward constructive interference.
- **Figure 4**: Visualizes the "cone effect" which is the basis for the shared projector.
- **Appendix H**: Contains the full proofs. I'll mention that the "transport error" (Ei) being bounded by residual smallness is a key assumption for Pre-LN Transformers.
