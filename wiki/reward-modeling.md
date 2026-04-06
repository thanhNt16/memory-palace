---
title: Reward Modeling
tags: [concept, reinforcement-learning, evaluation]
sources: [[training-multi-agent-grpo]]
created: 2026-04-06
updated: 2026-04-06
---

# Reward Modeling

The mechanism for assigning scalar rewards to agent trajectories, used within [[grpo-algorithm]] to compute advantages and update the policy.

## Judge-Based Reward

- Uses **GPT-4o** as an external, impartial judge.
- Compares the agent's final answer against ground truth.
- Produces a **binary reward**: 1.0 (correct) or 0.0 (incorrect).
- Forces structured output via a Pydantic model (`AnswerVerification`).

## Prompt Structure

The judge receives:
- The original query
- The ground truth answer
- The agent's final answer
- Instruction to evaluate correctness

## Fallback

If the judge API call or structured parsing fails, falls back to simple string matching: `ground_truth.lower() in final_answer.lower()`.

## Why Binary Reward

GRPO's group-relative advantage mechanism works with binary rewards because it normalizes across the group. A trajectory scoring 1.0 in a group averaging 0.25 gets a high positive advantage (+0.75), while failed trajectories (0.0) get negative advantage (-0.25).

## Referenced In

- [[grpo-algorithm]] — reward is the input signal for advantage computation
- [[training-multi-agent-grpo]] — full implementation
