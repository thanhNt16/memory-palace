---
title: QLoRA and PEFT
tags: [concept, model-training, efficiency]
sources: [[training-multi-agent-grpo]]
created: 2026-04-06
updated: 2026-04-06
---

# QLoRA and PEFT for Agent Training

Parameter-efficient fine-tuning techniques used to train the planner policy model in [[training-multi-agent-grpo]].

## QLoRA (Quantized LoRA)

- Loads the base model in **4-bit precision** (NF4 quantization) via `BitsAndBytesConfig`.
- Compute dtype: `bfloat16`.
- Drastically reduces VRAM usage while preserving model quality.

## LoRA Configuration

| Parameter | Value | Notes |
|-----------|-------|-------|
| `r` | 16 | Rank of low-rank decomposition |
| `lora_alpha` | 32 | Scaling factor |
| `target_modules` | q, k, v, o, gate, up, down projections | All major transformer layers |
| `lora_dropout` | 0.05 | Regularization |
| `task_type` | CAUSAL_LM | Autoregressive language modeling |

## Results

- Base model: `Qwen/Qwen2-1.5B-Instruct` (1.5B parameters).
- Trainable: **16.7M parameters (1.1%)** after LoRA application.
- This is the policy/planner model. The executor and verifier use a larger fixed model (`Qwen2.5-7B-Instruct`).

## Reference Model

The reference model (`ref_model`) starts identical to the policy model. During PPO loss computation, `ref_model.disable_adapter()` is used to compute reference log probs without LoRA weights — this is the KL penalty baseline.

## Referenced In

- [[training-multi-agent-grpo]] — the training pipeline using QLoRA + PEFT
