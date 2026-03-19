# 🧠 Conversational AI Memory Benchmark – Phase 1

**Author:** Meryem AIT MOUT  
**Year:** 2026  

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)  
[![Python 3.9+](https://img.shields.io/badge/python-3.9+-blue.svg)](https://www.python.org/downloads/)

---

## Key Insight

Simple memory strategies perform as well as complex semantic retrieval while being up to **680× faster**.  

This benchmark shows that conversational AI memory is more constrained by **time and content** than by retrieval strategy.

---

## Why It Matters

The results demonstrate that simpler memory strategies can achieve almost the same accuracy as complex ones, but with enormous speed advantages.

-  **Chatbots:** Faster responses without heavy computation, improving user experience.  
-  **Customer Support AI:** Lower infrastructure cost; simpler memory strategies require fewer resources.  
-  **AI Assistants:** Efficiently balance speed and recall, even in long conversations.  

> Complex embedding-based memory may often be unnecessary overhead, as simpler strategies can achieve similar performance while saving time, computation, and resources.
---

## Overview

This repository contains the complete code, data, and analysis for **Phase 1** of a research project on conversational AI memory.  

We evaluate how different memory strategies perform across **6,000 simulated conversations**, focusing on:

- Memory accuracy  
- Latency (speed)  
- Memory decay over time  
- Impact of content type  

### Key Research Questions

1. **Memory Strategy Comparison:** How do recency-based, similarity-based, and hybrid strategies compare?  
2. **Fact Type Effects:** Does content modulate memorability?  
3. **Memory Decay:** How does accuracy degrade with conversation length?  

---

## Memory Strategies

- **latest** → returns the most recent fact (recency-based, fastest)  
- **similarity** → retrieves the most semantically similar fact using embeddings  
- **hybrid strategies** → combine recency and similarity with different weights

This allows comparison between **time-based memory**, **semantic memory**, and **combined approaches**.

---

## Major Findings

| Finding | Result |
|---------|--------|
| **Best Strategy** | hybrid_r0.3_s0.7 (80.7%) |
| **Strategy Range** | 80.0% - 80.7% (all within 0.7%) |
| **Fact Type Ranking** | Pets (84.8%) > Hobbies (81.0%) > Cities (79.5%) > Colors (75.4%) |
| **Memory Decay** | 95.6% at 1 turn → 50.0% at 46+ turns |
| **Speed Difference** | latest is 680× faster than similarity |

---

## Visual Results

| Overall Accuracy | First vs Latest |
|-----------------|----------------|
| ![Overall](results/figures/overall_accuracy.png) | ![First vs Latest](results/figures/first_vs_latest.png) |

| Memory Decay | Fact Type Heatmap |
|-------------|-------------------|
| ![Decay](results/figures/memory_decay.png) | ![Heatmap](results/figures/fact_type_performance.png) |

| Latency Comparison | Hybrid Weights |
|-------------------|----------------|
| ![Latency](results/figures/latency.png) | ![Hybrid](results/figures/hybrid_heatmap.png) |

---

## Dataset

- **6,000** fact-retrieval attempts  
- **5** conversation lengths (10, 20, 30, 40, 50 turns)  
- **4** fact types (pets, hobbies, cities, colors)  
- **6** memory strategies (latest, similarity, 4 hybrids)  
- **200** runs per condition  
- **Fixed random seed 42** for perfect reproducibility  

---

## Phase 2: Coming Soon

Phase 2 will extend this work with:

- **5+ embedding models** (mpnet, BGE, E5, OpenAI)  
- **10+ fact types** (food, music, movies, sports, etc.)  
- **100+ turn conversations**  
- **Fuzzy evaluation** (embedding similarity, LLM judges)  
- **Real conversation logs** (customer service, therapy, etc.)  

> Follow this repository for updates!

---

## Limitations

- Single embedding model (all-MiniLM-L6-v2)  
- Synthetic conversations only  
- Exact-match evaluation (no partial recall)  
- Limited to 50-turn conversations  

---

## Run in Colab

[![Open in Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://github.com/aitmoutmeryem-hue/conversational-ai-memory-benchmark-phase1/blob/main/code/phase1_benchmark.ipynb)

---

## Quick Start

git clone https://github.com/aitmoutmeryem-hue/conversational-ai-memory-benchmark-phase1.git
cd conversational-ai-memory-benchmark-phase1

pip install -r requirements.txt

# Open the notebook locally
jupyter notebook code/phase1_benchmark.ipynb
