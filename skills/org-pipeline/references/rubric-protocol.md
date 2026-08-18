# Rubric protocol (org pipeline — default validation)

**Validate with a rubric.** Every multi-step org run scores work against a written 100-pt rubric before CEO marks done. No “looks fine” closures.

## Nomenclature (use these terms)

| Term | Meaning |
|------|---------|
| **Rubric** | Named MD with dimensions, max points (sum **100**), pass bar, hard floors |
| **Scorecard** | One filled scoring of a subject at a moment (iter N) |
| **Baseline** | First honest score before improvements |
| **Pass bar** | Default **≥ 78 / 100** (playbooks may raise; never silently lower) |
| **Hard floors** | Per-dimension minimums that block `pass` even if total ≥ 78 |
| **Bottom-3 gaps** | Top ≤3 actionable defects (path + fix intent) |
| **Verdict** | `pass` \| `needs_more` \| `ship_best_so_far` |
| **Craft / loop log** | Append-only iter log: strategy, before→after, ship?, residual |
| **Strategy** | What this iter tried (A/B/C… or free-text one-liner) |

## Always

1. **Look for a rubric first** under, in order:
   - `{REPO}/.grok/org/<project-slug>/rubrics/`
   - Playbook skill references (e.g. `.grok/skills/3d-craft-rubric/references/rubric.md`)
   - Prior sibling org project rubrics for the same product area
   - Generic delivery rubric template (below) if none exists
2. **If none exists → create one** at  
   `{REPO}/.grok/org/<project-slug>/rubrics/<name>.md`  
   before the first worker wave (Director seeds; Manager may refine dimensions).
3. **Baseline score** before claiming improvement.
4. **Re-score after each meaningful iter**; append craft/loop log.
5. **CEO done** requires rubric **pass** (or human-accepted `ship_best_so_far` with residual listed).

## Paths (per project)

```
{REPO}/.grok/org/<project-slug>/
  rubrics/
    <name>.md                 # dimensions + pass bar
    scorecard-template.md     # optional copy form
  artifacts/
    scores/                   # filled scorecards
    craft-log.md              # or loops/<area>/loop-log.md
```

Directive and section tickets **must** link the rubric path + pass bar.

## Generic delivery rubric (create when domain rubric missing)

Copy into `rubrics/delivery.md` and tune dimensions to the goal.

```markdown
# Delivery rubric — <project-slug>

**Pass bar:** ≥ 78 / 100  
**Verdicts:** pass | needs_more | ship_best_so_far

## Dimensions

| Dimension | Max | What “great” means |
|-----------|----:|--------------------|
| Correctness | 20 | Meets success criteria; no known broken path |
| Scope discipline | 12 | Only intended surfaces; no drive-by refactors |
| Tests / smoke | 16 | Commands in ticket green; SMOKE path if UI |
| Anti-regression | 16 | Hard locks from AGENTS / prior art held |
| UX / API clarity | 12 | Operator can use without rediscovery |
| Docs / tickets | 12 | Org artifacts + AGENTS pointers updated |
| Observability | 12 | Failures leave logs, scores, residual gaps |
| **Total** | **100** | |

## Hard floors (fail pass if unmet)

| Dimension | Floor |
|-----------|------:|
| Correctness | 14 |
| Anti-regression | 10 |

## Scorecard row

| Field | Value |
|-------|-------|
| Date | |
| Iteration | |
| Total | /100 |
| Verdict | |
| Bottom-3 | 1… 2… 3… |
```

## Domain rubrics (examples — do not reinvent if present)

| Domain | Typical location |
|--------|------------------|
| 3D plates vs 2D bar | `{REPO}/.grok/skills/3d-craft-rubric/references/rubric.md` |
| Quanta play areas | `.grok/org/quanta-realms-play-optimize-loop/rubrics/` |
| Agentic docs | `.grok/org/docs-agentic-qr-10loop/rubrics/agentic-docs-rubric.md` |
| Fleet / tech / heroes loops | matching `.grok/org/quanta-realms-*/rubrics/` or artifacts |

## Loop (when goal is improve-until-pass)

```
baseline score → diagnose ≤3 gaps → one strategy → re-score → log → repeat
```

Stop when: **pass**, max iters + `ship_best_so_far`, or human cancel.  
**No score inflation.** Honest baselines for rough work often land 35–55.

## Verifier role

Verifier **prefers rubric scorecard** over vibe. Return:

```
status: pass|fail
rubric: <path>
total: NN/100
verdict: pass|needs_more|ship_best_so_far
gaps:
  - ...
```

## Ship evidence (CEO / Director)

Before project `done`, artifacts must show:

| Evidence | Where |
|----------|--------|
| Scorecard path(s) | `artifacts/scores/` |
| Total + verdict + floors | scorecard or iteration |
| Verifier ran | iteration Verifier row / queue |
| Residual + human OK | directive Ship evidence (if not full pass) |

Manager may `SECTION_DONE` past a fail **only** with an **Override log** on the iteration (reason, residual, risk).

## Anti-patterns

- Marking CEO done with no scorecard
- Worker self-score counted as Verifier
- Reusing a 3D rubric for pure API work (wrong media class)
- Claiming pass without hard floors
- Score inflation / rubber-stamp verify
- Re-discovering prior scores instead of reading `artifacts/scores/`
