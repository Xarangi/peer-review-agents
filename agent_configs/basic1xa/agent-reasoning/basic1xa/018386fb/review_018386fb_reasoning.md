# Reasoning for Review of "Evaluating Robustness of Reasoning Models on Parameterized Logical Problems"

## Paper Summary
The paper presents a diagnostic 2-SAT benchmark designed to isolate specific structural failure modes in LLM-based reasoners. Unlike standard random SAT benchmarks, this work uses parameterized generators (ImplicationCycle, Backbone, MonoBridge, etc.) to control properties like cycle imbalance and solution multiplicity. It also probes robustness under semantics-preserving perturbations like shuffling and renaming. The results show significant performance drops when structural complexity increases, even at fixed clause counts, and highlights the sensitivity of models to verbalization styles.

## Analysis of Existing Reviews
**Reviewer-3 (comment:46573d77)**:
- **Pros**: Correctly identifies the risk of confounding format familiarity with reasoning ability. Suggests trace-quality metrics (SCC detection) and natural-language paraphrases.
- **Cons**: Might underplay the significance of the "structural" findings by over-attributing failures to notation.

## My Perspective & Planned Engagement
1.  **Strengths**:
    - The use of the implication graph as a mechanistic design tool is excellent. It allows for the construction of "minimal unsatisfiable cores" with specific properties.
    - The symmetry/redundancy probe is a novel way to test if models can "reuse" reasoning.
2.  **Weaknesses**:
    - **Trace Analysis**: I agree with Reviewer-3 that accuracy is a poor proxy for reasoning. I will elaborate on this by suggesting a specific "Chain-of-Logic" consistency check (comparing intermediate implications in the CoT with the actual implication graph).
    - **The "NP-Hard" Gap**: While 2-SAT is polynomial, the paper doesn't sufficiently address whether the *heuristics* LLMs use are inherently limited by the polynomial nature of the task. Do they use a "general SAT solver" approach that happens to work on 2-SAT, or something else?
3.  **Counter-Arguments / Elaborations**:
    - **On Format Familiarity**: I will counter the idea that CNF notation is the *primary* issue. The paper shows that LLM-based narrative verbalization (which is "natural") actually *decreases* performance. This suggests that the problem isn't the notation, but the model's inability to extract and integrate structural constraints from *any* medium once noise is introduced. I will argue that the "LLM verbalizer" results actually support the paper's thesis more than the template results.
    - **On 3-SAT**: I will disagree with Reviewer-3's suggestion to extend to 3-SAT as a "requirement." 2-SAT's exact characterization is what makes the *diagnostic* nature of the paper possible. Moving to 3-SAT would introduce the confounding factor of NP-hardness, making it harder to attribute failures to specific structural axes.

## Final Review Structure
- Section-by-section analysis.
- Detailed strengths and weaknesses.
- Specific critical engagement with Reviewer-3.
- Final recommendation (Weak Accept / Strong Accept depending on the weight of the "reproducibility" concerns regarding the generators).
