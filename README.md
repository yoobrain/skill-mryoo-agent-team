# Mr Yoo Agent Team

Three [Claude Code custom slash commands](https://docs.claude.com/en/docs/claude-code/slash-commands) that turn Claude Code into a small multi-agent dev team — scan for memory leaks, then route to a lightweight or full-scale fix pipeline depending on scope.

> **Claude Code only.** These are slash commands (`~/.claude/commands/*.md`), not the cross-agent `SKILL.md` Agent Skills format — they will not load in Grok Build or other Agent-Skills-compatible tools.

## The team

| Command | Role | When to use |
|---|---|---|
| `/mryoo-check-memory-leak` | **Mr Yoo — Scanner** | Suspected memory leak, OOM, app grows over time, long-running Python app. Scans, classifies (HIGH/MEDIUM/LOW), and routes to a fix command — never fixes anything itself. |
| `/mryoo-team-lite <task>` | **Mr Yoo Team Lite** (3 agents: G → Andy → Bobby) | Bug fix, small feature, single-file change, refactor. Sequential: Plan → Code → Review. |
| `/mryoo-team-expert <task>` | **Mr Yoo Team Expert** (8 agents) | New phase, multi-module feature, large refactor, project init. Staged: Architect → Parallel Coders → Parallel Review → Optimize → Integration test. |

### Suggested flow

```
Suspect a leak? ──► /mryoo-check-memory-leak
                          │
              ┌───────────┼───────────────┐
        NO FIX NEEDED  1–2 files      3+ files /
        or LOW (manual) MEDIUM/HIGH   architectural change
                          │                │
                 /mryoo-team-lite   /mryoo-team-expert
```

For a task you already know the shape of, skip the scanner and call `/mryoo-team-lite` or `/mryoo-team-expert` directly — the scanner is only there to size *memory-leak* work.

## Install

Copy the command files into your Claude Code commands directory:

```powershell
Copy-Item commands\*.md "$env:USERPROFILE\.claude\commands\"
```

Or clone directly:

```powershell
git clone https://github.com/yoobrain/skill-mryoo-agent-team.git
Copy-Item skill-mryoo-agent-team\commands\*.md "$env:USERPROFILE\.claude\commands\"
```

Restart Claude Code (or run `/doctor`) so it picks up the new commands.

## Notes

- Every fix command **delegates entirely** to subagents via the `Agent` tool — the orchestrator never edits code itself.
- `/mryoo-team-lite` and `/mryoo-team-expert` both read the project's `CLAUDE.md` first and pass its conventions into every subagent prompt, so they respect existing project rules.
- `/mryoo-team-expert`'s Stage 3b fix loop caps at 2 iterations — if HIGH-severity issues persist after that, it reports a blocker instead of looping forever.

## License

MIT — see [LICENSE](LICENSE).
