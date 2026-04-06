---
title: PPO Loss
tags: [concept, reinforcement-learning, optimization]
sources: [[training-multi-agent-grpo]]
created: 2026-04-06
updated: 2026-04-06
---

# PPO (Proximal Policy Optimization) Loss

The optimization objective used within [[grpo-algorithm]] to update the policy model. Prevents overly large policy updates that could destabilize training.

## Components

### 1. Clipped Surrogate Loss

```
ratio = exp(log π_new(a|s) - log π_old(a|s))
surr1 = ratio × advantage
surr2 = clamp(ratio, 1-ε, 1+ε) × advantage
loss = -min(surr1, surr2)
```

The importance ratio `π_new / π_old` is clamped to `[1 - ε, 1 + ε]` (default ε = 0.2), preventing the new policy from deviating too far from the old in a single update.

### 2. KL Divergence Penalty

```
kl_div = log π_new(a|s) - log π_ref(a|s)
total_loss = policy_loss + kl_coef × kl_div
```

Regularizes against the reference model (initial policy before LoRA adapters). Prevents catastrophic forgetting.

## Log Probability Computation

- During rollout: `output_scores=True` captures per-token logits.
- Convert to log probs via `F.log_softmax`.
- Gather log probs for the specific tokens actually generated.

## Referenced In

- [[grpo-algorithm]] — PPO loss is the core optimizer within GRPO training
