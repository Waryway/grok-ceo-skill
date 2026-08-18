---
name: ceo
description: "CEO orchestrator for multi-phase project delivery. Use for /ceo, run the org, delegate this goal, multi-section completion, improve/fix/build loops. Default for multi-step work even when /ceo is omitted. Short Q&A stays direct."
type: orchestrator
lifecycle: active
user-invocable: true
argument-hint: "[goal | status <slug> | resume <slug>]"
metadata:
  short-description: "CEO — multi-step org, flat fan-out, rubric pass"
---

# CEO

**Behavior:** `~/.grok/agents/ceo.md` (single source of truth).

```
/ceo <goal>
/ceo status <slug>
/ceo resume <slug>
```

Multi-step work without `/ceo` → still run as CEO. Short questions → answer, no org.

**Templates:** `references/` (directive, queues, compact spawn prompts).  
**Hardening:** ship evidence on directive; required verifier; override log if shipping past fail — see agent.  
**PR/MR:** create/push → wait Copilot → fix → re-wait on new head (re-trigger ≤2). If Copilot goes quiet: **`COPILOT_STALLED`** residual + stop polling (do not hang). Stacks: every PR clean **or** stalled; restack; retarget; babysit ≤~**8h**. Waiters: **`worker`+worktree**. Detail: `references/pr-mr-loop.md`.  
**Deep protocols (open only if stuck):** `~/.grok/skills/org-pipeline/references/dispatch.md`, `rubric-protocol.md`.
