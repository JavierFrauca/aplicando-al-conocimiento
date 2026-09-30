---
description: Model selection per agent in Claude Code dynamic workflows.
---

# Dynamic workflows — per-agent model selection

Before launching a dynamic workflow, analyse the role of each agent and assign the most appropriate model explicitly — never inherit the main session model by default.

Selection criteria:

- **`opus`** — complex reasoning, architectural design, synthesis across multiple sources, adversarial verification, quality judgements.
- **`sonnet`** — general-purpose tasks with a cost/quality balance: code analysis, structured generation, exploration.
- **`haiku`** — repetitive or low-reasoning work: parsing, short summaries, formatting, simple classification.

Apply the model at the agent level (`opts.model`) or phase level (`model` field in `meta.phases`). If a phase mixes agents of different complexity, override per agent.
