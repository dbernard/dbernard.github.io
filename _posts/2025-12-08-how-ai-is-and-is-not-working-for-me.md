---
layout: post
title: "How AI Is (And Is Not) Working For Me As A Staff Engineer"
date: 2025-12-11
categories: AI
---
# How AI is working (and not working) for me as a Staff Engineer
AI tools have really only become a serious part of my daily workflow within the past year or two. This post is meant to
be an informal review of what **has** and **has not** been working for me most recently as I use these tools to boost my
own productivity, and the productivity of those around me.

## What's Working
### Rubber Ducking
"Rubber ducking" is the process of explaining problems or thought processes to an inanimate object (*are LLMs
inanimate?*). By forcing yourself to explain and talk about these things, you're likely to identify key concepts that
help illuminate the correct decisions or next steps.

My experience is that, **when done intentionally** (see "Blind Trust" in the "What's Not Working" section below), LLMs
can be really useful here if only as a vector for deeper thought and forcing you out of mental blocks.

By the way, when asked if it would consider itself an "inanimate object", ChatGPT has opinions:
> Short answer: **No.**
> Longer explanation: I don’t have a physical body, so I’m not an “object” in the way a rock or a
> chair is. But I’m also not alive, conscious, or animate. I’m a **software system**—patterns of computation running on
> hardware. So I don’t fit neatly into the categories “animate” or “inanimate.” If you need a label, you could think of
> me as **non-physical** or **digital**, rather than inanimate.

### Architectural Diagramming
Specifically Mermaid flow charts, though I haven't experimented with much else. LLMs seem to be really good at working
with text-based diagrams like Mermaid. I've had a lot of early success generating architectural diagrams using LLMs for
both the current state and hypothetical future iterations.

I've had success asking an LLM to clean up a diagram to improve readability, proposing an architectural change and
asking the LLM to adjust the diagram, and even asking for a fresh perspective on a specific architectural design. The
latter should always be taken with a grain of salt - good architectural decisions will always depend on a multitude of
factors that you almost certainly haven't accounted for in the context that your AI Agent has access to.

### Generic Technical Tasks
AI coding agents have already come a long way. The advent of MCPs and Agent Skills (more on those soon) have only made
these coding agents **more** powerful. AI coding agents do a decent job with generic technical tasks - tasks with little
ambiguity that don't require deeper business knowledge to solve appropriately.

For example, feature flag cleanups are generally simple - delete the flag and clean up the code path that is no longer
used. This also isn't a particularly stimulating task, and so I don't feel like I'm necessarily missing much potential
growth by offloading these tasks to coding agents.

Frontend/UX changes are also a great candidate for AI coding agents. These tasks tend to be fairly generic and
unambiguous. I do strongly suggest documenting common patterns used within your organization via context such as with
`CLAUDE.md` [files](https://www.anthropic.com/engineering/claude-code-best-practices). An AI coding agent might fulfill
some set of requirements, but may do so in a way that breaks with commonly accepted patterns within your specific
codebase.

### MCP Servers
MCP servers take these tasks to a whole new level. I'm currently overseeing an org-wide migration to a new feature
flagging and experimentation solution, and cross-team health metrics for the next half will include cleaning up flags
that use our old solution. Using AI coding agents with access to the Atlassian MCP, I was able to 1) find all flag
references across the codebase for the old flag style and 2) cut tickets for removing or migrating these flags to the
new solution, assigning the correct team owners where context allows.

None of this is particularly complex, but it **_is_** time consuming. Offloading this work to an AI coding agent saved
me days of searching code, assigning correct team ownership, and creating tickets myself.

### Agent Skills
Agent Skills for Claude Code have helped to fill the skill gap related to execution of more complex technical tasks.
Agent Skills allow you to equip Claude Code with reusable, modular capabilities that extend Claude's existing
functionality. Skills define exactly how Claude should handle a given task, metadata about that task, and provide
Claude with access to the tools it needs to complete the task. Combine Agent Skills with MCP access and you can do some
really neat things!

Recently I've been using Agent Skills to define how Claude should handle stale feature flag and experiment clean up
tasks. These skills define how we use feature flags in our codebase, what tools Claude has access to for these tasks,
how Claude should handle ambiguity, and much more.

Equipped with this skill, our engineers can ask Claude to clean up a specific flag or experiment, clean up all stale
flags for a given team or set of teams, and even ask Claude to handle the flag or experiment clean up task defined in a
given Jira ticket. The flexibility and consistency of the tool is a direct result of the Agent Skill.

### Learning Frameworks & Generic Technical Domains
I don't work in the frontend codebase often - I'm proficient, but by no means an expert in core frameworks like React
and React Native.

AI coding agents have made it so much easier for me to context switch to frontend development when necessary by
effectively up-skilling me when I run into questions or need guidance around how these frameworks work and what some
best practices might be.

Now, I do want to be very clear - I personally believe that human-to-human pairing is a better solution to this,
providing mentorship and pairing opportunities that benefit both parties. That said, time won't always allow for this,
and my views on human vs AI communication don't change the fact that this just **works**.

