# Phase 1 Results Summary

**Author:** Meryem AIT MOUT  
**Date:** March 2026 
**Experiment:** Conversational AI Memory Benchmark – Phase 1 (seed=42)

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

**Key Insight:** All strategies perform within **0.7%** of each other. Simple latest memory is **680x faster** than similarity search with virtually identical accuracy.

---

## Fact Type Performance

| Fact Type | Accuracy | Sample Size | vs Best |
|-----------|----------|-------------|---------|
| **Pets** | **84.8%** | 1,542 | — |
| Hobbies | 81.0% | 1,512 | -3.8% |
| Cities | 79.5% | 1,494 | -5.3% |
| Colors | 75.4% | 1,452 | **-9.4%** |

**Key Insight:** Content matters more than strategy! There's a **9.4% gap** between pets (most memorable) and colors (least memorable). This suggests that some information is inherently easier for AI to remember.

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

*\*Small sample sizes cause some variation*

**Key Insight:** Memory gradually declines to **50% accuracy** (chance level) for very long conversations (46+ turns). The pattern shows a significant drop around **26 turns** (69.2%) and another at **46+ turns** (50.0%).

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

**Key Insight:** This reveals a **fundamental trade-off**:
- **Recency-based strategies** (latest + hybrids): Great at recent facts (97-99%) but poor at distant facts (60-62%)
- **Similarity-based strategy**: Consistent across time (78-82%) with only a 4% gap

This suggests two different memory mechanisms at work!

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

**Key Insight:** Speed differences are enormous! Latest memory is effectively **instantaneous**, while similarity search is **680x slower** – a critical consideration for real-time applications.

---

## Key Takeaways

| # | Finding | Implication |
|---|---------|-------------|
| 1 | **All strategies within 0.7%** | Strategy choice matters less than expected |
| 2 | **Pets (84.8%) > Colors (75.4%)** | 9.4% gap – content drives memorability |
| 3 | **Latest: 99% recent vs 60% distant** | Strong recency bias |
| 4 | **Similarity: consistent 78-82%** | No recency bias – different mechanism |
| 5 | **Memory drops to 50% at 46+ turns** | Gradual decay to chance level |
| 6 | **Latest is 680x faster** | Huge practical advantage |

---

## Limitations & Future Work

### Current Limitations
- Only **one embedding model** (all-MiniLM-L6-v2)
- Only **4 fact types** – may not generalize
- Only **up to 50 turns** – longer conversations untested
- **Exact match evaluation** – misses partial recall
- **Synthetic conversations** – not real human dialogue

### Planned for Phase 2
- 5+ embedding models
- 10+ fact types
- 100+ turn conversations
- Fuzzy evaluation (semantic similarity)
- LLM-based evaluation
- Real conversation logs

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