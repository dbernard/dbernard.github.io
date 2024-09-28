---
layout: post
title:  "On Prioritizing Technical Debt"
date:   2024-09-20
categories: technology
---
The fast pace of Product Engineering often means quickly transitioning from
project to project, leaving little time to circle back to resolve any technical
debt incurred during implementation.

Worse, densely packed roadmaps often encourage "quick and dirty" solutions,
where shortcuts are taken to get something functional into the hands of users
sooner. This decision to incur technical debt is not necessarily bad, however
technical debt with no plan or intent to be paid down is no longer technical
debt - it's just bad code.

When we choose to incur technical debt, how do we keep ourselves honest and
accountable for resolving it?

## What is "Technical Debt"?

First, **technical debt is not inherently bad.**

Like monetary debt, technical debt lets you achieve a thing earlier than you
would by paying the full cost upfront. Imagine, as a first-time home buyer, if
you had to save for the full cost of your first home before you could buy it.
You'd be saving for a **very** long time.

Similarly, with software development you can **choose** to incur technical debt
by taking some technical shortcut (de-prioritizing performance, adopting a
suboptimal architecture, deferring documentation, etc.) in order to deliver
results more quickly.

The catch to this is that, like real debt, technical debt must be paid down.
Technical debt is paid down by spending the time and effort that had been
deferred earlier to address the shortcuts that we took.

Also like real debt, technical debt doesn't typically remain constant - it
incurs interest over time as new features are built on top of it, as new
engineers are forced to work around and within the offending code, and as we
lose context around why we made the decisions that we did in the first place.
All of this makes it more expensive to pay this debt down as time goes on.

![Cost to pay down tech debt over time](/assets/images/PayingDownTechDebt.png)

Tech debt is **frustrating** for developers because most of us do not go into
new projects set on executing them suboptimally. The feeling of building well
architected, clean code is hard to match, but good developers understand when it
makes sense to sacrifice some aspect of quality to meet the needs of the
customer. Regardless, it never feels *good* to introduce technical debt, even
when it might make sense to do so.

## Prioritization

The solution for technical debt is effective prioritization. Not all tech debt
will *or should* be immediately prioritized, so identifying how to prioritize
tech debt is critical to maximizing the impact that you bring to your product.

### Misconception: "I shouldn't have to justify prioritizing tech debt"

I hear this comment in some form all the time from developers.

Earlier we discussed that technical debt can be frustrating to developers. That
frustration builds when the project wraps and suddenly there seems to be no
appetite from leadership to prioritize paying down this technical debt. The code
works after all, and our next roadmap item has an estimated impact to the
business that is hard to ignore!

*"I shouldn't have to justify prioritizing this tech debt! Leadership should
just trust my judgement!"* I sympathize with this mentality, but it just isn't
how businesses operate. Product Managers do their jobs by identifying new
product opportunities, then prioritizing them based on the impact that they
might have on the business (*I'm simplifying, but generally this is how it
works*).

This makes good PMs adept at justifying work by placing a dollar value on the
work and/or projecting a quantified change to some key business metric,
typically backed by extensive analysis. *"Feature improvement B will generate
between $1.5MM and $2MM for the business over the next year, while the addition
of feature C will increase customer retention by 15%."*

Put that statement next to, "*We need to optimize the API that we implemented
for Feature A and add documentation now that we've launched*" and it becomes
clear why prioritizing tech debt is so difficult, **and why developers do, in
fact, need to justify prioritizing tech debt.**

## Prioritizing Tech Debt

Prioritizing technical debt can't always compute to a raw dollar estimate, or at
least not one that will always outshine the potential of new feature work.
Luckily, there are other ways to keep you organization accountable for the tech
debt that it incurs.

### Document, and have a plan of action

When the decision is made to take on some form of tech debt in the short term,
**document it, along with a plan for resolving the technical debt**.

Documentation can take multiple forms, but the most effective way to document
technical debt is to add code comments that describe **why** you made the
decision that you made.

Including a plan for resolving technical debt helps to keep you and your team
accountable for its resolution. **Without a plan, technical debt is likely to be
forgotten and go unresolved until it becomes a much more expensive issue.**

