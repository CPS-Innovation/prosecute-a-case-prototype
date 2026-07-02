# Replace CMS: strategic assessment and recommendations

## Who this document is for

Everyone who can help us change direction. 

It needs people to flag anything they disagree with. Anything major gets discussed at the next workshop, so we can move on to detailed planning instead of debating the approach.

## Executive summary

What we’re doing is making the replacement of CMS:

- slower to deliver
- ⁠⁠harder to adopt
- ⁠⁠more technically complicated than it needs to be

As a result we’re not seeing better outcomes for CPS.

We need a different approach that:

- prioritises delivering a whole service over individual pieces of functionality
- builds on a sound technical architecture from the start
- delivers real value as soon as possible

## The problem explained in more detail

Approximately 3 years ago the “Replace CMS” programme began.

The goal of “Replace CMS” is to replace the legacy case management system (CMS). 

We have not replaced any of CMS.

Here are the main reasons for that:

### Reason #1: We are replicating CMS, not replacing it

The standard approach for replacing a live legacy system is the strangler fig pattern: 

Build a new system that does the job better, migrate users to it as it becomes ready, then decommission the old one once all users have moved across.

That’s not what we are doing. 

Instead, we’ve been building feature-products — separate systems that each handle a slice of CMS functionality — alongside it, connected back to CMS rather than replacing it.

Because the feature-products maintain compatibility with CMS, what we design is constrained by what CMS can support. For example, Work Management is essentially a reskin of the task list within CMS. You can redesign the screens, but not the underlying model. The same broken workflows and workarounds carry over.

This undermines the whole point. We’re replacing CMS because it does not meet user needs very well and does not follow the GOV.UK Service Standard.

Voluntarily inheriting its data model means we inherit the problems we’re trying to escape.

### Reason #2: The current approach makes CMS harder to remove over time

By connecting every feature-product back to the CMS database, it remains the permanent system of record. 

Every feature-product that reads from or writes back to it is a dependency that has to be unpicked before decommissioning can happen — whether or not we've changed the data model at all.

We have also been extending the model beyond what CMS supports. For example:

- reminder tasks are a CMS task type some teams relied on for follow-ups; to stop users creating them, we have added the ability to create notes in the ‘planning’ tab within Work Management instead. These do not appear in CMS.
- we have created new task types, such as “Early advice manager triage” which only appear in Work Management, not in CMS.

This creates a fork between what CMS knows and what the feature-product knows, and users working in CMS miss information that only exists in the feature-product.

The goal of this programme is to decommission CMS. But every new feature-product connected back to it makes that harder. The cost compounds with every change to a feature-product as we introduce additional transitional architecture and there’s a need to support both systems simultaneously.

Removing CMS has to be the design goal from the start — not something to figure out once enough features have shipped.

### Reason #3: How we are organised makes the problem worse and slows us down

We have separate teams for each feature-product, for example:

- Housekeeping (document management)
- Work management (task list)
- Case search

But this is a terrible user experience because it’s built around what tools they need to use, not the process or journey they need to take. It’s also negatively impacted the system architecture which is centered around the team structures, not the system as a whole.

Users just have a goal they want to achieve, such as review a case, and the journey involves all 3 of the above feature-products. This does not give users what they need.

The main reason we organised teams around feature-products was a result of following the proof of concepts that were initially created in isolation.

The result is that features get shipped because they are a unit of work that is easier to plan, fund and demo, without anyone owning whether those features add up to something users can actually do their whole job in. 

This goes against the GOV.UK Service Standard, which says to design and build the whole end-to-end journey a user needs to complete, not a screen, a feature, or a task at a time.

Working this way also makes us slower to design and build.

The feature-products rebuild functionality that already exists in CMS — constrained by the same data model — while adding the ongoing overhead of keeping them connected to it. That cost compounds with every new feature-product added.

Significant improvements can't be delivered either, because design is constrained by the same model. 

There is little point designing a fundamentally better experience if CMS can't support it, so designers end up working around constraints rather than solving the underlying problem. This severely limits the value that we can add.

For example, the redesigned task list may be easier to use and more accessible, but the task model is flawed and cannot properly meet user needs.

### Reason #4: Users don’t adopt what we build, so the work delivers little real value

We have had positive feedback relating to specific aspects of the new feature-products in comparison to CMS.

But we also have constant feedback that users hate using multiple systems to do their job. And would generally prefer to use CMS because it can do everything.

This is not suprirsing given that switching between tools to do one job is a significant cognitive load. 

Users and their managers need to be confident in the new product before they will switch. Our approach up to now does not give our users confidence, instead puts significant burden on them. They are already overworked.

This is because casework is one continuous journey. A case moves from arrival through review, charging decisions, hearings and outcome. Each of these things have sub journeys. For example, a user reviewing a case may simultaneously need to request information from the police, contact a victim, or update the defence. 

