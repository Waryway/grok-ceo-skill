# Token efficiency (org pipeline)

**Less rediscovery, smaller prompts, same quality.**

## Route

Multi-step → CEO. Short Q → no org. Explicit `/director` `/manager` `/worker` or a named playbook → that path only.

## Read budget

| Role | Open |
|------|------|
| CEO | Directive, dispatch/queues, plan, section status, latest score |
| Director | Directive + ≤3 context seeds |
| Manager | Section + iteration + rubric + workers queue |
| Worker | **Work order only** (+ Edit-only files) |
| Verifier | Iteration + acceptance + rubric + listed commands |

Never re-walk the monorepo if seeds / AGENTS / prior `.grok/org/*/artifacts` already answer it.

## Spawn prompts

Children load `~/.grok/agents/<role>.md`. Do **not** say “read the full skill.” Max ~12 lines — see `~/.grok/skills/ceo/references/spawn-prompts-compact.md`.

## Concert + artifacts

- Independent queue rows → **one turn**, many spawns.
- Decisions live in tickets/scorecards/craft-log, not long chat.
- Human updates: SIGNAL counts, rubric totals, blockers — not full child transcripts.
