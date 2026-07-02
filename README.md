# AntiDistillation Sampling using Watermarking Techniques

A worked-through synthesis of **Antidistillation Sampling** and **LLM Watermarking** that explores how watermarking techniques can be repurposed to create language model outputs that are simultaneously **resistant to model distillation** and **cryptographically auditable**.

---

## Overview

Large Language Models (LLMs) often expose reasoning traces that improve transparency but also make them vulnerable to **knowledge distillation**, allowing attackers to train cheaper student models using API outputs.

This project studies a unified framework that combines ideas from:

- **Antidistillation Sampling** (Savani et al., 2025)
- **A Watermark for Large Language Models** (Kirchenbauer et al., ICML 2023)

Instead of using watermarking purely for provenance detection, the proposed method derives the watermark's green-list directly from the antidistillation poison score, creating a single sampling strategy that aims to:

- Reduce the effectiveness of downstream distillation
- Preserve teacher utility
- Enable statistical verification using a watermark-style detector

---

## Key Ideas

### Antidistillation Sampling

The teacher model biases generation toward tokens that negatively influence a proxy student during future training while remaining likely under the teacher's original distribution.

### LLM Watermarking

The watermarking method partitions the vocabulary into green and red lists and biases sampling toward green tokens. Generated text can later be detected using a statistical z-test.

### Unified Approach

Rather than combining the two methods independently, this project constructs the green list from the antidistillation poison ranking.

This removes competing objectives and produces a sampler that is simultaneously:

- resistant to distillation
- statistically detectable
- computationally efficient

---

## Report Contents

The report covers:

- Background on LLM watermarking
- Background on antidistillation sampling
- Mathematical derivation of the poison score
- Finite-difference approximation
- Unified sampling algorithm
- Detector construction
- Calibration of false-positive rates
- Distillation evaluation protocol
- Discussion of utility–detectability trade-offs

---

## Results

The report discusses two complementary evaluations:

- **Utility vs Detection Trade-off**
  - Increasing watermark strength improves detectability but introduces higher utility cost.

- **Distillability Trade-off**
  - Antidistillation sampling can significantly reduce student performance while maintaining relatively high teacher accuracy.

Together, these illustrate the balance between watermark detectability and resistance to model extraction.

---

## References

1. **Savani et al.**
   *Antidistillation Sampling*
   arXiv:2504.13146 (2025)

2. **Kirchenbauer et al.**
   *A Watermark for Large Language Models*
   ICML 2023

---

## Repository Structure

```
.
├── antidistillation.pdf      # Full technical report
├── README.md
└── figures/                  # Figures used in the report (optional)
```

---

## Future Work

Potential extensions include:

- Scaling experiments to larger teacher and student models
- Evaluating on GSM8K, MATH, and MMLU benchmarks
- Testing cross-tokenizer compatibility
- Improving calibration using larger null datasets
- Exploring stronger poisoning objectives beyond finite-difference approximations

---



---
