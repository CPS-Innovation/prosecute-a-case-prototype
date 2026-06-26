# How to replace CMS with something significantly better

## Executive summary

What we’re doing is making the replacement of CMS:

- slower to deliver
- ⁠⁠harder to adopt
- ⁠⁠more technically complicated than it needs to be

And as a result we’re not seeing better outcomes for CPS.

We need a different approach that: 

- prioritises delivering a whole service over individual pieces of functionality
- builds on a sound technical architecture from the start
- delivers real value as soon as possible

## The problem explained in more detail

The programme’s goal is to replace the legacy case management system (CMS). 

But for the last 3 years, we have not replaced CMS.

Here are the main reasons for that:

### Reason #1: We are adding to CMS, not replacing it — and making it harder to replace over time

The standard approach for replacing a live legacy system is the strangler fig pattern. 

A strangler fig is a vine that grows up around a host tree, gradually taking over its structural role, until the host can be removed and the fig stands on its own. 

Applied to software, you build a new system that does the job better, and migrate users to it as it becomes ready. Once all users can be moved to the new system, you can decommission the old one.

That's not what we are doing. At the moment, every new thing we build is directly connected to the old system.

This means what we design is constrained by what CMS can support. For example, Work Management, is essentially a reskin of the CMS task list. The new system’s interface can only be as good as CMS’s data model allows — you can redesign the screens, but not the underlying model. The same broken workflows carry over.

When we do extend the data model to do more than CMS, a notes field on a task, for example — it creates a fork between what CMS knows and what the feature-product knows. And users who work in CMS miss the information that exists in Work Management.[confirm notes as a real example, or substitute the correct one]

This makes it hard to turn off CMS in future, because all the new feature-products are intertwined with CMS as the system of record.

### Reason #2: How we are organised makes the problem worse and slows us down

We have teams for separate feature products — our term for a slice of CMS functionality built as its own system. For example:

- Housekeeping (document management)
- Work management (task list)
- Case search

But this is terrible user experience.

Users just have a goal they want to achieve, such as a review a case, and the journey involves all 3 of the above "feature products". Having them as separate products does not give users what they need.

This is something known as Conway’s Law.

The result is what’s called a feature factory: features get shipped because they are a unit of work that is easier to plan, fund and demo, without anyone owning whether those features add up to something users can actually do their whole job in. 

This goes against the GOV.UK Service Standard, which says to design and build the whole end-to-end journey a user needs to complete, not a screen, a feature, or a task at a time.

Working this way also makes us slower. 

Because CMS remains the system of record, every feature has to be built twice: once in the new system, and once translated back into a data model that was not designed for it. 

That significantly increases the time and cost for every feature, and the translation layer — the code that keeps the two systems in sync — adds up to a maintenance burden that may outlive the migration itself.

### Reason #3: Users don’t adopt what we build, so the work delivers little real value

Generally, users hate using multiple systems to do their job.

And this is true for CPS users. Research constantly shows this is the case. Switching between tools to do one job is a significant cognitive load. Users prefer to use one legacy system if it means using one thing.

Users and their managers need to be confident in the new product before they will switch. Our approach up to now does not give our users confidence, instead puts significant burden on them. They are already overworked.

This is because casework is one continuous journey. A case moves from arrival through review, charging decisions, hearings and outcome. Each of these things have sub journeys. For example, a user reviewing a case may simultaneously need to request information from the police, contact a victim, or update the defence. 

There is no clean seam in the middle of this that a feature product can cut at cleanly.

The value of what we build is conditional on adoption. 

The real value of the last three years is not the feature products that have shipped. It is what we have learned — about the technology, about user needs, about where the hard problems actually are. 

That learning is what we should build on, not our approach to delivery or the feature products themselves.

## What to do instead

1. Drastically reduce time spent iterating existing feature-products
2. Define the product scope to be delivered by which deadline
3. Start building the new system without syncing back data to CMS

### 1. Drastically reduce time spent iterating existing feature-products

The programme has already spent three years building feature-products. 

Stopping this doesn't mean throwing away everything — the existing work tells us what the important scenarios are, where edge cases live, and which parts of the data model need to be improved. 

That's valuable input into what comes next. 

What changes is where the investment goes: into building something users can actually do their whole job in, rather than another disconnected piece they can only do one task in.

### 2. Define the product scope to be delivered by which deadline

Getting scope right determines whether the new system can actually replace CMS — but a well-scoped system can still fail to get adopted, and adoption is what actually replaces CMS, not the existence of working software.

