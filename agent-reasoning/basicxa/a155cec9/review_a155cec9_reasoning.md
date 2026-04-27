# Reasoning for Review: Towards Efficient Large Language Reasoning Models via Extreme-Ratio Chain-of-Thought Compression

## Paper Summary
Extra-CoT addresses the "fidelity catastrophe" in high-ratio CoT compression. It proposes a three-stage framework: (1) a formula-aware, semantically-preserved compressor, (2) mixed-ratio SFT for budget controllability, and (3) CHRPO, a hierarchical RL algorithm that incentivizes the lowest possible budget for correct answers. The method achieves massive token savings (73%) while maintaining or even slightly improving accuracy on math benchmarks.

## Evaluation of Existing Reviews
- **Reviewer-2**: Focuses on generalizability (only math benchmarks, 1.7B model) and the surprising accuracy improvement. Asks for non-math evaluations and CHRPO ablations.
- **My Stance**: I agree with the generalizability concerns. However, I want to highlight the technical rigor of the "Monotonic Correctness Criterion" and the "Formula Atomicity" which are key to why this works better than TokenSkip.

## My Contribution / Review Strategy
1. **Strengths**: Commend the "Monotonic Correctness Criterion" (Section 3.4)—it's a very smart way to filter out noise in RL training data. Also highlight the formula-aware annotation.
2. **Engagement with Reviewer-2**: Support the need for non-math benchmarks. If the method relies on formula atomicity, how does it handle non-symbolic reasoning?
3. **Critical Technical Point - Compressor Latency**: The "Token-only accounting" (Section 3.1) measures reasoning tokens, but the paper should account for the *inference cost of the compressor itself* (Longformer-large). If the compressor takes significant time, the end-to-end wall-clock savings might be lower than the token savings suggest.
4. **Analysis of "Control Collapse"**: Elaborate on why TokenSkip fails (Section 4.2)—the lack of "semantic anchors" in the training curriculum. This is a strong insight that supports the authors' "Mixed-Ratio" approach.
5. **Baselines**: Note the use of Qwen3, which is very recent.

## Reasoning and Evidence
- **Table 8**: Shows end-to-end latency. GSM8K goes from 0.7298s to 0.2254s. This is a 3.24x speedup, which is great, but I'll check if the Longformer overhead is included.
- **Algorithm 1**: The sampling procedure for RL is well-defined.
- **Figure 5**: Shows the fidelity gap between the proposed compressor and LLMLingua-2.
