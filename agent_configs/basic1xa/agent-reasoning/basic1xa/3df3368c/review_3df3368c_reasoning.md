# Review Reasoning: Online Vector Quantized Attention

The paper "Online Vector Quantized Attention" introduces a sequence mixing layer that dynamically learns key and value dictionaries during the forward pass using online clustering.

## 1. Quality & Soundness
The paper is technically very strong. The connection between VQ-attention and Gaussian Mixture Regression (GMR) provides a rigorous theoretical foundation that was previously lacking. The derivation showing that a single EM update is equivalent to a second-order Newton step on the GMR log-likelihood is particularly insightful. The assumptions (unit norm vectors, spherical covariance) are well-justified and directly implemented in the architecture.

## 2. Originality & Novelty
The shift from static to online dictionary learning is a significant advancement over Lingle (2023). Most efficient attention mechanisms (SSMs, Linear Attention) rely on dense state updates. The sparse update mechanism of OVQ-attention, grounded in associative memory principles, represents a distinct and promising architectural direction.

## 3. Significance & Impact
The empirical results on synthetic long-context tasks (ICR, ICL) are compelling, showing that OVQ-attention can match self-attention's precision while maintaining linear complexity. This has high potential for scaling LLM context windows.

## 4. Weaknesses to Highlight
As noted by other reviewers, the lack of wall-clock profiling is a major gap. The reliance on `gather` and `scatter_add` operations is a practical concern for GPU throughput. I will also point out that while the small-scale results (70M-480M) are promising, validation at the 7B+ scale is necessary to confirm the method's effectiveness in real-world models.

## 5. Engagement with Previous Discussion
I will support the call for wall-clock benchmarks and hardware-aware implementations. I will also elaborate on the "forgetting" mechanism: dense updates in SSMs distribute new information across the entire state, which can overwrite previous memories (interference), whereas OVQ's sparse updates provide a form of structural protection for stored keys.
