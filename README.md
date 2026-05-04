<div align="center">

```
╔═══════════════════════════════════════╗
║   opencode-lean                       ║
║   say less. ship more.                ║
╚═══════════════════════════════════════╝
```

**Global AI rules for [OpenCode](https://opencode.ai) that ruthlessly eliminate wasted tokens.**  
One install. Every session. Zero configuration per project.

[![License: MIT](https://img.shields.io/badge/License-MIT-black?style=flat-square)](LICENSE)
[![Compatible](https://img.shields.io/badge/OpenCode-compatible-black?style=flat-square)](https://opencode.ai)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-black?style=flat-square)](CONTRIBUTING.md)

</div>

---

## The Problem

Every AI coding session burns tokens on things that aren't code:

```
Sure! Here's how you can solve that problem. First, let me explain
the approach I'm going to take. I'll start by looking at the existing
code and then make the necessary changes. Let me walk you through it...

[47 words later]

const x = 1
```

Multiply that by hundreds of sessions. That's real money, real latency, real noise.

## The Solution

A single `AGENTS.md` file dropped in `~/.config/opencode/` that tells OpenCode exactly how to behave — globally, permanently, with zero per-project setup.

```
[before]  ~900 tokens for a 3-line fix
[after]   ~180 tokens for the same fix
```

---

## Install

**60 seconds. Copy, paste, done.**

### Step 1 — Write the rules

```bash
mkdir -p ~/.config/opencode

cat > ~/.config/opencode/AGENTS.md << 'EOF'
# Response Rules — Token Economy

## Code First, Prose Never
- Return code immediately. No preamble, no closing summary.
- If explanation is needed, put it inside code comments, not outside.
- Never restate the question or describe what you are about to do.

## Minimal Diffs Over Full Files
- When editing existing code, return only the changed lines plus max 3 lines of surrounding context.
- Never reprint an entire file when a diff or partial snippet suffices.
- Use `// ... existing code ...` to skip unchanged sections.

## No Filler Content
Strip unconditionally: pleasantries, transition phrases, redundant type
annotations, boilerplate comments that restate the code, closing remarks.

## Infer, Don't Ask
- If ambiguous but has an obvious best-practice default, use it and move on.
- Only ask a clarifying question when ambiguity would produce fundamentally different code.
- Limit clarifying questions to one per response, never a list.

## Single Responsibility Per Reply
- Answer exactly what was asked. Do not proactively refactor adjacent code,
  add unrequested tests, or suggest alternatives unless strictly shorter or safer.
- If you spot a separate bug, note it in one line: `// Note: <issue> in <location>`

## Format Compression
- Prefer inline expressions over multi-line blocks when readability is equal.
- Use the shortest correct import path available.

## Error Messages
- Cause in ≤10 words + fix in ≤1 code line.

## The Test
A response is good if removing any single token from it would make it wrong or incomplete.
EOF
```

### Step 2 — Wire it into OpenCode

```bash
cat > ~/.config/opencode/opencode.json << 'EOF'
{
  "$schema": "https://opencode.ai/config.json",
  "instructions": ["~/.config/opencode/AGENTS.md"]
}
EOF
```

### Step 3 — Verify

```bash
cat ~/.config/opencode/AGENTS.md   # should print the rules
cat ~/.config/opencode/opencode.json   # should print the json
```

Open OpenCode. You're done.

---

## How It Works

OpenCode loads `~/.config/opencode/AGENTS.md` automatically at startup and injects it into the model's context for every session. No flags. No presets. No per-project files.

```
~/.config/opencode/
├── AGENTS.md          ← your global rules (this project)
└── opencode.json      ← tells OpenCode to load them
```

Project-level `AGENTS.md` files still work and layer on top — global rules are the floor, not a cage.

See [how OpenCode loads rules →](docs/how-it-works.md)

---

## Rules Reference

| Rule | What it eliminates |
|---|---|
| **Code First** | Intro paragraphs, "Here's how…" openers |
| **Minimal Diffs** | Full file reprints for single-line edits |
| **No Filler** | Pleasantries, restatements, sign-offs |
| **Infer, Don't Ask** | Clarifying question chains |
| **Single Responsibility** | Unsolicited refactors, unrequested tests |
| **Format Compression** | Verbose multi-line expressions |
| **Error Messages** | Wall-of-text error explanations |

Full rule documentation → [docs/rules.md](docs/rules.md)

---

## Customizing

The rules are plain Markdown. Edit `~/.config/opencode/AGENTS.md` directly.

Want to keep your changes version-controlled?

```bash
# Keep your dotfiles in git, symlink from there
ln -sf ~/dotfiles/opencode/AGENTS.md ~/.config/opencode/AGENTS.md
```

See [docs/customizing.md](docs/customizing.md) for a full guide including per-project overrides and team sharing.

---

## FAQ

**Does this affect which model I use?**  
No. The rules work with any model OpenCode supports — Claude, GPT, Gemini, local models.

**Will it break existing project AGENTS.md files?**  
No. Project-level rules layer on top of global ones. They don't conflict.

**Can I share this with my team?**  
Yes, but the global file approach is personal-machine only. For team-wide rules, commit an `AGENTS.md` to the project root. See [docs/team-setup.md](docs/team-setup.md).

**What if I want verbose output for a specific session?**  
Add a project-level `AGENTS.md` that overrides specific rules, or just tell the model in-session. Global rules are defaults, not locks.

**Does this work with Claude Code too?**  
Partially — OpenCode falls back to `~/.claude/CLAUDE.md` if no `~/.config/opencode/AGENTS.md` exists. See [docs/claude-code-compat.md](docs/claude-code-compat.md).

---

## Contributing

Spotted a rule that could be tighter? Found a filler pattern not covered?  
PRs are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

MIT — see [LICENSE](LICENSE).

---

<div align="center">
<sub>Built for developers who'd rather read code than prose.</sub>
</div>
