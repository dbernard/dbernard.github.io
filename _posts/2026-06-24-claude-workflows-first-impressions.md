---
layout: post
title:  "Claude Workflows First Impressions"
date:   2026-06-24
categories:
  - AI
---
# Claude Workflows First Impressions

[Claude Workflows](https://code.claude.com/docs/en/workflows) are (another) way of orchestrating subagents at scale.
This is useful for things like broad refactor work, deep resesarch (in fact, `/deep-research` is a Workflow built into
Claude by default!), or a broad code sweep.

More generally, Workflows enable tasks that:
1. need more agents than one session can reasonably coordinate
2. benefit from deterministic, codified, re-runnable orchestration

## Whats the difference?
If you're familiar with the ecosystem, your first thought might be something
like, "_why would I use workflows when subagents and agent teams exist?_". Great
question.

The key differentiator for workflows is **deterministic execution**. When
spawning subagents via the `Agent` tool, you're still running inside a
model-driven loop - **Claude** decides what to do next based on each subsequent
result.

With Workflows, **each step in the flow is governed by a Javascript workflow
script.** This script decides what to run next, and intermediate results live in
script variables. This enables a level of determinism across runs, no matter how
complex the runs may be. It also allows the entire Workflow to be repeatable
**and** resumable if interrupted.

## My experience with Workflows

Workflows unlocked a complicated refactoring effort for my organization. This
refactor involved cleaning up a huge volume of old feature flags from our
codebase - simple right?

Except these flags are part of a hand rolled in-house framework that was showing
its age - development environments didn't have access to the production flag
state, certain flag definitions require special handling, and flags were defined
in at least three different ways.

Workflows enabled us to define a deterministic approach to pruning these flags:
- Check Jira for cross-run idempotency (_has this ticket been picked up by
  another agent already in another run?_)
- Interface with a production snapshot of flag state to determine the kept
  branch (_subagent uses a script to pull from a packaged SQLite snapshot_)
- Do the cleanup work, escalate to a human if the Agent is missing context
  necessary for the cleanup (_no guessing_)
- Dead code sweep, using `vulture` before and after the cleanup task and diffing
  the results to validate any net-new dead codepaths after flag removal.
- Pre-PR adversarial review (_don't put this in front of a human without being
  relatively certain that the change is viable_)
- (Optional) Fix loop if the task is kicked back to the implementation Agent by
  the adversarial reviewer
- PR creation/iteration + ticket lifecycle (_When ready, open a PR, address
  feedback, and move the cleanup ticket through typical ticketing lifecycle)

This Workflow also has **Evals built in**, which was honestly the most powerful
feature addition to the process.

We had a large swath of cleanup tickets completed in H1 with links to PRs that
were approved and merged by humans (Claude may have done the work, but we were
at least sure that the work had been approved and shipped).

The Workflow could be run in "Eval mode", which ran the exact same Workflow
steps (see: **deterministic execution** above), but instead of creating PRs it
would kick off a scoring agent that would compare and score the implementation
agent's changes against the shipped human changes for a given historical
cleanup.

**_How?_** In Eval mode, the orchestator would select (or could be given) a
"golden set" of examples from out H1 cleanup tickets. Implementation subagents
would then rewind Git history back to pre-removal of each of those flags,
execute the removal flow, then pass the results to the scoring agent who would
compare the diff against the merged PR diff. Agents were scored based on how
well they matched up with the human diff.

This was **key** to iterating on our workflow. Eval mode helped us understand
where we needed to iterate for the best results - what context was lost or not
well understood by the agents, which types of flag definitions the agents
struggled with most, etc.

## First impressions
Without diving much more deeply into the specific's of the Workflow described
above (_final outcomes still TBD - this is an H2 2026 effort, and I'll likely
write a Rover blog post about the end result_), I do have some more generic first
impressions.

First is that Workflows are incredibly powerful, but should be applied
strategically. They do a lot, and you won't always need the horsepower (more on
this below).

Second is that governing the agent execution via script is really neat, but
avoid the temptation to touch this code. I belive that in the coding agent
world, there is now a distinction between code that I care about and code that I
do not care about - these workflow scripts are the latter. This script is
written for Claude, by Claude. With enough knowledge about the deep internals of
Workflows I'm sure I could meaninfully edit the script, but why would I? Let
Claude own this. Absolutely look at it, but don't touch.

Third, the determinism of Workflows is a huge part of what has been missing from
deeply complex tasks. Even if each independent step is simple, tying them all
together has been a challenge. We've all seen coding agents skip or deviate from
parts of a plan in a meaninful way. Workflows have so far helped to keep the
process on the rails with the added benefits of being repeatable, **massively**
scalable, and resumable if interrupted.

## What to know before reaching for a Workflow
Workflows are incredibly powerful, but they are also a sledgehammer - **you do
not need workflows for every mutli-agent execution context.**

Workflows can spawn many agents, each with their own context windows. This can
absolutely **burn** tokens - a single, happy-path (i.e., minimal `fix` loops)
cleanup batch of the workflow I described above could consume well over 1M
tokents.

## Final thoughts
The primary benefit that we gained by using Workflows for the tasks above was in
the **deterministic execution** of a complex task. The only way that our cleanup
effort scales _well_ is if the process runs the same way each time and minimizes
the need for human intervention. The majority of these tasks are simple, but
**human code review is the bottleneck**, and nobody is really shipping fully
autonomous merges quite yet.

So now the emphasis is on ensuring that this process is stable and prioritizes
clean, ready-to-approve PRs. Workflows helped us build this rigor into a tool
that we can hand off to others instead of forcing them to re-discover best
practices or hoping that their agents behave in the same way across sessions.
