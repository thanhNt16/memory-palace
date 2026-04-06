---
title: "Training Multi-Agentic Systems for Complex Task Planning with GRPO Algorithm"
tags: [source, tutorial, reinforcement-learning, multi-agent]
sources:
  - "https://freedium-mirror.cfd/https://levelup.gitconnected.com/training-multi-agentic-systems-for-complex-task-planning-with-grpo-algorithm-f698e831d730"
created: 2026-04-06
updated: 2026-04-06
---

# Training Multi-Agentic Systems with GRPO

**Author:** Fareed Khan | **Published:** February 9, 2026 | **Platform:** Level Up Coding / Medium

**GitHub:** [FareedKhan-dev/multi-agent-training-grpo](https://github.com/FareedKhan-dev/multi-agent-training-grpo)

## Summary

Tutorial on applying [[grpo-algorithm]] to train the planning phase of a [[multi-agent-architecture]]. The core insight: untrained agentic systems hallucinate and make poor planning choices in long-horizon tasks; GRPO can improve the planner by generating multiple trajectories per query, comparing outcomes, and reinforcing successful strategies.

## Problem Demonstrated

An untrained agent given "Calculate 12 squared, then find a major historical event in that year AD":
1. Searched for history before confirming the math → wrong year assumption.
2. Got empty Wikipedia results for 144 AD → concluded no events existed.
3. Fell back to generalist tool → hallucinated "Rome expanded trade routes in North Africa."

## Pipeline

1. **Data Preprocessing** — Merged `DeepMath-103K` (103K math problems) + `Natural Questions` (79K real queries) → 182K combined training samples in Parquet format.
2. **Agent Architecture** — Planner/Executor/Verifier loop with 4 tools (Python coder, Wikipedia RAG, Google search, general LLM). LLM engine: `ChatVLLM` wrapping vLLM server with OpenAI-compatible API.
3. **GRPO Training** — Policy model: `Qwen2-1.5B-Instruct` with [[qlora-peft]] (1.1% trainable params). Fixed environment: `Qwen2.5-7B-Instruct`. Reward judge: `GPT-4o`. Loss: [[ppo-loss]] with [[reward-modeling]].
4. **Post-training evaluation** — Trained planner correctly sequences steps (math first, then search), switches tools on failure, produces grounded answers.

## Key Design Decisions

- **Only the planner is trained** — executor and verifier use a fixed, larger model.
- **Group size N=4** — 4 rollouts per query for advantage computation.
- **Binary reward** — GPT-4o judge evaluates final answer against ground truth.
- **QLoRA + LoRA** — 4-bit quantization with rank-16 adapters on all projection layers.

## Code Structure

```
GRPO_Training_Agentic/
├── 01_data_preprocessing.ipynb
├── 02_agentic_architecture.ipynb
├── 03_grpo_training.ipynb
└── utils.py
```

## Cross-References

- [[grpo-algorithm]] — the RL algorithm at the core
- [[multi-agent-architecture]] — the agent design pattern
- [[ppo-loss]] — the optimization objective
- [[qlora-peft]] — efficient fine-tuning approach
- [[reward-modeling]] — how rewards are assigned
