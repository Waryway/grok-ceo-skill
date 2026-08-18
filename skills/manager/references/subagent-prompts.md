# Spawn prompts — Manager → Worker / Verifier (compact)

Children load agent defs. **Do not** say “read SKILL.md.”  
CEO should prefer `~/.grok/skills/ceo/references/spawn-prompts-compact.md`.

## Worker

```
CHILD: no spawn
work-order: {REPO}/.grok/org/{project-slug}/work-orders/{order-id}.md
Execute Edit-only; update ticket; return status/summary/blockers.
```

## Verifier

```
CHILD: no spawn
section: {section-id}
iteration: {NN}
root: {REPO}/.grok/org/{project-slug}/
rubric: {rubric-path-or-from-section}
Run queue commands and/or rubric scorecard. Update iteration Results + Remaining gaps.
Return status pass|fail + total/verdict + ≤3 gaps.
```

## Manager prepare (CEO proxy)

```
MODE: prepare
CHILD: no spawn
proxy: CEO for Director
section: {REPO}/.grok/org/{project-slug}/sections/{section-id}.md
rubric: {rubric-path}
Create iteration + work orders; fill workers-queue + verifiers-queue.
SIGNAL: DELEGATION_READY workers
```

## Manager checkpoint (CEO proxy)

```
MODE: checkpoint
CHILD: no spawn
proxy: CEO for Director
section-id: {section-id}
Workers/verifier finished. Update tickets.
SIGNAL: SECTION_DONE | DELEGATION_READY workers | SECTION_BLOCKED
```