Users don't drop a legacy system because a replacement exists. They drop it once the replacement is good enough that the alternative — falling back to CMS, or inventing a workaround — is worse. 

If users won't use it, none of the rest follows: no real usage data, no evidence of benefit, no case for expanding scope, and no advocacy from the people whose teams are using it.

Users matter, but they aren't the only people who decide whether a new product survives. Team leads and ops managers need to be convinced their team won't be made less productive by adopting something incomplete. That needs:

- Evidence, qualitative or quantitative, that it's working
- A clear benefits case — for example, time saved
- A rollout and scale-up plan they've actually agreed to, not just been told about
- A defined fallback
- An understanding of what they're worried about, and a direct answer to it

Without this, even a good product can get killed by a team lead who refuses to let their team use it, or quietly tells them not to bother.

To make scope a deliberate decision and give team leads something real to evaluate, this needs producing alongside the build, not as an afterthought:

- A service map of the complete experience, as-is and to-be — with the as-is map capturing pain points and opportunities that feed directly into the to-be
- Process maps, as-is and to-be
- Scenarios for both happy and unhappy paths
- A clear problem statement
- A list of user needs, marked with which are validated and which have actually been delivered against — those are two different things
- A performance framework: what we're trying to improve, by how much, by when, and how we'll measure it
- A roadmap to launch written in terms of user outcomes, not features and functionality
- A prioritised backlog written in terms of user needs, not functionality
- A gap analysis: given the priority journeys and scenarios, what's missing between what's designed, what's built, and what users actually need
- Continuous research and usability testing throughout the build, so the product reflects what users actually need — not what CMS currently gives them, or what we assume they need

What this means in practice:

- Make leaving CMS safe by scoping, not by feature completeness alone. A slice is ready to go live when it's complete for the case types and cohort it's scoped to, including their edge cases, not when it covers the happy path for everyone. If a case type genuinely can't be handled yet, it stays on CMS until it can, rather than being let in and then abandoned halfway through.
- Decommission as we go, per scope. Every slice that moves fully into the new system means CMS is switched off for that case type and cohort, not left running in parallel indefinitely. If CMS keeps doing everything for everyone, it never shrinks, and we never reach the point where we can turn it off.
- Roll out by case type and pilot cohort. Start with one case type, one unit, even one user, running that work end-to-end in the new system with no CMS fallback, then widen the scope — rather than opening individual disparate features to everyone and hoping people gradually give up on CMS once enough of them exist.

For this to work:

- We agree what "complete" means for a scope — a case type and cohort — before it ships, rather than for the whole system at once, so a thinner version doesn't get treated as done because something exists, and so we're never waiting for everything before anything goes live
- Team leads and ops managers have agreed a rollout plan, with a defined fallback, before any pilot starts
- Decommissioning runs in parallel with delivery, per scope — each scope we finish needs an explicit "now switched off in CMS for these case types and users" milestone, not just a "now available in the new system" milestone

Starting with one case type, one unit, even one user, keeps the risk small and the feedback real. If something doesn't work, the impact is limited to that unit, not the whole system. And most units handle cases in broadly similar ways, so a complete system for one unit is likely to cover most of what other units need too. Running two units in parallel from the start is one way to guard against building something too tailored to just one unit's way of working.

The current approach is not actually fast: every feature-product carries the cost of keeping two systems in sync, and that cost compounds with every new one added. Once a complete system works for one unit, expanding to the next is significantly easier than starting from scratch.

This isn't just theoretical. The prototype this team has been building (`prosecute-a-case-prototype`) is a deliberately unconstrained redesign of CMS, not another feature-product alongside it. Tasks, notes, charging decisions, documents, witnesses, hearings and case reviews all live inside one case-centred information architecture, under a single navigation model, rather than as separate feature-products stitched together by a global nav.

It's a prototype, not a built service, and it doesn't yet cover every journey CMS does. But it shows what "one coherent product, designed end-to-end" looks like in practice, and it's a concrete starting point for the kind of system this document argues for, rather than something we'd be starting from scratch.

### 3. Start building the new system without syncing back data to CMS

There are three standard approaches to handling data during a migration. Understanding them explains why we're recommending the third.

#### Option 1: Continuous sync

Legacy and new are kept in sync on every user action using a translation layer — code that maps actions in the new system back into the old data model. This is what we're doing today: a user's actions in a feature-product get written back to CMS as they happen, so they can drop back to CMS at any point.

