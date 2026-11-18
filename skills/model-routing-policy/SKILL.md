---
name: model-routing-policy
description: Route substantial Codex or ChatGPT Work tasks across GPT-6.1 Terra, GPT-6.2 Sol, and GPT-6.2 Astra using cost-aware delegation and bounded escalation. Skip simple tasks with no useful decomposition.
---

# Cost-aware model routing

Preserve explicit user model, reasoning effort, and workflow choices. The configured local lead is GPT-6.2 Sol high; an instruction file cannot change an already-running lead. Keep task understanding, planning, architecture, coordination, integration, review, and final decisions with Sol.

Use these named local custom agents when the host exposes them:

- `routine_worker`: GPT-6.1 Terra medium for clear, repeatable, mechanical, or high-volume work.
- `ordinary_worker`: GPT-6.2 Sol medium for ordinary implementation, debugging, testing, and bounded analysis.
- `frontier_solver`: GPT-6.2 Sol max for complex reasoning, architecture, and difficult bounded problems.
- `exceptional_solver`: GPT-6.2 Astra xhigh for the smallest problem still unresolved after materially different approaches, or requiring exceptional complexity, uncertainty, or unusually large context.

Unnamed subagents default to Terra medium. Terra low and Astra max are opt-in only.

## Manual model switches

Delegate eligible subtasks automatically without asking the user to switch models. A stronger worker does not change the Lead. Only request a manual switch if the next step requires a different Lead model or effort, automatic routing cannot meet that requirement, and the target is available to the user. If availability is unknown or the target is unavailable, report the limitation and verified alternatives; do not guess or silently substitute.

Pause before the affected step and use the following user-facing format:
「需要你手動切換｜目標：<模型> · <推理強度>」
「原因：<為什麼這一步必須換主對話，不能只派子代理>」
「操作：<針對目前 CLI 或 Desktop 的具體步驟>」
「切換後：<如何接續任務>」

Give only controls or commands verified for this client. If a new chat is necessary, supply a copyable handoff with the goal, completed work, constraints, and next step before asking the user to leave. Wait for confirmation; distinguish user confirmation from runtime verification and never claim an unverified switch.

Prefer the cheapest configured role that can reliably finish each subtask. Keep small or tightly coupled work on the lead when handoff overhead outweighs the benefit. Parallelize only independent work. Assign explicit file/module ownership, tell workers they are not alone in the codebase, and preserve others' edits.

Escalate the smallest unresolved question with its constraints, evidence, decisions, and failed approaches. Have Sol integrate and verify the result. Do not select Astra merely because a task is large or because a cheaper attempt has not yet been made.

Local Codex discovers global custom agent TOML files under `~/.codex/agents/`. Respect their configured model and effort instead of overriding them incidentally. In ChatGPT Work or another host, use the same role split only when the requested model/effort is exposed; local files do not prove cloud support. Verify actual host selection when possible and disclose missing roles or supported substitutions. Do not rely on an agent's self-reported model as runtime evidence.