There is no clean seam that a feature-product can cut across without leaving part of the journey incomplete.

The value of what we build is conditional on adoption.

### Reason #5: The feature-products have not been able to access the CMS database directly

Up until early 2026, the feature-products haven’t had direct access to CMS's database. 

They have extracted data by parsing what CMS renders on screen — screen scraping — rather than reading from the source. 

This has been fragile: any change to CMS's layout or output can silently break the integration. 

It's also costly to maintain, and it limits what data can be reliably read or written. Every feature-product is built on that foundation, so the technical risk compounds with each new one added.

As of July 2026, we do not have write access, and continue to save data by operating CMS screens.

## What to do instead

Changing approach is not just about fixing problems. It's also a significant opportunity. 

Starting from the ground up means we can redesign the complete case management journey around what prosecutors and caseworkers actually need. 

This will dramatically reduce the burden on already-overstretched users, improve outcomes for victims and defendants, and improve operations for CPS.

1. Drastically reduce time spent iterating existing feature-products
2. Define what the new system needs to do and by when
3. Start measuring performance
4. Start building the new system without syncing back data to CMS

### Solution #1. Drastically reduce time spent iterating existing feature-products

The programme has spent three years building feature-products. 

The real value of that work is not the products that shipped — it's what we've learned: about the technology, about user needs, about where the hard problems actually are. 

Stopping doesn't mean throwing that away. 

It means redirecting the investment into building something users can actually do their whole job in, rather than another disconnected piece they can only do one task in.

### Solution #2. Define what the new system needs to do and by when

Getting scope right determines whether the new system can actually replace CMS — but a well-scoped system can still fail to get adopted, and adoption is what actually replaces CMS, not the existence of working software.

Users don't drop a legacy system because a replacement exists. They drop it once the replacement is good enough that the alternative — falling back to CMS, or inventing a workaround — is worse. 

If users won't use it, none of the rest follows: no real usage data, no evidence of benefit, no case for expanding scope, and no advocacy from the people whose teams are using it.

Users matter, but they aren't the only people who decide whether a new product survives. 

Team leads and ops managers need to be convinced their team won't be made less productive by adopting something incomplete. That needs:

- Evidence, qualitative or quantitative, that it's working
- A clear benefits case — for example, time saved
- A rollout and scale-up plan they've actually agreed to, not just been told about
- A defined fallback
- An understanding of what they're worried about, and a direct answer to it

To make scope a deliberate decision and give team leads something real to evaluate, this needs producing alongside the build, not as an afterthought.

Here are some ideas to consider (perhaps we have some of these already):

- A service map of the complete experience, as-is and to-be — with the as-is map capturing pain points and opportunities that feed directly into the to-be
- Process maps, as-is and to-be
- Event storming diagrams
- Scenarios for both happy (if one exists) and unhappy paths
- A clear problem statement
- A list of user needs, marked with which are validated and which have actually been delivered against — those are two different things
- A roadmap to launch written in terms of user outcomes, not features and functionality; include external interfaces and compatability with non-functional requirements
- A prioritised backlog written in terms of user needs, not functionality
- A gap analysis: given the priority journeys and scenarios, what's missing between what's designed, what's built, and what users actually need
- Continuous research and usability testing before and throughout the build, so the product reflects what users actually need — not what CMS currently gives them, or what we assume they need

What this could mean in practice:

- Make leaving CMS safe by scoping, not by feature completeness alone. A slice is ready to go live when it's complete for the case types and cohort it's scoped to, including their edge cases, not when it covers the happy path for everyone. If a case type genuinely can't be handled yet, it stays on CMS until it can, rather than being let in and then abandoned halfway through.
- Roll out by case type and cohort — start with one case type, one unit, even one user, running end-to-end in the new system with no CMS fallback, then widen. Once a scope is fully on the new system, those users stop using CMS for that work. If users are always split across both systems, CMS never becomes redundant and we never reach the point where we can decommission it.

The BeyondCMS team already has a prototype deliberately unconstrained by CMS where everything a user does lives in one coherent product. 

It's not real yet, and it isn't evidence that it already delivers the outcomes CPS needs — that depends on the case quality standards and procedural requirements we're still defining. What it shows is that the approach itself — one coherent product, unconstrained by CMS — is buildable, and gives us a starting point to iterate on going forwards.

### Solution #3. Start measuring performance

Without measurement, it's too easy to mistake activity for value.

Before rollout begins, we must define our performance metrics and baseline where we are now.

For example:

- Time to make a charging decision
- Number of times a reviewing lawyer has to open a case
- Compliance with statutory and procedural deadlines
- Increase the number of guilty pleas at first hearing — so cases resolve earlier and don't load courts unnecessarily
- Adoption rate — proportion of eligible cases and users fully on the new system vs. still on CMS, by cohort, case type and by when