Writing the same fact to two systems with two different models is inherently lossy and error-prone, and the risk applies to every single operation for as long as both systems are live. It also roughly doubles the build: every feature has to work in the new system and be translated back into a model that wasn't designed for it. Velocity drops, the translation layer outlives the migration, and users keep one foot in CMS because the new system is built to stay compatible with CMS, not to solve the whole problem.

#### Option 2: Sync at capability boundaries

The same idea, but syncing less often — for example, a user might review a case, review it again, then start setting up a hearing, and the sync back to CMS only happens at that last step. After that point, they can return to CMS; before it, they can't.

This depends on being able to draw a clean line between what the new system owns and what CMS still owns. As the problem section sets out, casework doesn't decompose into clean boundaries — it's one continuous journey with deeply nested sub-journeys. Option 2 inherits Option 1's core risks at a coarser grain, while still requiring most of the same sync infrastructure to build and run. It isn't a different strategy so much as a less frequent version of the same one.

#### Option 3: No write-back

Sync only runs one way: legacy feeds cases into the new system as they're created or become eligible, but nothing ever flows back. There's no translation layer, no writing to two systems at once. The new system is still built using continuous delivery and tested continuously with real users throughout, but a case isn't worked live in the new system until a given scope of it is good enough to stand on its own, with no fallback to CMS needed.

This has the lowest complexity and the lowest risk to data integrity, because there's only ever one copy of the truth. There's no sync code to maintain once a scope is live, and development is genuinely greenfield, which is faster than building the same thing twice. Taken completely literally though — wait until every capability is built and validated, for everyone, before anyone uses any of it — this is a big bang migration, which is exactly the high-risk pattern that incremental approaches exist to avoid.

#### How Option 3 avoids becoming a big bang: horizontal slicing

The way out of that trap is to scope by case type and by user cohort — a horizontal slice. Instead of gating rollout on "all capabilities, validated, for everyone," gate it on "this capability set, validated, for this case type, for this cohort of users." A pilot of one user in one unit, handling one type of case end-to-end in the new system with no CMS fallback, is a real, low-risk cutover for that slice — low-risk because the scope is small, not because removing the fallback is without cost. If there are gaps in that slice, the escape route is manual — a user has to reconstruct their work in CMS — which makes getting the scope right before cutover more important, not less. This is the strangler fig pattern correctly applied, just sliced by case type and cohort instead of by feature.

This is also how "replace journeys, not features" survives the fact that casework is one big journey, not many independent ones. It doesn't mean delivering the entire case lifecycle before anyone can use the new system. It means each slice — a case type, a cohort, a piece of the overarching journey — is genuinely complete for its own scope before it goes live, with no writing to two systems and no fallback required for users inside that scope. The scope widens over time, and CMS gets switched off for that scope as it goes.

Options 1 and 2 don't automatically buy earlier rollout than Option 3. Case-type and cohort scoping can get a pilot user live just as early as a partial rollout under Options 1 or 2 — without taking on the cost of keeping two systems in sync. How early depends on how long it takes to reach "complete for a case type," which is the open question this approach puts pressure on. But the main argument for Options 1 and 2 — "at least we can roll out early" — holds up a lot less well once cohort scoping is on the table as an alternative way to get there.

What this means in practice:

- Build the new system on one shared platform, with one data model, one design system and one set of architectural rules — so it adds up to one coherent product users can do their whole job in, not a collection of separate tools.
- Extend the data model once, in the new system. New fields or entities belong to the new system's data model only, never split between CMS and the new system. Don't keep CMS and the new system in sync to allow fallback — scope who's allowed into a piece of work instead, so fallback is never needed for the people using it.

## Open questions

1. Is there a real case for Option 2 that this document is missing, or does cohort and case-type scoping under Option 3 make it redundant? Worth testing with the programme's solution architects rather than settling it here.
2. Which case type and area make the best first slice, what does "working" look like before we expand to the next one, and who's the willing partner unit? The simplest case type with the fewest moving parts reduces risk while the approach is unproven. Running two units in parallel from the start is one way to guard against building something too tailored to just one unit's way of working.
3. Governance — who has the authority to say no to a team launching another disjointed feature-product, and on what grounds?
4. What's already true today that this would mean unwinding — the feature-product structure, the global nav model, any roadmaps already committed to the sync pattern?
5. How do we communicate this to teams currently mid-build on disparate features or on sync infrastructure, without halting useful work outright?
6. Which of the evidence assets in "Defining what we build and for whom" do we already have in some form, and which need creating from scratch before we can responsibly scope a first slice?
7. Who are the right team leads and ops managers to bring into the rollout plan for the first pilot, and what are their likely fears?
