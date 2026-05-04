# Claude Code Compatibility

OpenCode supports Claude Code's file conventions as fallbacks.

## File Mapping

| OpenCode (preferred) | Claude Code (fallback) |
|---|---|
| `~/.config/opencode/AGENTS.md` | `~/.claude/CLAUDE.md` |
| `./AGENTS.md` (project root) | `./CLAUDE.md` (project root) |

**The first matching file wins.** If both exist, only the OpenCode version is used.

## If You're Coming from Claude Code

Your existing `~/.claude/CLAUDE.md` will work in OpenCode automatically — as long as you don't have a `~/.config/opencode/AGENTS.md`.

To migrate properly:

```bash
mkdir -p ~/.config/opencode

# Copy your Claude Code rules into OpenCode's location
cp ~/.claude/CLAUDE.md ~/.config/opencode/AGENTS.md

# Append the token-saver rules from this project
cat >> ~/.config/opencode/AGENTS.md << 'EOF'

---

# Token Economy Rules

## Code First, Prose Never
...
EOF
```

## If You Use Both Tools

Keep both files. They're separate tools with separate configs and don't interfere.

```
~/.claude/CLAUDE.md              ← Claude Code reads this
~/.config/opencode/AGENTS.md    ← OpenCode reads this
```

You can keep them in sync manually, or symlink one to the other if your rules are identical:

```bash
ln -sf ~/.config/opencode/AGENTS.md ~/.claude/CLAUDE.md
```
