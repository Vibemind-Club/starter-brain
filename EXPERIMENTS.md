# Experiments

The rules in this brain make cost and speed claims, so we measure them. This page is the experiment program: controlled A/B runs of the practices the brain recommends, published with their numbers and their caveats. It is layered on purpose: small pilot shapes first, to isolate the mechanisms and define what to measure; a real-scale controlled wave next, to test whether the shapes hold; product-level and long-task layers after that.

Two ground rules for everything below. Every run was re-verified from outside the session (tests re-run, commits inspected); an agent's own "done" claim was never counted as a result. And where a small-scale result failed to hold at real scale, that reversal is published here, plainly.

## Pilot shapes (complete)

Three small controlled experiments on toy tasks, one purpose each. Their numbers are citable only as toy-scale figures; their job was to expose the mechanisms and define the real-scale measurements, and one of their headlines did not survive the trip (noted below, and in Layer 1).

### Pilot 1: serial vs parallel-isolated vs parallel-shared

**The setup.** Three small tasks on a purpose-built repo, run three ways with real Claude CLI sessions (Haiku): one serial session doing all three; three parallel sessions, each in its own git worktree; three parallel sessions sharing one checkout. One trial per condition.

**What it showed.** Parallel-isolated beat serial 2.79x on wall clock, and the isolation ceremony (worktree setup plus three merges) took 408 milliseconds, 1.2 percent of the run. Three fresh sessions also used 15 percent fewer output tokens than the one long session. **That token finding was a toy-scale artifact: it did not hold at real scale.** At real-task size (Layer 1 below), isolated is token parity with serial and costs more in metered dollars, not less. We are publishing the reversal because the toy figure is quotable and wrong at scale.

**The useful survivor.** The shared checkout produced no hard failure (timing luck), but its slowest session burned about ten extra turns of git forensics watching the repo change underneath it: 17 turns vs 7 against its isolated twin, +61 percent wall clock, +85 percent output tokens on that session. Isolation removes constant soft confusion, not just occasional hard collisions. This finding did survive, and grew, at real scale.

**Caveats.** Haiku on toy two-file tasks; single trial per condition; no statistical power; the shared checkout passing was one lucky draw.

### Pilot 2: the re-telling tax (cold start vs durable docs)

**The setup.** An identical convention-dependent task on a repo with unguessable conventions and one trap (tests fail unless a generated manifest is resealed first). Condition A: fresh clone, no docs. Condition B: the same clone plus a 43-line CLAUDE.md and a short handoff doc. Four verified runs per condition, Haiku.

**What it showed.** The cold side ran 1.8x slower, took 2.3x the turns, made 5.9x the orientation calls before its first write, processed 2.4x the input tokens, cost 1.75x, and produced every tool error in the experiment. Success was 4/4 on both sides; at this size the tax is time, tokens, errors, and variance, not outcomes. Two findings: a cold start pays either the read tax or the failure tax (three cold runs read everything first; the fourth skipped reading and walked into the trap), and docs make agents deterministic (the docs condition ran the same eight-to-ten-call sequence all four times and provably used the docs; the cold condition wandered a different path every run).

**Caveats.** A deliberately tiny repo (11 files) made discovery unrealistically cheap for the cold side; quote the shape, never the 1.8x as universal. The docs covered exactly this task, a best case; stale docs narrow the gap. N=4 per condition, but zero overlap between conditions on any headline metric.

### Pilot 3: the long-context tax (fresh vs in-session)

**The setup.** The same measured task run as the only prompt of a fresh session, and as the final item after seven padding tasks in one long session. N=3 per condition, Haiku.

**The honest result: at small scale, the long session won.** 0.77x wall clock, 0.52x cost. The naive hypothesis, "long sessions always cost more," fails at small context: the long session's history billed at cheap cache-read rates while the fresh session paid a cold-start tax (cache writes were 42 percent of its cost) plus about three extra exploration turns re-learning conventions.

**The mechanism is real, measured, and unbounded.** Billed context grew monotonically on every turn of the long session (30,171 to 38,114 tokens across 21 turns): every message re-carries the entire conversation as input, a tax that grows forever while clear-per-task stays flat. Crossover estimate at Haiku rates: roughly 50k tokens of conversation growth; about five times sooner at Opus-class read rates. This experiment's padding grew context about 8k tokens, deliberately below crossover, which is exactly why the long session won. The defensible claim, and the only one we make: past roughly 50k tokens of chat, the same task costs more in-session than fresh, and it only gets worse from there. Not "long sessions always cost more."