Creating a plan for resolving tech debt at the time that the tech debt is added
also serves as a great way to 1) inform how much effort will need to be spent to
pay down this tech debt, and 2) persist the desired vision for the code for
future developers who aren't you and may not know your original intent.

Consider ways to make this plan part of the development life cycle for your
project. For example, you could use milestones to break your project down into
concrete deliverables, including a post-launch tech debt payment plan:

```
+--------------+      +--------------+                     +----------------------+
| Milestone 1: | ---> | Milestone 2: | --- [ LAUNCH ] ---> |     Milestone 3:     |
|     API      |      |   UI / UX    |                     | Post-launch clean up |
+--------------+      +--------------+                     +----------------------+
```

Regardless of approach, you should find a way to plan for the payment of tech
debt that works for you and your team. If you are a Staff Engineer, make sure to
discuss this plan with your Engineering Manager and Product Manager to align on
how best to keep your team accountable, and ensure that the appropriate amount
of time is allocated to this effort.

### Have OKRs and working agreements

When you find an approach to technical debt that works, consider making it a
working agreement across your team and organization.

As an example, let's say our team finds success by creating tickets for incurred
technical debt, applying a `tech-debt` label to these tickets. The team then has
an allocation of 10% time per sprint to spend on tech debt.

We can formalize this approach into a documented "working agreement" for our
team, holding ourselves accountable to documenting and addressing the tech debt
that we introduce.

From this working agreement we can also create a set of "objective key results"
(or OKRs) that help keep us on track. Perhaps our team creates an OKR that says
that we will have no more than N points of outstanding major technical debt
older than three months, using the `tech-debt` labeled tickets as our tracked
bucket of points (again, just a hand-wavy example).

With regular check-ins, these OKRs can keep us honest and accountable regarding
how much tech debt we are letting slip through the cracks.

### How much does it cost to NOT do this work?

Just as Product Managers do for new product work, try to quantify the dollar
value or time impact that this tech debt inflicts on the organization by simply
existing.

"*Until optimized, this API executes a query that will run up against our
performance SLAs in approximately 10% of requests.*"

"*Team X will be blocked on their new upcoming feature work until we clean up
the hard-coded values that we added to reach the launch deadline on time*."

"*By not cacheing these third party responses, we will incur approximately
$50,000 in excess vendor costs per year, scaling linearly with traffic.*"

Statements like these put a measurable weight behind the impact of tech debt and
make it easier to argue in favor of prioritization. They also help you
understand how impactful some tech debt is, which informs **how** you prioritize
it.

### What tech debt comes first?

Some tech debt tasks will be more impactful to resolve than others. If you've
found a way to quantify the cost of this tech debt, you've made it relatively
easy to compare this task to others.

Raw cost is not the only way to order your tech debt, however. Pay attention to
how specific technical debt interacts with other work - if you know that another
team will be building on top of some tech debt that you intend to resolve,
consider weighing those tasks more heavily to prevent future dev work from
needing to work around it or worse, build on top of it.

### Not all tech debt will (or should) be paid down

Ideally this is just your least impactful tech debt, but sometimes technical
debt just won't get paid down. This likely means it was never impactful or high
priority enough to be prioritized and other tasks were more valuable. It's
important to remember that **this is fine**, and it's a part of real world
software development.

An example of this might be during the A/B testing phase of some new feature
work. During this stage, it might not make sense to commit time and effort to
technical debt if the feature it relates to might not end up being adopted.
Paying down this tech debt might ultimately be wasted effort if the code ends up
getting thrown away anyways.

## Summary

Technical debt is a natural part of software development in the real world, and
it is not an inherently bad thing. The trouble comes when technical
organizations do not have an appropriate strategy in place for paying down
technical debt.

This write up explored ways in which you can think about prioritizing technical
debt within your team and technical organization, including:

- Clearly documenting technical debt and proactively planning for addressing
tech debt
- Identifying working agreements and OKRs that your team can use to stay
accountable
- Estimating the cost and/or impact that technical debt has on your organization
- Ensuring that technical debt tasks are prioritized in the appropriate order
- Understanding when to prioritize and when to **defer** paying down technical
debt

Paying down technical debt is essential to a healthy codebase. When tech debt 
goes unpaid, it is no longer tech debt; instead, it simply becomes bad code, the
cost of which must be paid each time your fellow developers and customers
interact with it. 
