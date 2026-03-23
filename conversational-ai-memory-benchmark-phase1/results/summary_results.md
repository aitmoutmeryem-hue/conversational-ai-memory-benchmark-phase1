# Phase 1 Results Summary

**Author:** Meryem AIT MOUT  
**Date:** March 2026  
**Experiment:** Conversational AI Memory Benchmark – Phase 1 (seed=42)

---

## Abstract

We evaluate six memory strategies for conversational AI across 6,000 simulated dialogues.  
All strategies achieve similar accuracy (~80%), despite large differences in computational cost.  
Simple recency-based memory performs comparably to embedding-based retrieval while being up to **680× faster**.  

Additionally, memory performance depends more on content type than retrieval strategy, and gradually degrades to chance level in long conversations.  
These results suggest that conversational memory remains a challenging and under-optimized component of AI systems.

---

## Overall Statistics

| Metric | Value |
|--------|-------|
| **Total samples** | 6,000 fact-retrieval attempts |
| **Random seed** | 42 (fixed for reproducibility) |
| **Conversation lengths** | 10, 20, 30, 40, 50 turns |
| **Runs per length** | 200 |
| **Fact types** | pets, hobbies, cities, colors |
| **Memory strategies** | 6 (latest, similarity, 4 hybrids) |

---

## Strategy Performance

| Strategy | Accuracy | Latency (ms) | vs Fastest |
|----------|----------|--------------|------------|
| `hybrid_r0.3_s0.7` | **80.7%** | 0.59 | 590x slower |
| `hybrid_r0.5_s0.5` | 80.2% | 0.59 | 590x slower |
| `hybrid_r0.7_s0.3` | 80.2% | **0.55** | 550x slower |
| `hybrid_r0.9_s0.1` | 80.2% | 0.60 | 600x slower |
| `latest` | 80.2% | **0.00** | **1x (fastest)** |
| `similarity` | 80.0% | 0.68 | 680x slower |

**Key Insight:** All strategies perform within **0.7%** of each other.  
Simple `latest` memory is **~680× faster** than similarity search with virtually identical accuracy.

---

## Fact Type Performance

| Fact Type | Accuracy | Sample Size | vs Best |
|-----------|----------|-------------|---------|
| **Pets** | **84.8%** | 1,542 | — |
| Hobbies | 81.0% | 1,512 | -3.8% |
| Cities | 79.5% | 1,494 | -5.3% |
| Colors | 75.4% | 1,452 | **-9.4%** |

**Key Insight:** Content matters more than strategy.  
There is a **9.4% gap** between pets (most memorable) and colors (least memorable).

This suggests that **concrete or salient entities** (e.g., animals) may create stronger memory traces than more abstract attributes (e.g., colors).

---

## Memory Decay Over Distance

| Distance (turns) | Accuracy | Sample Size | Change |
|-----------------|----------|-------------|--------|
| 1 | 95.6% | 252 | — |
| 6 | 87.0% | 300 | -8.6% |
| 11 | 93.8% | 96 | +6.8%* |
| 16 | 82.6% | 138 | -11.2% |
| 21 | 83.3% | 48 | +0.7% |
| 26 | **69.2%** | 78 | **-14.1%** |
| 31 | 81.7% | 60 | +12.5%* |
| 36 | 73.1% | 78 | -8.6% |
| 41 | 66.7% | 18 | -6.4% |
| 46+ | **50.0%** | 24 | **-16.7%** |

*\*Small sample sizes cause variation*

**Key Insight:** Memory gradually declines to **~50% accuracy (chance level)** for long conversations (46+ turns).

The non-monotonic variations are due to limited sample sizes at certain distances.  
However, the overall trend clearly follows a **downward decay curve**.

This suggests that conversational memory behaves like a **noisy decay process** approaching random retrieval over time.

---

## First vs Latest Facts

