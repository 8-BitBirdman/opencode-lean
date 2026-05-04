# Contributing

Thanks for wanting to make this better. Contributions are welcome.

## What's Worth Contributing

- **Tighter rules** — if an existing rule is wordy or could be expressed more precisely, open a PR
- **New filler patterns** — common AI verbosity patterns not covered by the current rules
- **Use-case examples** — before/after token comparisons that demonstrate a rule working (or not)
- **Compatibility notes** — how these rules interact with specific models or OpenCode versions
- **Bug reports** — rules that produce bad output, cause the model to misbehave, or conflict with each other

## What Doesn't Fit

- Language-specific rules — those belong in your personal `AGENTS.md`, not this project
- Model-specific tuning — the rules should work across all models OpenCode supports
- Opinionated style preferences — "always use tabs" is a you thing, not a this-project thing

## Process

1. Fork the repo
2. Make your change
3. Open a PR with a clear title and a before/after example if applicable

No formal issue required for small changes. For bigger rewrites, open an issue first to discuss.

## Rule Writing Guidelines

Good rules are:

- **Specific** — "return only changed lines + max 3 context lines" not "be concise"
- **Verifiable** — you can tell by reading a response whether the rule was followed
- **Model-agnostic** — they should work with Claude, GPT, Gemini, and local models
- **Additive** — they should make responses better, not just shorter

Rules that trade correctness for token savings are wrong. The goal is eliminating waste, not information.
