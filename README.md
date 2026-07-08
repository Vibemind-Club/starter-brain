# VibeMind Brain (Starter)

A starter brain for Claude Code: working rules your agent reads at session start, kept current by watching the sources they stand on.

## The problem

Every fresh AI coding session starts knowing nothing about how to work well. Not your project, not your habits, not what happened yesterday. So you re-explain it, every time.

And the ground under the guidance keeps moving. Default models change and the token economics change with them, prices have end dates, features flip from opt-in to default, policies narrow. The facts live scattered across changelogs, release notes, news posts, and community threads, and they move faster than anyone tracks by hand.

## The idea

Give the agent a brain: a small set of working rules it reads the moment it starts, so it boots already knowing how to behave. Keep those rules current by actually watching the sources, instead of assuming last month's facts still hold.

## What we do

We watch six feeds: the [Claude Code changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md), the [Claude platform release notes](https://platform.claude.com/docs/en/release-notes/overview), [Anthropic news](https://www.anthropic.com/news), [Simon Willison's blog](https://simonwillison.net/tags/anthropic/), [Hacker News](https://news.ycombinator.com), and [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/). When something changes, we turn it into a working rule that clears one bar: it is **sourced**, with a link where you can confirm the claim yourself, or it is **self-evident**, a mechanism you can check by reasoning about how the tools work. No arguments.

This repository is that method's free output: `CLAUDE-STARTER.md` is the brain you paste into your project, and `GUIDELINES.md` is the compiled rules with their receipts. The feed list with one line on each lives in `SOURCES.md`.

## Quickstart

Five minutes. No account, no installer, nothing to trust but a repo you can read.

1. Get the file.

   ```
   git clone https://github.com/Vibemind-Club/starter-brain.git
   ```

   Or skip the clone and copy [`CLAUDE-STARTER.md`](CLAUDE-STARTER.md) straight from GitHub.

2. Copy everything below the horizontal rule in `CLAUDE-STARTER.md` into your project's `CLAUDE.md` (create the file at your project root if it does not exist).

3. Open Claude Code in that project. The next session boots with the brain loaded: banner, catch-up, and a question about what you are building.

## What you get

**Thalamus, the front desk.** A named character who runs a small boot ritual at the start of every session: reads the notes, prints a status banner, gives you a two-line "since last time" catch-up, and asks what you are building. At the end of a session it closes the books: decisions, shipped work, and open questions go back into the notes before goodbye.

**External memory that survives sessions.** A plain `NOTES.md` at your project root holds the current task, what shipped, open questions, and what is next. Sessions read it at boot and sync it at close, so you can end conversations freely and lose nothing.

**A status board on demand.** Type `board` and the session renders a compact ASCII summary: current task, what changed since last session, what finished in this one, open questions, next up. No dashboard, no service, just your notes read back in one clean block.

## The full product

This starter brain is the free slice of VibeMind. The full version is an app: the same brain kept continuously current as the sources change, plus mission control for running several agents at once. It lives at [vibemind.club](https://vibemind.club).

## License

MIT. See `LICENSE`.
