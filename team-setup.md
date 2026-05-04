# Team Setup

## Personal vs. Shared Rules

| File | Who sees it | In Git? | Use for |
|---|---|---|---|
| `~/.config/opencode/AGENTS.md` | You only | No | Personal token-saver rules |
| `./AGENTS.md` (project root) | Everyone | Yes | Project conventions, team defaults |

Both are loaded and merged. Use both.

---

## Setting Up Team Rules

### 1. Create a project AGENTS.md

```bash
cat > ./AGENTS.md << 'EOF'
# [ProjectName] AI Rules

## Architecture
- This is a SwiftUI app using MVVM. Never suggest UIKit patterns.
- Networking is done via async/await + URLSession. No Alamofire.
- Database is SQLite via GRDB. No CoreData suggestions.

## Code Style
- Follow existing naming conventions in the codebase before inventing new ones.
- All new public APIs need a one-line doc comment.

## What Not to Touch
- Do not suggest changes to the build system without being asked.
- Do not add dependencies without explicit approval.
EOF
```

### 2. Commit it

```bash
git add AGENTS.md
git commit -m "Add OpenCode AI rules for the team"
```

Everyone who clones the repo gets these rules automatically on their next OpenCode session.

---

## Recommended Split

**In `~/.config/opencode/AGENTS.md` (personal, not shared):**
- Token economy rules (this project)
- Personal style preferences
- Your preferred defaults for ambiguous situations

**In `./AGENTS.md` (project, committed to Git):**
- Architecture decisions ("we use X, not Y")
- Framework constraints
- What files/systems are off-limits
- Team naming conventions

---

## Org-Wide Rules (Advanced)

OpenCode supports a remote config loaded from `.well-known/opencode` on a domain. This lets an organization push rules to all developers without any local setup.

This is outside the scope of this project — see the [OpenCode docs](https://opencode.ai/docs/config) for details.
