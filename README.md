# VibeMind Brain (Starter)

A starter brain for Claude Code. Curated working rules your agent reads at session start, so it boots already knowing how to behave.

Claude Code reads a `CLAUDE.md` file when a session starts. Most people leave it empty or fill it with project trivia. This repo gives you something better to put there: a compact block of working guidance that makes a bare session noticeably sharper. Your agent boots with a front desk that greets you and catches you up, keeps notes across sessions so context survives, verifies its own work before calling it done, and asks before it does anything destructive.

Five minutes. No account, no installer, nothing to trust but a repo you can read.

## Quickstart

Three steps, all literal:

1. Get the file.

   ```
   git clone https://github.com/therealevanc/vibemind-brain.git
   ```

   Or skip the clone and copy [`CLAUDE-STARTER.md`](CLAUDE-STARTER.md) straight from GitHub.

2. Copy everything below the horizontal rule in `CLAUDE-STARTER.md` into your project's `CLAUDE.md` (create the file at your project root if it does not exist).

3. Open Claude Code in that project. The next session boots with the brain loaded: banner, catch-up, and a question about what you are building.

## What you get

**Thalamus, the front desk.** A named character who runs a small boot ritual at the start of every session: reads the notes, prints a status banner, gives you a two-line "since last time" catch-up, and asks what you are building. At the end of a session it closes the books: decisions, shipped work, and open questions go back into the notes before goodbye.

**External memory that survives sessions.** A plain `NOTES.md` at your project root holds the current task, what shipped, open questions, and what is next. Sessions read it at boot and sync it at close, so you can end conversations freely and lose nothing.

**A status board on demand.** Type `board` and the session renders a compact ASCII summary: current task, what changed since last session, what finished in this one, open questions, next up. No dashboard, no service, just your notes read back in one clean block.

**Working guidelines you can check.** Every entry in `GUIDELINES.md` clears one of two bars: it is sourced, with a link where you can confirm the claim yourself, or it states a mechanism that follows from how the tools work. They cover the levers that actually move cost and quality: session length, model choice, parallel-agent isolation, and verifying the served result instead of the agent's report.

## The sources we watch

The sourced guidelines trace to real, dated changes. We watch these feeds continuously:

- **Claude Code changelog** on GitHub, for new defaults, new tools, and changed agent behavior.
- **Claude platform release notes**, for model launches, API changes, and pricing.
- **anthropic.com/news**, for official announcements.
- **Simon Willison's blog**, for independent analysis of Anthropic releases.
- **Hacker News**, where policy changes surface and get stress-tested in public.
- **r/ClaudeAI**, for day-to-day reports from people running agents.

Full list with links is in `SOURCES.md`.

## This is the free slice

This starter brain is the free part of VibeMind. The full product is an app that runs the whole workflow: a live brain that greets you, dispatches building work in the background, tracks it on a board, and keeps its own memory in order. The full membership brain updates continuously as the sources above change, so the guidance your agent reads stays current without you maintaining it.

The app lives at [vibemind.club](https://vibemind.club).

## License

MIT. See `LICENSE`.