| Strategy | First Facts | Latest Facts | Gap |
|----------|-------------|--------------|-----|
| `latest` | 60% | 99% | **39%** |
| `similarity` | 78% | 82% | **4%** |
| `hybrid_r0.9_s0.1` | 60% | 98% | 38% |
| `hybrid_r0.7_s0.3` | 60% | 98% | 38% |
| `hybrid_r0.5_s0.5` | 60% | 98% | 38% |
| `hybrid_r0.3_s0.7` | 62% | 97% | 35% |

**Key Insight:** There is a fundamental trade-off between **recency and stability**:

- Recency-based strategies (latest + hybrids):  
  → Excellent short-term recall (97–99%)  
  → Poor long-term recall (60–62%)

- Similarity-based strategy:  
  → Stable performance across time (78–82%)  
  → Minimal recency bias (only 4% gap)

This suggests the existence of **two distinct memory mechanisms** in conversational systems.

---

## Latency Comparison

| Strategy | Avg Latency (ms) | Relative Speed |
|----------|------------------|----------------|
| `latest` | **0.00** | **1x (fastest)** |
| `hybrid_r0.7_s0.3` | 0.55 | 550x slower |
| `hybrid_r0.5_s0.5` | 0.59 | 590x slower |
| `hybrid_r0.3_s0.7` | 0.59 | 590x slower |
| `hybrid_r0.9_s0.1` | 0.60 | 600x slower |
| `similarity` | 0.68 | **680x slower** |

**Key Insight:** Speed differences are extreme.  
`latest` is effectively instantaneous, while similarity-based retrieval is **orders of magnitude slower**.

---

## Interpretation

These results suggest that:

- Memory architecture may be **less important than expected**
- Content characteristics strongly influence recall
- Recency and similarity represent **different memory paradigms**
- Long-term conversational memory remains an **open challenge**

---

## Critical Limitations & Interpretation

While the results show minimal differences between memory strategies, several aspects of the experimental design may influence this outcome.

- **Synthetic conversations:**  
  The use of templated and low-variability dialogue likely simplifies the task and may favor recency-based strategies. Real conversations include paraphrasing, ambiguity, and noise, which could increase the importance of semantic retrieval.

- **Exact match evaluation:**  
  The strict string-matching evaluation may underestimate the performance of similarity-based approaches. Semantically correct but differently phrased answers are counted as incorrect.

- **Lack of conflicting or updated facts:**  
  The benchmark does not include scenarios where information changes over time (e.g., “I moved from Paris to Berlin”). This removes a key challenge for conversational memory systems and may reduce differences between strategies.

- **Simplified query structure:**  
  Probe questions are direct and uniform (e.g., “What is my latest pet?”), which reduces linguistic complexity and makes retrieval easier for all methods.

- **External validity:**  
  Due to these constraints, results may not fully generalize to real-world conversational AI systems.

> These limitations suggest that the current results may represent a **lower-bound estimate of the importance of semantic memory**, and more realistic settings could reveal larger performance differences.

---

## Key Takeaways

| # | Finding | Implication |
|---|---------|-------------|
| 1 | **All strategies within 0.7%** | Strategy choice matters less than expected |
| 2 | **Pets (84.8%) > Colors (75.4%)** | Content drives memorability |
| 3 | **Latest: 99% vs 60%** | Strong recency bias |
| 4 | **Similarity: stable 78–82%** | Temporal robustness |
| 5 | **Memory → 50% at long range** | Decay to chance level |
| 6 | **Latest is 680× faster** | Massive practical advantage |

---

## Limitations & Future Work

### Current Limitations
- Only **one embedding model** (all-MiniLM-L6-v2)
- Only **4 fact types**
- Limited to **50 turns**
- **Exact match evaluation**
- **Synthetic conversations**

### Planned for Phase 2
- Multiple embedding models
- More fact types
- 100+ turn conversations
- Semantic evaluation
- Real-world data

---

## How to Cite

```bibtex
@software{aitmout2026memory,
  author = {AIT MOUT, Meryem},
  title = {Conversational AI Memory Benchmark – Phase 1 Results},
  year = {2026},
  publisher = {GitHub},
  url = {https://github.com/aitmoutmeryem-hue/conversational-ai-memory-benchmark-phase1}
}
