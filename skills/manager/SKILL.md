---
name: manager
description: "Manager iteration lead. Use for /manager, work orders, worker queues, rubric-aware checkpoints. Root spawns workers; CEO child returns DELEGATION_READY workers."
type: orchestrator
lifecycle: active
user-invocable: true
argument-hint: "<project-slug> <section-id> [prepare|checkpoint]"
metadata:
  short-description: "Manager — iterations, workers queue, checkpoint"
---

# Manager Skill

**Canonical:** `~/.grok/agents/manager.md`. Templates: `references/`.

Never use `general-purpose` for Worker/Verifier. Spawn types: `worker`, `verifier`.
