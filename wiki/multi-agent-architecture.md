---
title: Multi-Agent Architecture
tags: [concept, agent-design, architecture]
sources: [[training-multi-agent-grpo]]
created: 2026-04-06
updated: 2026-04-06
---

# Multi-Agent Architecture

A framework where a complex query is solved by a series of specialized agents that collaborate in a loop. Each agent has a distinct role in the problem-solving pipeline.

## Core Roles

### Planner (Policy Model)
- Analyzes the query and decides the next best action at each step.
- Selects which tool to use and defines a sub-goal.
- **This is the trainable component** — improved via [[grpo-algorithm]].

### Executor
- Translates the planner's high-level sub-goal into precise, executable Python code.
- Calls the selected tool with correct arguments.

### Verifier
- Reviews the full action history and current state after each step.
- Determines if the query is fully answered (`stop_signal = True`) or if more steps are needed.

## Agentic Loop

```
Query → Analyze → [Plan → Execute → Verify] → Synthesize → Final Answer
                        ↑___________|
                        (repeat until stop or max_steps)
```

1. **Analysis** — one-time breakdown of query intent, required skills, relevant tools.
2. **Plan** — choose tool and sub-goal for this iteration.
3. **Execute** — generate and run tool command.
4. **Verify** — check completeness; continue or stop.
5. **Synthesize** — combine all results into final answer.

## Tool Types

| Tool | Purpose | Requires LLM |
|------|---------|-------------|
| `Python_Coder_Tool` | Execute Python code for calculations | Yes |
| `Wikipedia_RAG_Search_Tool` | Search Wikipedia with RAG pipeline | Yes |
| `Google_Search_Tool` | Web search via Gemini API with grounding | No |
| `Base_Generator_Tool` | General LLM answer generation | Yes |

## Key Data Structures

- **`QueryAnalysis`** — structured breakdown of query (skills, tools, considerations)
- **`NextStep`** — planner's decision per step (tool, sub-goal, context, justification)
- **`ToolCommand`** — executor's generated command
- **`MemoryVerification`** — verifier's stop/continue decision
- **`Memory`** — append-only log of all actions and results

## Referenced In

- [[training-multi-agent-grpo]] — implementation and GRPO training of this architecture
