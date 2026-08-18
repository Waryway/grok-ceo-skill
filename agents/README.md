# Global Org Agents

User-scoped agent definitions (`~/.grok/agents/*.md`). Spawn with matching `subagent_type`.

| Agent | Role |
|-------|------|
| `ceo` | Root orchestrator — ship evidence gate; budgets; flat fan-out |
| `director` | Plan, rubrics; verify needs scorecards |
| `manager` | Iterations; mandatory verifier; override log |
| `worker` | One work order; Edit-only; no rediscovery |
| `verifier` | Independent score/test; no rubber-stamp |

**Agents** = what `subagent_type` loads. **Skills** = slash + templates (`~/.grok/skills/…`).

Never spawn `general-purpose` for org roles.

| Topic | Path |
|-------|------|
| Dispatch (phases/signals) | `~/.grok/skills/org-pipeline/references/dispatch.md` |
| Rubric | `…/rubric-protocol.md` |
| Tokens | `…/token-efficiency.md` |
| Compact CEO prompts | `~/.grok/skills/ceo/references/spawn-prompts-compact.md` |

Multi-step → CEO. Short Q → no org.
