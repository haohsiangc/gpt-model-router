# Cost-aware model routing

Apply this policy to substantial tasks that benefit from decomposition. Explicit user model, effort, and workflow choices take precedence. Keep small or tightly coupled work on the lead when delegation overhead outweighs the benefit.

| Role | Model | Reasoning effort | Responsibility |
| --- | --- | --- | --- |
| Lead | GPT-6.2 Sol (`gpt-6.2-sol`) | high | Understanding, planning, coordination, integration, review, final decisions |
| `routine_worker` | GPT-6.1 Terra (`gpt-6.1-terra`) | medium | Clear, repeatable, mechanical, high-volume subtasks |
| `ordinary_worker` | GPT-6.2 Sol (`gpt-6.2-sol`) | medium | Ordinary implementation, debugging, testing, bounded analysis |
| `frontier_solver` | GPT-6.2 Sol (`gpt-6.2-sol`) | max | Complex reasoning, architecture, difficult bounded problems |
| `exceptional_solver` | GPT-6.2 Astra (`gpt-6.2-astra`) | xhigh | Smallest exceptional problem still unresolved after materially different approaches, or requiring exceptional complexity or unusually large context |

Unnamed subagents default to Terra medium. Terra low and Astra max are opt-in only.

## Manual model switches

Delegate eligible subtasks automatically without asking the user to switch models. A stronger worker does not change the Lead. Only request a manual switch if the next step requires a different Lead model or effort, automatic routing cannot meet that requirement, and the target is available to the user. If availability is unknown or the target is unavailable, report the limitation and verified alternatives; do not guess or silently substitute.

Pause before the affected step and use the following user-facing format:
「需要你手動切換｜目標：<模型> · <推理強度>」
「原因：<為什麼這一步必須換主對話，不能只派子代理>」
「操作：<針對目前 CLI 或 Desktop 的具體步驟>」
「切換後：<如何接續任務>」

Give only controls or commands verified for this client. If a new chat is necessary, supply a copyable handoff with the goal, completed work, constraints, and next step before asking the user to leave. Wait for confirmation; distinguish user confirmation from runtime verification and never claim an unverified switch.

Use the named custom agents when available. Lead model and effort are startup defaults in `config.toml`; these instructions do not switch an existing chat's model. Custom agent files control their own model and effort. A caller's explicit spawn overrides can affect selection, so avoid overriding the custom agent's intended settings without a user request.

Prefer the cheapest configured role that can reliably finish the subtask. Parallelize independent work only. Assign explicit file/module ownership, tell workers they are not alone in the codebase, and preserve others' edits. Keep cross-cutting integration and final review on Sol. Escalate only the smallest unresolved subproblem, passing constraints, evidence, decisions, and failed approaches; have Sol integrate the result.

If a role, model, or effort is unavailable, verify available options and report the limitation before using the closest supported configuration. Never claim a model switch or successful route from agent self-report alone; use host/runtime evidence when available.

For relevant multi-model workflows, also follow the `model-routing-policy` skill when available. This policy authorizes normal in-scope delegation; external writes and communication still require authorization from the task.
