---
layout: post
title:  "What I Learned from my First Vibe Coded Project"
date:   2026-04-06
categories:
  - AI
  - Personal Development
---
# What I Learned from my First Vibe Coded Project
I haven't manually written large portions of code in quite a while thanks to Claude and the improvements made by Opus, though I typically am still very hands-on. Recently I decided to invest some time in intentionally vibe coding a solution to a problem I was having as a way to learn more about coding agents and "AI-native" development that I might not have learned without loosening my grip on the process.

This was successful beyond my expectations, but has also reaffirmed my belief that being intentional about where we relinquish control over development 

## The problem
Recently, I find myself context switching more often than the pre-LLM days. Coding agents have just made it more accessible to juggle more tasks and as the Staff Engineer for my group of three teams, this has unlocked faster feedback loops across projects for me ([for better or worse...](https://lethain.com/limiting-wip/)).

Because of this, my bottleneck had shifted to "clean" workspaces. At Rover we use [Github Codespaces](https://github.com/features/codespaces) for development. It is sometimes just easier to jump into a fresh Codespace when switching contexts rather than stash / commit changes, pull, switch branches, refresh or rebuild containers, etc.

**The problem was now that it could take ~10-15 mins or so to create a fresh codespace from scratch,** including Codespace setup & personal dotfiles. This doesn't sound like much time, but doing this a few times a day can really break my flow state.

## The solution
**It would be nice if I could have some specified number of "hot" Codespaces ready for me on demand.** There are ways for organizations to pre-warm Codespaces for their developers, but the hard part relates to personal dotfiles. I use a very personalized Neovim setup alongside some hefty bash/etc customizations that is pulled in when I create Codespaces in Github (this is managed via a Github setting per account). Many of my coworkers use VS Code, or something else, with varying degrees of personal customizations in the form of dotfiles. **Organizational pre-warming doesn't easily fix this.**

It should be fairly trivial to write a tool to handle **personal** Codespace pre-warming (that is, pre-warming Codespaces with my dotfiles) in a manner that was "hands off", but it just simply wasn't something I was going to divert effort to myself.

Lucky for me, **code generation is incredibly cheap now thanks to coding agents like Claude Code.** It's easy to spend just as much time hand-holding a coding agent as you might spend doing the work yourself, so it was important for me to limit my involvement. Instead, I used this as an opportunity to try out **and learn from** [vibe coding](https://en.wikipedia.org/wiki/Vibe_coding).

## Vibe coding a solution
There are probably degrees of vibe coding - one one end you have the coding agent doing the work, but with close involvement and guidance from the developer, including code review. On the other end you have agentically driven process with the human input scoped strictly to requirements generation. While I didn't quite lean into the extreme, I took this as an opportunity to challenge my assumptions about what these coding agents can and can't do.

My process was effectively:
1. Document requirements as discrete work items.
2. Feed these work items to Claude (Opus).
3. Allow Claude to write the code in it's entirety (no hand holding).
4. Drive code review primarily though other adversarial agents.
5. **Manually** review for glaring issues (security issues, misunderstood requirements, etc.) once the agents reached consensus on approval.
 
Here are a couple things I've learned **so far**.

### The "good"
1. **Vibe coding is not inherently bad.** My first reaction to the concept of vibe coding in the early days of coding agent adoption was visceral. Having done this for over a decade, its hard to separate yourself from the development process without feeling a little anxious. **That said**, not all code needs to be perfect, and sometimes fast and functional is good enough. Consider the context - one-off scripts or personal tools are very different from user-facing production code.
2. **Code generation is cheaper than ever.** Before coding agents, I may have hacked around some problem for a while to get to the important details. I'd spend more time on trial and error than ideal, and even short scripts or tools cost time. Now, I can describe the problem to Claude and receive code that will at least get me started, if not address my need entirely. It's not always perfect code, but again, it doesn't need to be when I value results over code quality.
3. **Claude's custom Commands are awesome.** I learned a lot about scaffolding a project to be primarily driven by coding agents, but custom slash commands ended up being more impactful than I expected! Conceptually they are just skills that can be invoked with a slash command: I ended up with `/check` (dependency checking), `/lint`, `/pr`, `/release`, and `/test`. Though my intent was not to intervene often, these made basic repetitive tasks **so much easier** when I needed to.

### The "bad"
1. **Claude, even Opus, is not good at DRY by default.** So, much, repeated, code. More often than not this was functional, but it was actually painful to see important and generic functionality like date parsing or Github CLI response parsing duplicated multiple times. Almost certainly this was a result of incremental updates, but more than once I needed to task Claude with identifying and consolidating shared code.
2. **Claude will quietly grant itself "dangerous" permissions.** Watch for this when you use Claude to bootstrap itself for development purposes. My project uses a `settings.json` for Claude that manages permissions so that I could be relatively uninvolved in the development process. When I had claude create this file, it (of course) gave itself more permissive access to my local machine and to Github than I would have liked. One could argue that this is part of true vibe coding, so the point here is just that you should be **aware** and cautious of this.
3. **Claude will write complicated, borderline unmaintainable code.** Maybe this was made worse by nature of this tool being a bash script spanning multiple tools, but the complexity of `jq` filters and regex used in the code Claude generated for this project is not easy to mentally grep. These things all work and are conceptually sensible, but it is also the sort of code I might push back on if I knew other developers would need to understand it often.

## Enter `spaceheater`
Today I have a CLI tool with a `launchd` integration that ensures that I **always** have a couple clean Codespaces ready for context switching.

This tool (named [spaceheater](https://github.com/dbernard/spaceheater)) is effectively just an abstraction over the `gh` CLI for Codespace creation and maintenance with an added state layer for defining what "clean" means (up to date with master, no older than a few days, etc.). This integrates with local git repos, adopting `devcontainer` defaults if they exist for organizationally defined machine types, etc.

**This is absolutely not something I would have ever built for myself without coding agents, and is a clear example of how coding agents have flipped the mindset around rolling your own tools vs buying or adopting an existing tool.**

This tool has already sped me up noticeably by solving my pre-warming need, but I did build it for me and around my existing organization workflows so YMMV. Feel free to give it a try! I'll write a short follow up post on `spaceheater` soon.
