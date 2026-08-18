# Directive — {project_title}

**Project slug:** {project_slug}  
**Status:** pending  
**Created:** {date}  
**CEO round:** 0

## Human direction

{human_goal}

## Success criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Rubric **pass** (≥ pass bar) or human-accepted `ship_best_so_far` with residual

## Context seeds (≤3 — anti-rediscovery)

| # | Path | Why |
|---|------|-----|
| 1 | {seed_1 e.g. apps/…/AGENTS.md} | Product map / hard locks |
| 2 | {seed_2 e.g. .grok/org/…/artifacts/…} | Prior art / SMOKE / craft-log |
| 3 | {seed_3 optional} | Key code or design doc |

Agents open **only** these plus tickets they own. No monorepo rediscovery.

## Rubric

| Field | Value |
|-------|-------|
| Path | `.grok/org/{project_slug}/rubrics/{name}.md` _(create if missing)_ |
| Pass bar | ≥ 78 / 100 _(or playbook)_ |
| Scorecards | `.grok/org/{project_slug}/artifacts/scores/` |
| Loop log | `.grok/org/{project_slug}/artifacts/craft-log.md` |

**Validate with a rubric** every CEO review. Verdicts: `pass` \| `needs_more` \| `ship_best_so_far`.

## Budgets

| Cap | Value |
|-----|------:|
| Max CEO rounds | 3 |
| Max iters / section | 3 |
| Max sections | 6 |
| Human gate on ship residual | yes |

## Constraints

- Single-repo: {repo_name}
- Route via Context seeds + product AGENTS — not full-repo exploration
- **Verifier required** on product/code sections (default)
- Tests/SMOKE green or documented N/A before CEO marks done
- Anti-regression locks from seed AGENTS (list extras below)

### Extra locks

- 

## CEO verdict log

| Round | Date | Verdict | Rubric total | Notes |
|-------|------|---------|-------------:|-------|
| 0 | {date} | pending | — | Directive issued |

## Ship evidence (fill at ceo_review — required for done)

| Field | Value |
|-------|-------|
| Sections done | 0 / 0 |
| Scorecard paths | |
| Latest total / verdict | |
| Hard floors | ok / failed: … |
| Verifier ran | yes / no |
| Residual (if any) | |
| Human accepted residual | n/a / yes / **AWAIT** |

### PR / stack evidence (when shipping via GitHub)

| Field | Value |
|-------|-------|
| PR stack order | e.g. #128 → #129 → #130 → #131 |
| Per-PR bases / branches | |
| Per-PR latest head SHA | |
| Per-PR Copilot | received / re-triggered×N / timeout — **every** PR |
| Unaddressed review threads | none / list |
| STACK_READY at | n/a / timestamp |
| Main CI URL (post-merge) | |

## Director handoff summary

- Plan: `.grok/org/{project_slug}/plan.md`
- Sections complete: 0 / 0
- Latest rubric: —
- Blockers: —

## CEO satisfaction checklist

- [ ] All plan sections `done` (or dropped with note)
- [ ] Success criteria met
- [ ] Ship evidence table complete
- [ ] Rubric pass **or** human-accepted residual
- [ ] No open SECTION_BLOCKED without disposition
- [ ] Human has not objected this round
- [ ] If PRs: every stack PR review-clean on **latest** head (or residual logged)
- [ ] If stack: STACK_READY or all merged + main CI green

**CEO final status:** pending
