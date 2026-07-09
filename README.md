# VibeMind Brain (Starter)

A starter brain for Claude Code: working rules your agent reads at session start, kept current by watching the sources they stand on.

## The problem

Every fresh AI coding session starts knowing nothing about how to work well. Not your project, not your habits, not what happened yesterday. So you re-explain it, every time.

And the ground under the guidance keeps moving. Default models change and the token economics change with them, prices have end dates, features flip from opt-in to default, policies narrow. The facts live scattered across changelogs, release notes, news posts, and community threads, and they move faster than anyone tracks by hand.

## The idea

Give the agent a brain: a small set of working rules it reads the moment it starts, so it boots already knowing how to behave. The same brain keeps your project's memory in a notes file, read at the start of every session and written at the end, so a fresh session opens caught up and a finished one closes with nothing lost. You stop re-explaining, and you stop losing what you decided. And the rules themselves stay current by watching the sources, instead of trusting last month's facts.

## What we do

We watch six feeds: the [Claude Code changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md), the [Claude platform release notes](https://platform.claude.com/docs/en/release-notes/overview), [Anthropic news](https://www.anthropic.com/news), [Simon Willison's blog](https://simonwillison.net/tags/anthropic/), [Hacker News](https://news.ycombinator.com), and [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/). When something changes, we turn it into a working rule that clears one bar: it is **sourced**, with a link where you can confirm the claim yourself, or it is **self-evident**, a mechanism you can check by reasoning about how the tools work.

One recent pass through that loop, whole:

> **They shipped:** Claude Sonnet 5 launched as the new default model. Buried in the announcement: its tokenizer turns the same text into roughly 30% more tokens ([news](https://www.anthropic.com/news/claude-sonnet-5)).
>
> **The brain now:** recalculated how long a session should run before it gets expensive, and put the date the introductory pricing ends on its calendar. The splashy part of a release is the new model. The part that quietly changes your bill is the part you miss, and the part we watch for.

This repository is that method's free output: `CLAUDE-STARTER.md` is the brain you paste into your project, and `GUIDELINES.md` is the compiled rules with their receipts. The feed list with one line on each lives in `SOURCES.md`.

We also measure what these rules cost and save, in controlled experiments published with their caveats: see [`EXPERIMENTS.md`](EXPERIMENTS.md).

## Quickstart

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

This starter brain is the free slice of VibeMind, and we keep it maintained: re-grab the repo whenever you want the latest rules. How it runs a session, opening caught up, closing clean, checking its work before it claims done, is the real thing, the same discipline the paid product runs on. The full version is an app that keeps the brain current for you, automatically and continuously, and adds mission control for running several sessions at once instead of one in a terminal. It lives at [vibemind.club](https://vibemind.club).

## License

MIT. See `LICENSE`.
