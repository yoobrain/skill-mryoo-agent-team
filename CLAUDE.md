# CLAUDE.md — skill-mryoo-agent-team

Guidance for any agent (Claude, Grok, etc.) working in this repo.

## What this is

A public rebrand of the user's personal Claude Code slash commands (multi-agent dev workflow: memory-leak scanner → routes to a 3-agent or 8-agent fix pipeline). **Not** the Agent Skills / `SKILL.md` format — these are Claude Code custom slash commands (`~/.claude/commands/*.md`), Claude Code only. Grok Build will not load them.

Repo: https://github.com/yoobrain/skill-mryoo-agent-team (public)

## IMPORTANT: this repo is a renamed mirror, not the source of truth

The user's **live, daily-use** commands are separate files and use the **original** names — do not confuse the two or assume they're the same file:

| Live command (daily use, unchanged) | Published repo (this repo, renamed) |
|---|---|
| `C:\Users\YooRich\.claude\commands\mrm-check-memory-leak.md` (`/mrm`) | `commands/mryoo-check-memory-leak.md` |
| `C:\Users\YooRich\.claude\commands\mrg-team-lite.md` (`/mrg-team-lite`) | `commands/mryoo-team-lite.md` |
| `C:\Users\YooRich\.claude\commands\mrg-team-expert.md` (`/mrg-team-expert`) | `commands/mryoo-team-expert.md` |

The user explicitly chose to **keep the live `/mrm`, `/mrg-team-lite`, `/mrg-team-expert` triggers as-is** (muscle memory) and only rebrand the public copy to `mryoo-*` / persona "Mr Yoo" (replacing "Mr M" / "MR G") for brand consistency with their `yoobrain` identity. There is no automated sync — if the live commands change, this repo goes stale until someone manually re-applies the diff (with the name substitutions) here.

## Rules

- **Format**: Claude Code slash command frontmatter (`description`, `user-invocable: true`) + body using `$ARGUMENTS`. This is not the same spec as `SKILL.md` Agent Skills — don't convert it without the user's explicit request, and say so clearly in the README if compatibility is ever asked about.
- **Branding**: prefix is `mryoo-`, orchestrator persona is "Mr Yoo" (was "Mr M" for the scanner, "MR G" for the team orchestrators). Sub-agent names inside the pipelines (G, Andy, Bobby, Architect, Coder-A/B, Syntax Auditor, Logic Reviewer, Perf Optimizer, Integration Tester) are unrelated codenames and were **not** renamed — leave them as-is unless told otherwise.
- **YAML frontmatter**: `description:` must be a folded block scalar (`description: >` + indented body), never a plain single-line scalar. A plain scalar containing `": "` mid-string breaks YAML parsing ("mapping values are not allowed in this context") — this bit us once already (see below). Any new command added here must follow this pattern from the start.
- **Git identity for this repo is local, not global.** Set per-repo to `yoobrain <122417558+yoobrain@users.noreply.github.com>` — never the user's real name/email. Verify with `git log --format='%an <%ae>'` before pushing.
- **License**: MIT, copyright held under the GitHub handle `yoobrain`, not the user's real name.
- **No personal info in content** — this repo had none in file bodies to begin with (only the git commit metadata leaked; see below).

## What's been done (chronological)

1. Read the 3 live commands, confirmed no personal info in the file bodies.
2. Bundled all 3 into **one repo** (not 3 separate repos) because `/mrm-check-memory-leak` routes into the other two — they're one interlinked system.
3. Renamed `mrm`/`mrg` → `mryoo` in filenames, frontmatter triggers, and self-referential persona text ("Mr M" → "Mr Yoo", "MR G" → "Mr Yoo") — **published copy only**, live commands untouched (see table above).
4. Created `README.md` (explicitly notes Claude Code-only, not Grok-compatible), `LICENSE` (MIT), `.gitignore`.
5. Published as a **public** GitHub repo via `gh repo create`, added topics (`claude-code`, `slash-commands`, `multi-agent`, `ai-agents`, `memory-leak`, `code-review`, `developer-tools`).
6. **Found and fixed a PII leak**: git commit author metadata carried the user's real name + personal Gmail (inherited from global git config). Fixed by setting repo-local git identity to `yoobrain` + GitHub noreply email, rewriting all commit history (`git filter-branch --env-filter`), and force-pushing. Same issue was found and fixed in `skill-latest-screenshot` — check `git log --format='%an <%ae>'` on any newly-created repo before it's pushed publicly, going forward.
7. **Found and fixed a YAML bug**: `mryoo-team-expert.md` and `mryoo-team-lite.md` (plus the still-live `mrg-team-expert.md` / `mrg-team-lite.md`) had single-line, unquoted `description:` values containing `"...pipeline: Architect..."` — the mid-string `: ` broke YAML frontmatter parsing ("mapping values are not allowed in this context", reproduced at line 1 col ~99-100). Fixed by switching to folded scalars (`description: >`). `mryoo-check-memory-leak.md` (and the live `mrm-check-memory-leak.md`) were already using folded style and were never affected.

## If extending this repo

- New commands added here must use `description: >` (folded), not a plain one-line `description:`.
- Keep the "Claude Code only" disclaimer in the README accurate — don't quietly add SKILL.md-style Grok support without discussing format implications with the user first.
- If the user asks to sync a change from the live commands, apply the same `mrg`/`mrm` → `mryoo` and "Mr M"/"MR G" → "Mr Yoo" substitutions used originally (see table above) rather than copying verbatim.
