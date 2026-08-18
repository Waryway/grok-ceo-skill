---
name: org-pipeline
description: "Multi-phase AI delegation: CEO to Director to Manager to Worker with flat fan-out and rubric-validated delivery. Use for /ceo, /director, /manager, /worker, run the org, multi-section delivery."
type: orchestrator
lifecycle: active
user-invocable: true
argument-hint: "[goal or project-slug]"
metadata:
  short-description: "Org pipeline — flat fan-out + rubric validation"
---

# Org Pipeline

**Default path:** `/ceo` → agent `~/.grok/agents/ceo.md`.

| Role | `subagent_type` |
|------|-----------------|
| CEO / Director / Manager / Worker / Verifier | matching name |

Skills = slash + templates. **Never** `general-purpose` for org roles.

## References (open only when needed)

| Doc | When |
|-----|------|
| `references/dispatch.md` | Phases, proxy, signals, concert |
| `references/rubric-protocol.md` | Scorecards, pass bar, craft log |
| `references/token-efficiency.md` | Read budgets, compact prompts |

```
/ceo <goal>
```
