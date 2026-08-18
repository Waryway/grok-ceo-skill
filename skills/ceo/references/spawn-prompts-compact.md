# Compact spawn prompts (CEO)

Children load `~/.grok/agents/<role>.md` automatically. **Do not** tell them to re-read skill files.

Replace `{REPO}`, `{slug}`, `{section-id}`, `{order-id}`, `{rubric}`, `{PR_URL}`, `{BRANCH}`, `{BASE}`.

**Spawn types under CEO:** `director` · `manager` · `worker` · `verifier` only — **never** `general-purpose`.

## Director plan

```
MODE: plan
CHILD: no spawn
slug: {slug}
directive: {REPO}/.grok/org/{slug}/directive.md
Seed only context in directive. Plan + sections + managers-queue.
If multi-PR ship: note merge-order stack in plan (bases linear).
Require rubrics/ + Verifier required=yes on product sections.
Return SIGNAL: DELEGATION_READY managers
```

## Director verify

```
MODE: verify
CHILD: no spawn
slug: {slug}
root: {REPO}/.grok/org/{slug}/
Require sections done + scorecard/verifier evidence (not vibe).
Return SIGNAL: DIRECTOR_DONE only if evidence OK; else blockers
```

## Manager prepare

```
MODE: prepare
CHILD: no spawn
proxy: CEO for Director
slug: {slug}
section: {REPO}/.grok/org/{slug}/sections/{section-id}.md
rubric: {rubric}
Create iteration + Edit-only work orders.
Queue workers AND verifier (required unless waived).
Return SIGNAL: DELEGATION_READY workers
```

## Manager checkpoint

```
MODE: checkpoint
CHILD: no spawn
proxy: CEO for Director
slug: {slug}
section-id: {section-id}
Workers+verifier finished. SECTION_DONE only if verifier pass
or Override log on iteration. Else workers again or SECTION_BLOCKED.
SIGNAL: SECTION_DONE | DELEGATION_READY workers | SECTION_BLOCKED
```

## Worker (product)

```
CHILD: no spawn
proxy: CEO for Manager
work-order: {REPO}/.grok/org/{slug}/work-orders/{order-id}.md
Execute Edit-only files; update ticket; return status/summary/blockers.
```

## Verifier

```
CHILD: no spawn
proxy: CEO for Manager
slug: {slug}
section: {section-id}
iteration: {NN}
rubric: {rubric}
Independent review: run commands + score rubric honestly (no inflation).
Write scorecard if rubric; update iteration gaps.
Return pass|fail + total/verdict/floors + <=3 gaps
```

## Worker — PR Copilot waiter (one PR; stack = concert N workers)

```
CHILD: no spawn
subagent_type: worker
isolation: worktree preferred
PR: {PR_URL}  Branch: {BRANCH}  Base: {BASE}
Wait hard-cap 12m/head for Copilot (poll ~60s). Fix+test+push+reply on THIS branch only.
After every push: RESTART wait (12m cap) on new head.
Re-trigger max 2 if silent after CI/wait. Then COPILOT_STALLED: log, PR comment, STOP polling.
Never hang. Never merge. Never other stack branches. Cap 3 fix commits unless CEO expands.
Return: head SHA, comments fixed, re-triggers, stalled yes/no, CI.
```

## Stack babysit (CEO runs monitor — not a child role)

```
CEO tool: monitor
Poll stack PRs every 60-90s; retarget next base->main after lower merge.
On new Copilot comments/CI fail: CEO spawns fix worker / restacks.
Copilot stalled on a PR is OK residual (do not wait 8h for it).
Exit STACK_READY or ALL_MERGED_MAIN_CI_OK or timeout residual (<=~8h).
```
