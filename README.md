# Grok CEO skill suite

Public operator pack for **multi-step delivery with Grok**: CEO orchestrator, org pipeline, and role agents (Director, Manager, Worker, Verifier).

This repository is a **published export** from the private Waryway monorepo. It is intended for install under your local Grok config (`~/.grok/`), not as a runtime dependency of Waryway.com.

## Install

Copy (or symlink) into your Grok home:

```bash
# From a clone of this repo
export GROK_HOME="${GROK_HOME:-$HOME/.grok}"
mkdir -p "$GROK_HOME/skills" "$GROK_HOME/agents"
cp -R skills/* "$GROK_HOME/skills/"
cp -R agents/* "$GROK_HOME/agents/"
```

**Windows (PowerShell):**

```powershell
$GrokHome = if ($env:GROK_HOME) { $env:GROK_HOME } else { Join-Path $HOME ".grok" }
New-Item -ItemType Directory -Force -Path "$GrokHome\skills","$GrokHome\agents" | Out-Null
Copy-Item -Recurse -Force .\skills\* "$GrokHome\skills\"
Copy-Item -Recurse -Force .\agents\* "$GrokHome\agents\"
```

Then in Grok Build:

```text
/ceo <your multi-step goal>
```

## Layout

```
skills/
  ceo/                 # /ceo slash skill + templates
    references/
      pr-mr-loop.md    # single PR + stacked PR babysit (Copilot re-wait, ~8h)
      spawn-prompts-compact.md
      …
  org-pipeline/        # dispatch, rubrics, token efficiency
  director/
  manager/
  worker/
agents/
  ceo.md               # SoT for /ceo (PR stack rules + ship gate)
  director.md
  manager.md
  worker.md
  verifier.md
```

## PR / stack delivery (summary)

- After every PR **create** and **push**: wait for async Copilot; fix/reply; **re-wait on new head** (re-trigger ≤2 if silent).
- If Copilot **stops re-reviewing**: declare **`COPILOT_STALLED`**, log residual, PR comment, **stop polling** — do not hang the stack.
- **Stacks:** every PR clean **or** stalled residual (not tip-only); restack; retarget; babysit ≤~8h → `STACK_READY` / main CI.
- Under CEO, waiters = **`worker`+worktree**, never `general-purpose`.
- Detail: `skills/ceo/references/pr-mr-loop.md` § Copilot stalled.

## Docs

- Operator guide (product): https://guide.waryway.com/guides/grok-ceo-org/
- Source of truth for publish: private monorepo `publish/grok-ceo-skill/` (exported by CI)

## License

Content is provided as-is for operators using Grok with Waryway-style org delivery. See LICENSE if present.
