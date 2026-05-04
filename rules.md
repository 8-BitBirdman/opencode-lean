# Rules Reference

Complete documentation for every rule in `AGENTS.md` — what it does, why it exists, and examples of before/after.

---

## Rule 1: Code First, Prose Never

**What it does:** Forces the model to open with code, not explanation.

**Before:**
```
Great question! There are a few ways to approach this. The most common
pattern in Swift is to use a guard statement for early exit. Let me show
you how that works in practice...

guard let value = optional else { return }
```

**After:**
```swift
guard let value = optional else { return }
```

**Why:** The opener consumed 35 tokens. The answer was 9. The ratio was 4:1 wasted.

---

## Rule 2: Minimal Diffs Over Full Files

**What it does:** Prevents the model from reprinting entire files when only lines changed.

**Before** (editing one line in a 200-line file):
```swift
// [full 200-line file reprint]
```

**After:**
```swift
// ... existing code ...
func connect() {
    timeout = 30  // changed from 10
}
// ... existing code ...
```

**Why:** This is the single highest-leverage rule. A 200-line file reprint for a 1-line change wastes ~190 lines of output tokens — often 10–50× the actual answer.

---

## Rule 3: No Filler Content

**What it eliminates:**

| Category | Examples |
|---|---|
| Pleasantries | "Sure!", "Great question!", "Of course!" |
| Transition phrases | "Now let's look at…", "As you can see…" |
| Restatements | "You want to add a timeout, so here's how to add a timeout" |
| Sign-offs | "Let me know if you need anything else!", "Hope that helps!" |
| Boilerplate comments | `// increment i` above `i++` |

**Why:** These are pure noise. They add tokens, add latency, and train you to skim.

---

## Rule 4: Infer, Don't Ask

**What it does:** Stops the model from asking clarifying questions when a sensible default exists.

**Before:**
```
Before I write this, could you clarify:
1. Which Swift concurrency model do you prefer — async/await or Combine?
2. Should this be a class or a struct?
3. Do you want error handling?
```

**After:** The model picks `async/await`, `struct`, and includes basic error handling — the obvious modern defaults — and proceeds.

**Exception:** If the ambiguity would produce fundamentally different architecture (e.g. "should this be a local DB or a remote API?"), one clarifying question is allowed.

**Why:** Question-answer cycles are the most expensive token pattern. Each round-trip doubles the context.

---

## Rule 5: Single Responsibility Per Reply

**What it does:** Keeps each response scoped to what was actually asked.

**Before** (asked to fix a nil crash):
```swift
// Fixed your crash:
guard let x = optional else { return }

// Also refactored your networking layer for better separation of concerns:
// [40 lines of unrequested changes]

// And here are some tests you might want:
// [20 lines of unrequested tests]

// You might also consider switching to async/await...
```

**After:**
```swift
guard let x = optional else { return }
// Note: force-unwrap on line 84 in NetworkManager.swift
```

**Why:** Unsolicited changes are expensive twice — they cost output tokens to generate and input tokens to re-read in future turns.

---

## Rule 6: Format Compression

**What it does:** Prefers the shorter correct form when readability is equal.

| Verbose | Compressed |
|---|---|
| `let result = numbers.filter({ $0 > 0 })` | `let result = numbers.filter { $0 > 0 }` |
| `return value` in single-expr function | Implicit return |
| `import Foundation\nimport UIKit` in order | Shortest needed import only |

**Why:** Small savings per line compound across a codebase.

---

## Rule 7: Error Messages — Precise, Not Verbose

**What it does:** Caps error explanations at cause + fix, nothing more.

**Before:**
```
This error occurs because Swift's type system requires that all values
in a dictionary literal have the same type. In your case, you're mixing
Int and String values, which the compiler cannot reconcile at compile time.
To fix this, you should either...
```

**After:**
```
Dictionary values must be same type.
Fix: [String: Any] instead of [String: String]
```

**Why:** The cause and fix are what you need. Everything else is padding.

---

## The Test

> A response is good if removing any single token from it would make it wrong or incomplete.

This is the model's self-evaluation heuristic. It's stricter than "be concise" because it gives the model a concrete test to apply before responding.
