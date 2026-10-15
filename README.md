# index-plan

A [Claude Code](https://claude.com/claude-code) skill that saves each plan as its own
artifact page and keeps one index page per project listing every plan, newest first.

- Every plan becomes a **new** page; earlier plans are never overwritten.
- The index marks the current plan and which version each older one was superseded by.
- Page sources and a small registry are kept in `~/.claude/plan-artifacts/<project>/`,
  so nothing is lost when a session ends.

## Install

Copy the `index-plan` folder into your personal skills folder:

- macOS / Linux: `~/.claude/skills/index-plan/`
- Windows: `%USERPROFILE%\.claude\skills\index-plan\`

Or into a single project: `<your-project>/.claude/skills/index-plan/`.

## Use

After Claude writes a plan, type `/index-plan`, or ask Claude to
"save the plan as an artifact and add it to the index".

Requires a Claude Code setup where the Artifact tool is available.

## License

MIT — see [LICENSE](LICENSE).
