# Spawn prompts — compact

Children load agent defs. **Do not** instruct “read SKILL.md.”

Prefer CEO copy in `~/.grok/skills/ceo/references/spawn-prompts-compact.md`.

## Root Director → manager prepare

```
MODE: prepare
CHILD: no spawn   # if this Manager will be depth-1 under you as root Director, omit CHILD and allow spawn
slug: {project-slug}
section: {REPO}/.grok/org/{project-slug}/sections/{section-id}.md
rubric: {rubric-path}
Create iteration + work orders; fill workers-queue + verifiers-queue.
Return SIGNAL: DELEGATION_READY workers if you cannot spawn; else concert-spawn workers.
```

## Root Director → manager checkpoint

```
MODE: checkpoint
slug: {project-slug}
section-id: {section-id}
Workers/verifier finished. Update tickets. SIGNAL: SECTION_DONE | DELEGATION_READY workers | SECTION_BLOCKED
```
