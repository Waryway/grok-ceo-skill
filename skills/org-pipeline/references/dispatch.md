# Dispatch — flat fan-out + proxy (org pipeline)

**Platform:** nesting depth **1**. Only the root session may `spawn_subagent`. Role hierarchy lives in tickets/queues; the spawn tree is **flat**.

**Canonical role behavior:** `~/.grok/agents/{ceo,director,manager,worker,verifier}.md`.  
**CEO compact prompts:** `~/.grok/skills/ceo/references/spawn-prompts-compact.md`.  
**Rubric:** `rubric-protocol.md`. **Tokens:** `token-efficiency.md`.

## Spawn tree

```
Root (CEO, or standalone Director/Manager)
  ├── director   MODE=plan | verify
  ├── manager    MODE=prepare | checkpoint
  ├── worker
  └── verifier
```

| Intent | `subagent_type` |
|--------|-----------------|
| Plan / sign-off | `director` |
| Section prepare / checkpoint | `manager` |
| Implement one work order | `worker` |
| Score / build-test | `verifier` |

**Never** `general-purpose` for org roles.

## Who spawns

| Root | Spawns |
|------|--------|
| `/ceo` | Everyone (Director, Managers, Workers, Verifiers) as direct children |
| `/director` standalone | Managers; proxies Worker wave when Managers return `DELEGATION_READY workers` |
| `/manager` standalone | Workers after prepare |

**Children never spawn.** They write queue JSON and return `SIGNAL: DELEGATION_READY …`. Root executes the wave (`proxy_for` in dispatch-state).

## Files

`{REPO}/.grok/org/<slug>/delegations/`:

| File | Writer |
|------|--------|
| `dispatch-state.json` | CEO |
| `managers-queue.json` | Director |
| `workers-queue.json` | Manager |
| `verifiers-queue.json` | Manager |

Templates: `~/.grok/skills/ceo/references/*-queue.template.json`.

```json
{ "spawn_tree": "flat", "concert": true, "proxy_for": null, "proxy_wave": null }
```

`proxy_for`: `"director"` | `"manager"` | `null` during proxy waves.

## CEO phases

```
director_planning → director_orchestrate → worker_dispatch
  → verifier_dispatch → manager_checkpoint → (loop)
  → director_verify → ceo_review
```

| Phase | Spawn |
|-------|-------|
| `director_planning` | 1× director plan |
| `director_orchestrate` | all pending managers prepare (concert) |
| `worker_dispatch` | all pending workers (`background: true`, concert) |
| `verifier_dispatch` | pending verifiers |
| `manager_checkpoint` | manager checkpoint per section |
| `director_verify` | 1× director verify |
| `ceo_review` | none — rubric + criteria |

## Signals

| SIGNAL | From | Root does |
|--------|------|-----------|
| `DELEGATION_READY managers` | Director | concert managers |
| `DELEGATION_READY workers` | Manager prepare | concert workers |
| `DELEGATION_READY verifier` | Manager | spawn verifier(s) (**required** after product workers) |
| `SECTION_DONE` | Manager checkpoint | next section or director verify (needs verifier pass or Override log) |
| `SECTION_BLOCKED` | Manager | escalate / human — do not auto-ship |
| `DIRECTOR_DONE` | Director verify | CEO review (Director must cite score evidence) |
| `AWAIT_HUMAN_SHIP` | CEO | pause for human on residual / `ship_best_so_far` |

## Ship hardening (summary)

- Verifier wave after workers (default).
- Manager: no `SECTION_DONE` on failed verify without **Override log**.
- Director verify: scorecards/totals on disk.
- CEO `done`: directive **Ship evidence** table complete (paths, totals, floors, verifier ran).

## Concert

Same turn → all independent queue rows (no `depends_on` / no shared file lock). Serialize only on real dependencies. Workers: prefer `background: true`, wait for wave, then verifier/checkpoint.

## Ticket layout

```
.grok/org/<slug>/
  directive.md  plan.md  rubrics/
  sections/  iterations/  work-orders/
  artifacts/scores/  artifacts/craft-log.md
  delegations/*.json
```

Status values: `pending` · `in_progress` · `blocked` · `done` · `rejected`.
