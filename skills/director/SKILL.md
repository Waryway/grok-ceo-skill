---
name: director
description: "Director planner for the org pipeline. Use for /director, plan sections, seed rubrics, fill managers-queue. Root may spawn managers; CEO child never spawns."
type: orchestrator
lifecycle: active
user-invocable: true
argument-hint: "<project-slug> [plan|orchestrate|verify]"
metadata:
  short-description: "Director — plan, rubrics, managers queue"
---

# Director Skill

**Canonical:** `~/.grok/agents/director.md`. Templates: `references/`.

Standalone: `/director <slug>` → plan then orchestrate (root spawn allowed).  
Under `/ceo`: child only — queue + `DELEGATION_READY managers`.
