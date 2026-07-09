# VibeMind Brain (Starter)

A starter brain for Claude Code: working rules your agent reads at session start, kept current by watching the sources they stand on.

## The problem

Every fresh AI coding session starts knowing nothing about how to work well. Not your project, not your habits, not what happened yesterday. So you re-explain it, every time.

And the ground under the guidance keeps moving. Default models change and the token economics change with them, prices have end dates, features flip from opt-in to default, policies narrow. The facts live scattered across changelogs, release notes, news posts, and community threads, and they move faster than anyone tracks by hand.

## The idea

Give the agent a brain: a small set of working rules it reads the moment it starts, so it boots already knowing how to behave. The same brain keeps your project's memory in a notes file, read at the start of every session and written at the end, so a fresh session opens caught up and a finished one closes with nothing lost. You stop re-explaining, and you stop losing what you decided. And the rules themselves stay current by watching the sources, instead of trusting last month's facts.

## What we do

We watch six feeds: the [Claude Code changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md), the [Claude platform release notes](https://platform.claude.com/docs/en/release-notes/overview), [Anthropic news](https://www.anthropic.com/news), [Simon Willison's blog](https://simonwillison.net/tags/anthropic/), [Hacker News](https://news.ycombinator.com), and [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/). When something changes, we turn it into a working rule that clears one bar: it is **sourced**, with a link where you can confirm the claim yourself, or it is **self-evident**, a mechanism you can check by reasoning about how the tools work.

One pass through that loop, whole:

> **They shipped:** Claude Code will auto-summarize a conversation once it grows too big to fit, so a long session can keep going ([changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)).
>
> **The brain now:** keeps your project's memory out of the conversation to begin with, in a notes file it reads fresh each session. There is nothing to summarize, so nothing gets quietly dropped. The tool's answer to a full context is to compress it. The brain's answer is to never fill it with your memory in the first place.

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

## See it

Open a session and the front desk greets you, already caught up from the notes:

```
∿∿∿ VibeMind ∿∿∿  starter brain

  thalamus  · front desk online
  notes     · loaded
  task      · wire the password-reset email
```

Type `board` for the whole picture. On day one it is empty:

```
┌─ board ────────────────────────────────
  task    none yet
  since   none
  done    none
  open    none
  next    none
└─────────────────────────────────────────
```

After a day of work, it is your project's memory at a glance:

```
┌─ board ────────────────────────────────
  task    wire the password-reset email
  since   2026-07-08  shipped the login rate-limiter
  done    reset-token model + migration; test email sends
  open    expire reset tokens at 1h or 24h?
  next    reset-form UI, then the success page
└─────────────────────────────────────────
```

## The full product

This starter brain is the free slice of VibeMind, and it is a real brain, not a teaser: the session discipline it runs on, opening caught up, closing clean, checking its work before it claims done, is the same discipline the paid product runs on. We keep it maintained, so re-grab the repo whenever you want the latest rules.

The full product runs a more powerful brain. Three things the paste-in file cannot do on its own:

- **It stays current on its own.** The starter is a snapshot you re-copy by hand. The full brain updates itself continuously, so it never drifts behind the model, pricing, and policy changes the rules depend on.
- **It commands the machine.** A terminal runs one session at a time, and the board above is text you type up. The full brain runs many sessions at once in isolated lanes, and that board becomes a live visual dashboard, mission control for everything in flight. That parallel-isolated setup is the exact configuration our own experiments measured as the best way to work, and the one no model chooses on its own.
- **It remembers wider.** The starter keeps one project's notes. The full brain carries memory across projects, and on a team it adds a shared layer every seat inherits.

The starter gets you the discipline. The full brain keeps it current, runs it at scale, and remembers across everything you build. It lives at [vibemind.club](https://vibemind.club).

## License

MIT. See `LICENSE`.
