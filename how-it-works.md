# How It Works

## OpenCode's Rule Loading System

When OpenCode starts, it looks for instruction files in a specific order and merges them all into the model's system context. Understanding this lets you layer rules precisely.

### Load Order (lowest → highest priority)

```
1. Remote org config     (.well-known/opencode)
2. Global config         (~/.config/opencode/opencode.json)
3. Global AGENTS.md      (~/.config/opencode/AGENTS.md)      ← this project
4. Project AGENTS.md     (./AGENTS.md in your repo)
5. OPENCODE_CONFIG env   (custom path override)
6. Project opencode.json (./opencode.json in your repo)
```

Later entries win on conflicts. Non-conflicting rules from all levels are preserved.

### What Gets Injected

Every file listed in `instructions` inside `opencode.json`, plus any `AGENTS.md` found at the global or project level, gets concatenated and injected into the model's context at the start of each session.

This means the model "knows" your rules before you type a single character.

### The Global Files

```
~/.config/opencode/
├── AGENTS.md          ← plain Markdown rules, always loaded
└── opencode.json      ← config, points to AGENTS.md + any other settings
```

`AGENTS.md` is loaded automatically by OpenCode without needing to be listed in `opencode.json`. The `instructions` field in `opencode.json` is for *additional* files you want to include.

### Project Rules Layer On Top

If a project has its own `AGENTS.md`, it adds to your global rules — it doesn't replace them. You can use this to:

- Add project-specific conventions (naming, framework patterns)
- Override a global rule for one repo (e.g. allow verbose output for a docs project)
- Share team-wide rules via Git

```
~/.config/opencode/AGENTS.md     ← your personal token-saver rules
~/projects/myapp/AGENTS.md       ← myapp-specific rules on top
```

### Claude Code Compatibility

OpenCode falls back to Claude Code's file conventions if its own files aren't found:

| OpenCode looks for | Falls back to |
|---|---|
| `~/.config/opencode/AGENTS.md` | `~/.claude/CLAUDE.md` |
| `./AGENTS.md` in project | `./CLAUDE.md` in project |

The first matching file wins — if you have both, only `AGENTS.md` is used.
