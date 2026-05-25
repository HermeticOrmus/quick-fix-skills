<p align="center">
  <img src="https://ormus.solutions/mascot/pixellab_liquid_to_spiral.gif" alt="Quick Fix Skills" width="128" style="image-rendering: pixelated;" />
</p>

<h1 align="center">Quick Fix Skills</h1>

<p align="center">
  <em>A Claude Code skill for fast troubleshooting — a lightweight diagnostic pass for common issues when the full debug protocol is overkill.</em>
</p>

<p align="center">
  <a href="https://github.com/HermeticOrmus/quick-fix-skills/stargazers"><img src="https://img.shields.io/github/stars/HermeticOrmus/quick-fix-skills?style=flat-square&color=aa8142" alt="Stars" /></a>
  <a href="https://github.com/HermeticOrmus/quick-fix-skills/blob/main/LICENSE"><img src="https://img.shields.io/github/license/HermeticOrmus/quick-fix-skills?style=flat-square&color=aa8142" alt="License" /></a>
  <a href="https://github.com/HermeticOrmus/quick-fix-skills/commits"><img src="https://img.shields.io/github/last-commit/HermeticOrmus/quick-fix-skills?style=flat-square&color=aa8142" alt="Last Commit" /></a>
  <img src="https://img.shields.io/badge/Claude_Code-aa8142?style=flat-square&logo=anthropic&logoColor=white" alt="Claude Code" />
</p>

---

A single triage file for the routine breakage that does not need an investigation. Drop it in, and Claude runs a fast diagnostic pass instead of a full debugging session.

## The problem

Most issues are common and obvious. A version mismatch. A typo in a config key. An unset environment variable. A stale cache. A port already in use. These do not need a six-phase root-cause protocol; they need someone to ask the two cheap questions first and reach for the usual suspect.

The cost of skipping triage runs the other way too. Reaching for a heavyweight protocol on a one-line typo wastes time and over-engineers the fix. Quick-fix is the fast pass that resolves the routine cases and, when it fails, hands off cleanly to the rigorous one.

## The quick-diagnostic flow

| Step | Question |
|---|---|
| **What is broken** | Pin it to one sentence. Get the exact error text. |
| **What changed** | Did it ever work? What changed since? Check the diff. |
| **Common causes** | Match the symptom to the usual suspect for this class. |
| **The smallest fix** | Change the least possible. Re-run against the repro. |
| **When to stop** | Two or three failed fixes, or a novel cause: escalate. |

Full content: [`CLAUDE.md`](CLAUDE.md). Worked examples: [`EXAMPLES.md`](EXAMPLES.md).

## Install

### As a project CLAUDE.md

Drop [`CLAUDE.md`](CLAUDE.md) at the root of your repository. Claude Code picks it up automatically. Merge with existing project instructions if any.

```bash
curl -o CLAUDE.md https://raw.githubusercontent.com/HermeticOrmus/quick-fix-skills/main/CLAUDE.md
```

### As a Claude Code skill

The same content is packaged as a skill under [`skills/quick-fix/`](skills/quick-fix/) for `~/.claude/skills/`. See the `SKILL.md` inside for installation.

### As a `/quick-fix` slash command

Save [`CLAUDE.md`](CLAUDE.md) as a command file so you can invoke the triage on demand.

```bash
mkdir -p ~/.claude/commands
curl -o ~/.claude/commands/quick-fix.md https://raw.githubusercontent.com/HermeticOrmus/quick-fix-skills/main/CLAUDE.md
```

Then run `/quick-fix` in any Claude Code session to start the diagnostic pass.

### In Cursor

See [`CURSOR.md`](CURSOR.md) for the Cursor-rule equivalent at [`.cursor/rules/quick-fix.mdc`](.cursor/rules/quick-fix.mdc).

### In other AI coding tools

If your tool reads a single instruction file at the project root, copy `CLAUDE.md` to whatever name your tool expects (`AGENTS.md`, `INSTRUCTIONS.md`, etc.).

## See also

- [`hypothesis-debugging-skills`](https://github.com/HermeticOrmus/hypothesis-debugging-skills): the escalation path. Quick-fix is the fast first pass for common, obvious issues; hypothesis-debugging is the rigorous six-phase root-cause protocol for everything quick-fix cannot resolve. When two or three quick fixes fail, or the cause is novel or intermittent, escalate there.

## Contributing

PRs welcome, especially for additional worked examples in [`EXAMPLES.md`](EXAMPLES.md), more common-cause classes, translations of the README, and adaptations of `CURSOR.md` for other AI coding tools (Windsurf, Cline, Aider, Continue, etc.).

## License

MIT. Use it, fork it, merge it into your own CLAUDE.md.
