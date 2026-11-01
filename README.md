# Dynamic-Prompt-Router
A cost-effective AI router using Google Gemini API and hierarchical decision-tree memory indexing for rapid hackathon prototyping.
# Dynamic Prompt Router & Context Reduction Trees

An AI orchestration architecture designed for resource-efficient Hackathon prototyping. This project optimizes API token spending and prevents context window bloating by pairing **Dynamic Prompt Routing** with a **Hierarchical Decision-Tree Context Structure**.

## 📌 Architecture Overview

Instead of sending every user prompt to an expensive, high-compute model with a massive historical transcript attached, this system uses a lightweight "traffic cop" model (`gemini-3.1-flash-lite`) to intercept, classify, and route the query. 

```text
                          [ User Input Query ]
                                   │
                                   ▼
                 ┌───────────────────────────────────┐
                 │  Fast Router (3.1-Flash-Lite)     │
                 │  1. Extracts domain category      │
                 │  2. Evaluates compute complexity  │
                 └─────────────────┬─────────────────┘
                                   │
             ┌─────────────────────┴─────────────────────┐
             ▼                                           ▼
  [ Simple Task Route ]                       [ Complex Task Route ]
    (e.g., Factual Q&A)                         (e.g., Code/Analysis)
    gemini-3.1-flash-lite                       gemini-3.5-flash-lite
             │                                           │
             └─────────────────────┬─────────────────────┘
                                   ▼
                       [ Targeted Context Shard ]
                  (Fetched from Nested Memory Dictionary)
                                   │
                                   ▼
                        [ Optimized Response ]
```
💡 Core Capabilities
1. Context Sharding (Memory Management)
To eliminate redundant token usage, user memory is stored locally in a nested dictionary rather than appended to every API call. The fast router analyzes the prompt, identifies the core domain (e.g., career, fitness, gaming), and fetches only that specific context shard to inject into the final payload.

2. Compute-Aware Execution
Tier 1 (Simple Execution): Factual questions and basic text manipulations are processed by gemini-3.1-flash-lite to ensure ultra-low latency and minimal cost.

Tier 2 (Complex Execution): Heavy reasoning, coding, and analytical tasks are dynamically routed to the heavy-compute gemini-3.5-flash-lite model.

💻 Tech Stack
Language: Python 3.11

Libraries: google-generativeai, pandas, numpy

Models: Google Gemini API (3.1-flash-lite, 3.5-flash-lite)

Environment: Google Colab

🚀 Execution Example
```text
--- Processing New Prompt ---
User: Name the three primary colors.

[System] Selected Context Shard: GENERAL
[System] AI Router Classification: SIMPLE
[System] Routing to GEMINI-3.1-FLASH-Lite (Light Compute)

Response:
The three primary colors (in the traditional RYB color model used in art) are red, yellow, and blue.
