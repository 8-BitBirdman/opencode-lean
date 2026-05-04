# Customizing

## Editing Your Rules

The rules file is plain Markdown at `~/.config/opencode/AGENTS.md`. Open it in any editor:

```bash
nano ~/.config/opencode/AGENTS.md
# or
code ~/.config/opencode/AGENTS.md
```

Changes take effect the next time you start an OpenCode session.

---

## Keeping Rules in Version Control

If you manage dotfiles in Git, symlink instead of copying:

```bash
# In your dotfiles repo
mkdir -p ~/dotfiles/opencode
mv ~/.config/opencode/AGENTS.md ~/dotfiles/opencode/AGENTS.md
ln -sf ~/dotfiles/opencode/AGENTS.md ~/.config/opencode/AGENTS.md
```

Now your rules travel with your dotfiles across machines.

---

## Adding Rules for Specific Languages

Append language-specific sections to the bottom of your `AGENTS.md`:

```markdown
## Swift
- Prefer `guard` over nested `if let`
- Use `async/await` over Combine for new code
- Mark all `@MainActor` requirements explicitly

## TypeScript
- No `any` unless casting from an external API response
- Prefer `type` over `interface` for unions
- Always use strict null checks
```

---

## Per-Project Overrides

Create an `AGENTS.md` in your project root to add or override rules for that repo only:

```bash
cat > ./AGENTS.md << 'EOF'
# Project: MyApp

## Style
- Use 4-space indentation (not 2)
- All public functions need doc comments

## Override: Verbose Output Allowed
This is a documentation project. Full explanations are preferred over terse code.
EOF
```

Global rules + project rules are merged. If you write a rule with the same heading, the project version takes precedence.

---

## Disabling a Global Rule for a Project

You can't "delete" a global rule from a project override, but you can explicitly counter it:

```markdown
## Override: Diffs Not Required
Return full file contents when editing — this project uses file-level review tools.
```

The model will weight the more specific (project-level) instruction higher.

---

## Sharing with a Team

The global `~/.config/opencode/AGENTS.md` is personal — it lives outside Git. For team-wide rules:

1. Create `AGENTS.md` in your project root
2. Commit it to Git
3. Everyone who clones the repo gets the rules automatically

See [team-setup.md](team-setup.md) for a full team workflow.
