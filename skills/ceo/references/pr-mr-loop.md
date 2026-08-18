# CEO — GitHub PR / MR + stack merge loop

**Owner:** CEO only (spawns waiters/workers; children never open PRs or merge).  
**Human:** merge cascade (CEO does not merge unless human explicitly delegates).  
**Waiter pattern:** under CEO, spawn **`worker` + worktree** (see `spawn-prompts-compact.md`). Never `general-purpose`.

## Mistake to avoid

1. Copilot is **async** — checking once at create (or only after merge) **misses** review.  
2. After every **push**, the **new head** needs its own wait (previous head clean != current clean).  
3. Copilot often **stops re-reviewing** after a while — **do not block forever**. Use **stall exit** below.

## Roles

| Actor | Does | Does not |
|-------|------|----------|
| **CEO** | Open/record PRs; stack order; spawn waiters; restack; re-trigger **until cap**; stall residual; monitor; ship evidence | Auto-merge without human; infinite Copilot wait |
| **Worker** | Edit one branch; push fixes; reply Fixed | Spawn; merge; restack other branches |
| **Copilot waiter** | Poll **one** PR; fix that branch only; re-wait after own push; **exit stalled** when caps hit | Other stack branches; merge |
| **Director / Manager / Verifier** | Product plan/score | PR open, Copilot wait, merge |
| **Monitor** | Poll PR/CI; retarget base after lower merge | Product edits |

**One writer per branch.** Restack after lower fixes is **CEO** (or one multi-branch work order).

---

## Single PR

```text
create → wait Copilot → fix/reply → re-wait on new head → (re-trigger up to cap)
  → CI green + review-clean OR stalled residual → human merge → main CI
```

| Step | Rule |
|------|------|
| Wait | Poll ~30–60s, **hard cap ~12 min/head** (may run alongside CI, but do not exceed cap waiting only for Copilot). Sources: `pulls/{n}/comments` (`Copilot`), `pulls/{n}/reviews` (`copilot-pull-request-reviewer`). |
| Fix | Valid → commit + push + thread reply. Won't fix → reply with rationale. |
| Re-wait | **Every** push restarts wait on new SHA (same hard cap). |
| Re-trigger | No Copilot on new head after **CI complete** (or 12 min wait) → empty commit or PR comment; wait again. Cap **2** re-triggers/PR/session (3 total waits max). |
| **Stall exit** | See below — **required** so sessions do not hang. |
| Ready | CI green **and** (all known comments fixed/declined **or** stall residual logged on latest head). |

---

## Copilot stalled (do not get stuck)

Copilot frequently never returns after the first review, or after fix pushes. Treat that as **residual**, not a hard gate.

### Declare `COPILOT_STALLED` on a head when **any** is true

1. First wait hits **12 min** with **no** Copilot review and **no** new inline comments on that head.  
2. After addressing comments and push: re-wait hits 12 min **and** re-trigger budget is **exhausted** (max 2 re-triggers) with still no new Copilot activity.  
3. CI has been green for **≥15 min** on current head, all **known** Copilot threads are fixed/replied or declined, and no new Copilot events for **≥12 min** (bot went quiet).

### When stalled — **must** do all of:

1. **Stop** waiting/re-triggering on that PR head.  
2. Log craft-log + directive: `copilot: stalled` + head SHA + waits/re-triggers used.  
3. Post a short **PR issue comment** (not only chat):  
   `Copilot did not re-review head <sha> within budget (stalled). Known threads addressed. CI green — residual: optional human re-request.`  
4. Treat PR as **review-complete for STACK_READY purposes** if CI green and no **unaddressed** open threads (stalled != leave comments hanging).  
5. **Do not** open more empty commits “just in case.”

### Still block merge readiness when

- Required CI red.  
- **Open** Copilot/human review threads without fix **or** explicit decline reply.  
- Human required checks (CODEOWNERS, etc.) still pending.

### Do **not** block solely on

- “Copilot never left a second review on the fix head.”  
- “Re-trigger didn’t produce a new review.”

---

## Stacked PRs

Merge order = section/phase order. Linear history: `p1 ⊂ p2 ⊂ … ⊂ pN`.

| Field | Spec |
|-------|------|
| Bases | PR1 → `main`. PR*k* → PR*k-1* until lower merges, then retarget → `main`. |
| Review | Every open PR: wait/fix/re-wait **or** stall residual. Tip-only = process bug. |
| Lower fix | Lowest dirty first → rebase uppers → force-with-lease → re-wait **or stall** each changed head. |
| Concert | One waiter per open PR (worktree). |

### STACK_READY

1. Each stack PR **MERGED** or **OPEN+MERGEABLE**  
2. Each **OPEN**: required CI green  
3. Each **OPEN**: Copilot on **current** head is **fixed/declined** **or** **`copilot: stalled` residual logged** (not still polling)  
4. Bases/order correct  
5. Craft-log: PR#s, head SHAs, comment IDs, stall notes, CI URLs  

### Multi-hour babysit

| | Default |
|--|---------|
| Wall | **~8h** (or human budget) |
| Tool | `monitor` (stream) |
| Cadence | 60–90s hot; 3–5 min idle on human merge |
| Exit OK | `STACK_READY` or all merged + main CI green |
| Exit residual | wall timeout / human stop / CI red / open threads |

Copilot stall on individual PRs does **not** require waiting the full 8h.

---

## Ship evidence (minimum)

- Stack order + bases  
- Per PR: head SHA, CI, `copilot: received | re-triggered×N | stalled | timeout`  
- Fix SHAs + comment IDs  
- Stall PR comments posted  
- `STACK_READY` and/or main CI URL  

## Windows / gh

- `PR${n}` not `PR$n:` in PowerShell  
- Prefer `gh api --template` over fragile `--jq`  
- Force only with `--force-with-lease` after fetch  