**Caveats.** Haiku, toy tasks, synthetic small padding, N=3. The long session's advantage depends on its cache staying warm; idle gaps re-pay cache writes. No quality claim in either direction from this sample.

## Layer 1: real-scale controlled wave (complete)

Preliminary summary; the consolidated write-up is pending an independent review of the full suite.

**The setup.** 41 runs: 8 cells of 5 trials each, plus a pilot run, on real open-source host repositories (nanoid at 791 lines of code, validator.js at 24k, undici at 130k), with real-shaped multi-file, test-carrying tasks instead of toy padding. Every session ran on a bare, verified-stock Claude Code profile (no custom instructions, no hooks, no extensions), so the baseline is stock behavior. Every run was independently re-verified: required-case tables re-run, the session's own tests re-run, the host repo's full pre-existing suite re-run, and each commit's file set inspected. All 41 runs passed. Total spend: $69.98. One measurement note travels with everything below: the benchmark machine was in active use throughout, so wall-clock figures are directional; token and dollar figures are exact.

**Speed and predictability.** At three parallel tasks (Sonnet, medium repo, medians of 5 trials), parallel-isolated finished in 259 s against serial's 547 s: a 2.11x median speedup on real tasks. The second half of the claim matters as much: serial wall clock swung from 325 to 1005 seconds across trials, while isolated stayed within 228 to 266. Isolation buys speed and tight delivery variance; the toy pilot's 2.79x may only be cited as a toy-scale figure.

**Cost, honestly, in two payment modes.** On metered API pricing, splitting into isolated parallel sessions is not cheaper: output tokens are parity (+2 percent) and dollars are a +26 percent premium versus serial, because each session pays its own cache creation. The metered headline is: a roughly 26 percent premium buys 2.11x speed and much tighter delivery variance. On subscription plans the same numbers read differently: batch wall time stayed flat (259 to 289 s) whether the batch held two, three, or six tasks, so six real features completed in the wall time of two, 92.6 percent of billed tokens were cheap cache reads, and zero of the 41 runs hit a blocking rate limit.

**The worst measured configuration is the common one.** Three sessions sharing one checkout, the thing people do by default, lost to plain serial on both axes across 5 clean trials: +18 percent output tokens and +51 percent dollars, with +65 percent more tool errors than isolated as sessions fought over a repo that moved underneath them. No trial produced a hard failure (timing luck, again), so we claim no failure rate; collision prevention is a property of isolation by construction (disjoint files per session cannot collide), and the measured soft tax above is what skipping it costs even when nothing breaks.

**Repo size barely moved the bill for scoped tasks.** Per-task cost was nearly flat from the 24k-line repo to the 130k-line repo ($0.63 vs $0.62, similar tokens, modestly more wall time). Pilot 2's caveat predicted the orientation tax would grow with repo size; for well-scoped, well-briefed tasks it did not. Our interpretation, marked as interpretation: a thorough brief flattens the size tax, because the agent works the few files the brief points at instead of reading the tree. Unbriefed orientation is where size should bite; this wave did not measure that regime.

**A cheap worker cleared the hard tasks.** On the hardest cell (three tricky validators with 25 to 34 exact required cases each), a Haiku coder went 5/5 clean at 0.42x the Sonnet dollar cost. The caveat travels with the claim: these tasks carried exhaustive specifications and golden case tables, a best case for a cheap model. Vaguer tasks are where the tiers would separate, and this wave does not measure that. Haiku also used slightly more output tokens than Sonnet; the saving is the per-token price, not fewer tokens.

**Caveats (these are the credibility).** Five trials per cell gives spread, not statistical power; treat 2.11x as "about 2x, with isolation removing the variance." Tasks were well-scoped with exhaustive specs, deliberately measuring the regime a good brief produces. Wall clock is directional throughout (shared machine). Two coder tiers only; no Opus-class coder in this wave. And the pilot's token-savings headline is explicitly dead at real scale: only the speedup and worst-configuration findings carried.

## Layers 2 and 3 (not yet run)

Layer 2 puts an orchestrator in the loop: the same fixed workload handed to orchestrator sessions at different model tiers, measured end to end, to test whether a smarter orchestrator pays for itself. Layer 3 adds long-task anchor runs and a locked holdout benchmark, committed in advance and never fitted on, for validating predictions. Results will be published here when they land.
