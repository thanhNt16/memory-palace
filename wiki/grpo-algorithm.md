---
title: GRPO Algorithm
tags: [concept, reinforcement-learning, agent-training]
sources: [[training-multi-agent-grpo]]
created: 2026-04-06
updated: 2026-04-06
---

# GRPO (Group Relative Policy Optimization)

A reinforcement learning algorithm designed for multi-agent settings. Instead of grading every action in isolation, GRPO asks the agent to attempt the same problem multiple times (a "group"), compares results, and reinforces strategies that outperform the group average.

## How It Works

1. **Group Rollouts:** For each query, generate N distinct trajectories (e.g., 4 attempts) using the policy model with non-zero temperature for exploration.
2. **Reward Assignment:** An external judge evaluates each trajectory's final answer against ground truth, producing a scalar reward (e.g., 1.0 or 0.0).
3. **Relative Advantage:** `Advantage = (Reward - GroupMean) / GroupStdDev`. Trajectories above the group mean get positive advantage; below get negative.
4. **Policy Update:** Uses [[ppo-loss]] (clipped surrogate) with the advantage signal to update the policy model.

## Why GRPO for Agents

- **Error suppression:** Poor trajectories receive negative advantage, reducing hallucination and incorrect tool usage.
- **Relative comparison:** Learns from group performance rather than absolute single-step rewards.
- **Coordination:** Helps multiple sub-agents align their actions by training in a group context.

## Key Hyperparameters

| Parameter | Typical Value | Purpose |
|-----------|--------------|---------|
| `rollout_n` | 4 | Trajectories per query (group size) |
| `ppo_clip_eps` | 0.2 | Clipping range to prevent drastic updates |
| `kl_coef` | 0.01 | KL-divergence penalty strength |
| `temperature` | 0.7 | Sampling temperature for diverse rollouts |

## Referenced In

- [[training-multi-agent-grpo]] — full implementation using GRPO for agent planning
