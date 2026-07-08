# Working Guidelines

Rules for running AI coding agents. Every entry here clears one of two bars:

- **Sourced.** The claim carries a link where you can confirm it yourself.
- **Mechanism.** The claim follows from how the tools work, and the entry states the mechanism so you can check the reasoning without trusting anyone.

Nothing on this page asks you to take our word for it.

---

### 1. Run one task per session

Every message you send re-sends the whole conversation to the model as input, so a long chat re-bills its own history on every turn and costs more the longer it runs. When the history grows past the point of compaction, the model summarizes its own past, and a summary drops detail by definition. Finish one task, close the session, start the next one clean.

Basis: mechanism. Chat models are stateless between calls; the full history goes back in every time.

### 2. Keep context in notes, not in the thread

A conversation vanishes when the session ends; a file on disk does not. Keep a short notes file (current task, decisions made, what shipped, what is next) that every new session reads at boot. Then you can close threads freely: the notes carry the context forward at the cost of one small file read instead of a growing, re-billed history.

Basis: mechanism.

### 3. Isolate parallel agents at the file level

Subagents run in the background by default as of Claude Code v2.1.198, so parallel work is now the normal case, not the exception. Two agents writing the same file silently overwrite each other: the file has one final state and the last write wins. Give each parallel task its own checkout (a git worktree or a separate clone), and watch the shared files hardest, because configs, docs, and lockfiles are the files every task touches.

Source: Claude Code changelog, v2.1.198. https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md

### 4. Verify the served result, not the report

The file an agent edited and the file your server serves are two different objects until you read the served one. Deploys fail partway, caches hold old bytes, builds ship stale artifacts, and none of that appears in the agent's "done" message, which is generated text, not a measurement. Before anything counts as done, exercise the real thing: load the page, hit the endpoint, run the program.

Basis: mechanism.

### 5. Use model choice as the main cost lever

On current Claude API pricing, Haiku 4.5 runs $1 per million input tokens and $5 per million output, Sonnet 5 runs $2/$10 (introductory, through August 31, 2026; $3/$15 after), and Opus-class models start at $5/$25. Same tokens, up to an order of magnitude apart in price. Route routine work to the cheaper model and escalate only for a named reason: real design judgment, a wide blast radius, or failure modes that are hard to catch.

Source: Claude API pricing. https://platform.claude.com/docs/en/about-claude/pricing

### 6. Redo the arithmetic when your model changes

Claude Sonnet 5 launched on June 30, 2026 with a new tokenizer that produces roughly 30 percent more tokens for the same text, and introductory pricing that ends on August 31, 2026. Any session budget or cost estimate tuned before a model switch undercounts after it. When the model under your workflow changes, recompute what a session costs, and put hard dates like that August 31 pricing change on a calendar.

Sources: https://www.anthropic.com/news/claude-sonnet-5 and https://platform.claude.com/docs/en/release-notes/overview

### 7. Give model routing a fallback

A model being available is a fact about today, not a guarantee. On June 13, 2026, a US government export-control directive suspended API access to Claude Fable 5 and Claude Mythos 5; access was restored on July 1, eighteen days later. A workflow hard-coded to one model stops when that model does. Configure a next-best fallback so work degrades instead of halting.

Sources: https://simonwillison.net/2026/Jun/13/us-government-directive-to-suspend-access/ and https://platform.claude.com/docs/en/release-notes/overview (July 1, 2026 entry)

### 8. Point automation at the metered API, not a subscription login

In April 2026, Anthropic blocked Claude subscription accounts from spending their limits through third-party harnesses, and pointed affected users to API pricing as the continued channel. Consumer subscription terms are the vendor's to change on notice; the metered API is the channel built and priced for programmatic use. If your automation rides a subscription login, it rides on terms that have already moved once.

Source: https://news.ycombinator.com/item?id=47633396

### 9. Ask before anything irreversible

An action with no undo has no recovery path, by definition: deleted files with no backup, rewritten git history, dropped tables, sent messages, spent money. Asking first costs one message; guessing wrong costs whatever was destroyed. Let agents act freely on anything version control can revert, and require confirmation for anything it cannot.

Basis: mechanism.

### 10. Keep auto-loaded instruction files lean

A file like CLAUDE.md is read into the model at every session start, so every line in it is paid for on every boot, used or not. Keep the always-loaded file down to what steers current work, and move settled history and old decisions into files that are read on demand. Every line in the boot file should earn its seat.

Basis: mechanism.

### 11. Prune your notes on a schedule

A notes file only grows; nothing removes a stale rule except you. Whatever the file contains gets read and followed at every boot, current or not, so an outdated instruction keeps steering new work until someone deletes it. Put consolidation on a schedule: merge duplicates, delete dead rules, and date what remains.

Basis: mechanism.