### Technical Documentation
Another major time saver has been deferring technical / framework documentation to AI coding agents. Something like
documentation for an internal framework for developers in your codebase is a great candidate for bootstrapping with an
AI coding agent. These coding agents are generally very good at understanding what code does, and translating that
behavior to well-organized documentation.

You did hear me recommend "bootstrapping" your documentation in this way - sometimes the "why" is just as important as
the "how", and your coding agent may not have as clear access to that information.

Project this out a bit and you can imagine a setup where changes to a given internal framework trigger an AI agent to
review and update the relevant documentation, preventing the inevitable "documentation drift".

### Meeting Notes
Our organization uses the Google Suite of tools, and as a result we have access to Gemini for meeting note annotation. I
have come to really love this feature as a way of removing the need for a human to take notes.

My experience with meeting note taking has been that someone needs to be the primary note taker, and that person has a
much harder time being present and involved in discussion when they are forced to keep up with the pace of discussion
while taking notes. Offloading notes to AI allows us to keep everyone involved in the meeting with minimal pauses or
lost context.

## What's Not Working

### Blind Trust
I mean, _duh_. For all of us except for maybe the most overzealous of vibe coders this is probably obvious, but always
carefully vet and review the output of LLMs. Blind trust in AI output will bite you sooner rather than later. Treat code
output just as you would review code from a coworker, and treat technical proposals and guidance as a discussion worthy
of scrutiny.

These tools can be a real force multiplier in the long run, but only if used appropriately.

### Being "Absolutely Right!"
"_You're absolutely right!_" has become a bit of a meme at this point for those working with Claude. This seems to be
the standard response when Claude is reasonably challenged on just about _any_ topic. The broader issue at play seems to
be prevalent across AI agents and centers around these agents pandering to users.

To me, this feels like an unhealthy way to manage discourse. Being told I'm "absolutely right" **up front** makes me
less likely to think critically about the rest of the context presented afterwards.

### Business Logic
You're unlikely to ever give an LLM or coding agent enough context to cover the entirety of your business logic for any
sufficiently complex business domain in order for it to make the right decisions. What's worse, these tools (in my
experience) will often invent a decision when ambiguity is encountered rather than ask for clarity.

My experience has been that AI coding agents do not operate well within nuanced business domains because these domains
are just inherently difficult to understand without institutional knowledge.

### Compaction
Compaction refers to the process by which an AI coding agent compresses or condenses large codebases, plans, or
intermediate reasoning steps into smaller, more efficient representations without losing essential functionality. In
practice, you see this after interacting with an AI coding agent for some prolonged period during which the agent has
exhausted its reserve of contextual tokens.

Most of the issues that I have had with AI coding agents have been a side effect of compaction. So much so that I
regularly build my workflows _around_ compaction, starting fresh with updated prompts before compaction happens.

The typical problems that I encounter with compaction are things like the agent forgetting some important context
related to the task, skipping or ignoring an important step that coincides with when compaction happened.

### AI Code Reviews
These are generally fine for a first pass to catch low-hanging issues, but often end up being noisier than is useful or
missing important context related to the change. I think this is more generally a side effect of the "Business Logic"
issue described above, but another important aspect here is that these tools just aren't ready to be a trustworthy final
source of review for production applications, so it ends up just being an added step to the code review process.

### Large Codebases
The real issue here, again, is compaction. There is just so much context in a large codebase that AI coding agents very
quickly run out of tokens and require compaction to continue. Most notable codebases existed before AI coding agents,
and as such were not built for use with these tools.

File size is a common issue that I have seen. For example, very large unit test files will quickly chew through
available context. Microservices _may_ fare better here, but a sufficiently mature monolith is likely to struggle.

## Final Thoughts
It's crazy to think that we're still likely in the "early days" of AI tools. There is so much potential here, but there
are also some common and very impactful issues as well. There are very strong feelings on both ends of the AI spectrum -
we've all heard CEOs proclaim that software would be entirely written by AI within *months*, just like we've heard
fellow developers claim that coding agents are only capable of producing slop.

I think it would be naive to think that the current state of these tools is indicative of the long term. I also think
that writing safe, reliable software without humans in the loop will require more than an LLM. Additionally, I'm still
waiting for the "hook" that really makes AI tooling irreplacable in my workflows. **Nothing** that I use AI for at the
moment is absolutely necessary, and efficiency gains are still nebulous enough to make it difficult to truly sell as a
force multiplier. Its exciting to work with these tools and there certainly is potential here, but its important to be
realistic about hype versus true impact.

As with all things, I believe that there is a balance. My guidance to fellow developers who trend towards the "anti-AI"
end of the spectrum has been this: these tools exist now, and they are not going away regardless of how you feel about
them. As a software engineer, it is your responsibility to understand how these tools work so that you can use them if
and where appropriate, but more importantly so that you can help guide the usage of these tools by your mentees and
fellow engineers. It is arguably **more** important to know what use cases AI coding agents and other tooling **do not**
work well for, and as a technical leader you should be capable of providing this guidance to others.

I'm excited to see where these tools go in 2026!