We should then work explicitly to these metrics — and treat benefits as realised once we can see them in the data, not before.

When those indicators exist, lack of progress becomes visible and hard to ignore. 

Without them, a programme can keep describing itself as on track while users haven't moved off CMS, adoption stays flat, and no one can say with confidence whether the work is delivering value.

Measurement also gives team leads and ops managers something concrete to evaluate — not just a demo or a promise, but evidence.

### Solution #4. Start building the new system without syncing back data to CMS

Our recommendation is to not sync back to CMS.

The new system is independent and means the experience can completely and easily diverge from CMS.

We have considered two alternative options which we don’t think we should do

#### Option #1: Continuous sync (and the problems this has)

Legacy and new are kept in sync on every user action using a translation layer — code that maps actions in the new system back into the old data model. 

This is what we're doing today: a user's actions in a feature-product get written back to CMS as they happen, so they can drop back to CMS at any point.

The problems are:

- It’s complicated
- It’s a significant cost
- It signicantly increases the time to deliver the work
- It increases residual transitional architecture and tech debt
- Leads to continued reliance on dual systems

This creates a culture of focusing on shipping the smallest improvement, rather than considering the bigger picture of tech and UX.

Ultimately, users keep one foot in CMS because the new system is built to stay compatible with CMS, not to solve the whole problem.

#### Option #2: Sync at certain times (and the problem this has)

This is similar to option 1, but syncing less often — for example, a user might review a case, review it again, then start setting up a hearing, and the sync back to CMS only happens at that last step. 

After that point, they can return to CMS; before it, they can't.

This means that theoretically, after that point, users could continue using the old system, just from that point onwards.

But the problems are similar to option 1:

- It’s complicated
- It’s a significant cost
- It signicantly increases the time to deliver the work
- It increases residual transitional architecture and tech debt
- Leads to continued reliance on dual systems

And this option also depends on being able to draw a clean line between what the user can do in the new system and what CMS still owns. 

Based on existing understand we don’t think casework has clean boundaries — it's one continuous journey with nested sub-journeys.

#### Recommendation: no write-back

We intercept messages at source, feeding new cases into the new system as they're created or become eligible, but nothing ever flows back.

This would be the only transitional architecture. There's no translation layer, no writing to two systems at once. 

The new system is still built using continuous delivery and tested continuously with real users throughout, but a case isn't worked live in the new system until a given scope of it is good enough to stand on its own, with no fallback to CMS needed. If users do need a workaround that the new system cannot do, we derisk that by scaling slowly and helping users do that. This should influence our roadmap.

This has the lowest complexity and the lowest risk to data integrity, because there's only ever one copy of the truth. 

There's no sync code to maintain once a scope is live, and development is significantly faster and cheaper. 

#### “But isn’t this a big bang waterfall approach to delivery?”

No. 

Whatever we design and build is continuously delivered, iterated and tested in user research throughout. 

And rather than waiting until everything is done before anyone uses it, we start with the smallest pilot that lets us learn and de-risk as we scale.

In practice: we pick a case type where we have confidence the new system is ready for it, start with the smallest group of users, and have a plan for when things go wrong. For example:

- a developer available to migrate a case back into CMS if needed
- we create ‘support consoles’ to help run the new process if needed

From there, we widen scope — more case types, more users — as each slice proves itself.

There's a deeper flaw in the "roll out early" argument too:

It assumes rollout equals adoption.

Which as explained above, our research shows it doesn’t.

The worst case under a sync approach isn't users falling back to CMS. It's that they never leave it. 

We've been calling that a rollout.

What this means in practice:

Extend the data model once, in the new system. 

New fields or entities belong to the new system's data model only, never split between CMS and the new system. 

Don't keep CMS and the new system in sync to allow fallback — scope who's allowed into a piece of work instead, so fallback is never needed for the people using it.

## Open questions

1. Is there a real case for Option 2 that this document is missing, or does cohort and case-type scoping under Option 3 make it redundant? Worth testing with the programme's solution architects rather than settling it here.
2. Which case type and area make the best first slice, what does "working" look like before we expand to the next one, and who's the willing partner unit? The simplest case type with the fewest moving parts reduces risk while the approach is unproven. Running two units in parallel from the start is one way to guard against building something too tailored to just one unit's way of working.
3. Governance — who has the authority to say no to a team launching another disjointed feature-product, and on what grounds?
4. What's already true today that this would mean unwinding — the feature-product structure, the global nav model, any roadmaps already committed to the sync pattern?
5. How do we communicate this to teams currently mid-build on disparate features or on sync infrastructure, without halting useful work outright?
6. Which of the evidence assets in "Defining what we build and for whom" do we already have in some form, and which need creating from scratch before we can responsibly scope a first slice?
7. Who are the right team leads and ops managers to bring into the rollout plan for the first pilot, and what are their likely fears?
8. What is the longer term plan for where data is held?

