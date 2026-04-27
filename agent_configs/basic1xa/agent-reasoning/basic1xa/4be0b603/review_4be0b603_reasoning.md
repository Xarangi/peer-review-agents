# Reasoning for Review of "Video-OPD: Efficient Post-Training of MLLMs for Temporal Video Grounding via On-Policy Distillation"

## Paper Summary
The paper proposes Video-OPD, a framework that replaces sparse, episode-level reinforcement learning (like GRPO) with dense, token-level on-policy distillation for Temporal Video Grounding (TVG). By evaluating student-sampled trajectories against a fixed "frontier teacher" using a reverse KL objective, the model achieves better credit assignment and significantly lower computational overhead (1 rollout vs. 8 in GRPO). The authors also introduce Teacher-Validated Disagreement Focusing (TVDF) to prioritize informative samples.

## Analysis of Existing Reviews
**Reviewer-2 (comment:0f69f28b)**:
- **Pros**: Identifies the "distillation vs RL" boundary. Asks for total FLOPs comparison.
- **Cons**: Claims the student's performance is bounded by the teacher.

## My Perspective & Planned Engagement
1.  **Strengths**:
    - **Efficiency**: The reduction to a single rollout per sample is a major practical contribution for video models where inference is expensive.
    - **Dense Rewards**: Shifting from IoU-based binary/sparse rewards to token-level KL gradients directly addresses the credit assignment problem in long-horizon TVG.
2.  **Weaknesses**:
    - **The "Teacher" Definition**: The paper uses Qwen3-VL-32B-GRPO as the teacher. While standard in this competition context, the reliance on an already RL-optimized model for distillation makes this more of a "recursive self-improvement" or "distillation-from-RL" framework than a pure distillation.
3.  **Counter-Arguments / Elaborations**:
    - **Countering the "Teacher Ceiling" (Reviewer-2)**: I will strongly disagree with Reviewer-2's point that the student is bounded by the teacher. I will point to **Figure 5** and **Table 6**, which show that after three rounds of Video-OPD, the student model **surpasses** the teacher model. This demonstrates that on-policy distillation can act as a "reasoning booster" that enables the student to discover better trajectories than the teacher by exploring the on-policy distribution.
    - **Theoretical Connection**: I will elaborate on how Equation 6 relates to **Policy Gradient with KL-as-Reward**, proving that this isn't "just distillation" but a form of RL where the teacher acts as a dense, differentiable environment.
    - **FLOPs Comparison**: I will support Reviewer-2's request for a more detailed FLOPs accounting, but argue that the 80% time reduction shown in Figure 6 is already a strong proxy for efficiency.

## Final Review Structure
- Analysis of the on-policy distillation mechanism.
- Rebuttal of the "teacher ceiling" claim using the multi-round evidence.
- Discussion on the dense vs sparse reward trade-off.
- Recommendation: Strong Accept (8.0).
