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

1. Drastically reduce the time spent building existing feature-products
2. Define the product scope to be delivered by 2030
3. Start building the new system without syncing back data to CMS




This isn't new territory. Replacing a live legacy system while keeping it running is a well-studied problem, and there are essentially three standard strategies for handling data while the migration is in progress. Each has known costs, and the industry has names for the failure modes.

### Option 1: Continuous sync

Legacy and new are kept in sync on every user action, usually through an anti-corruption layer (ACL) and/or change data capture (CDC) - patterns from Domain-Driven Design for translating between two different models without one corrupting the other. This is what we're doing today: a user's actions in a feature-product get written back to CMS as they happen, so they can drop back to CMS at any point.

It enables early and frequent rollout, and it surfaces edge cases and business complexity early, because users hit them straight away. But it's a textbook case of what's known as the dual-write problem: writing the same fact to two systems with two different models is inherently lossy and error-prone, and the risk applies to every single operation, forever, for as long as both systems are live. It also roughly doubles the build: every feature has to work in the new system and be translated back into a model that wasn't designed for it. Velocity drops, transitional architecture and cleanup work outlive the migration, and the user experience itself reveals the compromise - it's built to stay compatible with CMS, not to solve the whole problem, so users keep one foot in CMS and adoption stays low.

This is vertical slicing: delivering individual features across the whole system, for every case type, rather than a complete journey for one case type at a time. Continuous sync is the price of slicing this way - if a feature only exists for some case types, every user needs a working fallback to CMS for the rest, so the two systems have to stay compatible. A concrete example: a charging manager needs to find cases ready to assign after triage, but "ready to assign" isn't a concept the legacy data model has. It only exists as an inferred state across several task types, filtered one unit at a time - users have learned to navigate that because they understand the internal system mechanics. Recreating that workaround in the new system, just to stay in sync with CMS, means shipping a modern interface on top of the same broken workflow.

This is the deeper cost of continuous sync: the new system's UX can only be as good as CMS's data model allows. You can redesign the interface, but you can't redesign the underlying model it has to conform to - so the problems that come from that model, missing statuses, inferred states, workarounds baked into the data structure, carry over wholesale into the new system.

### Option 2: Sync at capability boundaries

The same idea, but syncing less often - for example, a user might review a case, review it again, then start setting up a hearing, and the sync back to CMS only happens at that last step. After that point, they can return to CMS; before it, they can't.

This depends on being able to draw a clean line, what Domain-Driven Design calls a bounded context, between what the new model owns and what CMS still owns. As the previous section sets out, casework doesn't decompose into clean boundaries like that - it's one continuous journey with deeply nested sub-journeys. Option 2 inherits Option 1's core risks - lossy translation, users stuck mid-task - at a coarser grain, while still requiring most of Option 1's sync infrastructure to build and run. It isn't a different strategy so much as a less frequent version of the same one, and it's not obvious it meaningfully reduces the risk or the cost.

### Option 3: No write-back

Sync only runs one way: legacy feeds cases into the new system as they're created or become eligible, but nothing ever flows back. There's no ACL, no CDC, no dual-write. The new system is still built using continuous delivery and tested continuously with real users throughout, but a case isn't worked live in the new system until a given scope of it is good enough to stand on its own, with no fallback to CMS needed.

This has the lowest complexity and the lowest risk to data integrity, because there's only ever one copy of the truth. There's no residual transitional architecture or sync code to maintain once a scope is live, and development is genuinely greenfield, which is faster than building the same thing twice. Taken completely literally though - wait until every capability is built and validated, for everyone, before anyone uses any of it - this is a big bang migration, which is exactly the high-risk pattern that incremental approaches like the strangler fig pattern exist to avoid. For a system this size, a true big bang is a bigger risk than Options 1 or 2, not a smaller one.

### How Option 3 avoids becoming a big bang: horizontal slicing

The way out of that trap is to scope by case type and by user cohort, not by capability alone - a horizontal slice, as opposed to the vertical slicing behind Options 1 and 2. Instead of gating rollout on "all capabilities, validated, for everyone," gate it on "this capability set, validated, for this case type, for this cohort of users." A pilot of one user in one unit, handling one type of case end-to-end in the new system with no CMS fallback, is a real, low-risk cutover for that slice — low-risk because the scope is small, not because removing the fallback is without cost. If there are gaps in that slice, the escape route is manual — a user has to reconstruct their work in CMS — which makes getting the scope right before cutover more important, not less. This is the strangler fig pattern correctly applied, just sliced by case type and cohort instead of by feature.

This is also how "replace journeys, not features" survives the fact that casework is one big journey, not many independent ones. It doesn't mean delivering the entire case lifecycle before anyone can use the new system. It means each slice - a case type, a cohort, a piece of the overarching journey - is genuinely complete for its own scope before it goes live, with no dual-write and no fallback required for users inside that scope. The scope widens over time, and CMS gets switched off for that scope as it goes.

Seen this way, Options 1 and 2 don't automatically buy earlier rollout than Option 3. Case-type and cohort scoping can get a pilot user live just as early as a partial rollout under Options 1 or 2 — without taking on dual-write risk to do it. How early depends on how long it takes to reach "complete for a case type," which is the open question this approach puts pressure on. But the main argument for Options 1 and 2 — "at least we can roll out early" — holds up a lot less well once cohort scoping is on the table as an alternative way to get there.

### Common components, not common products

The Ministry of Justice's HMPPS probation service (MPOP, Managing People on Probation) has already worked through a version of this problem. They treat their case system as a platform, not a single product: journey-level products like induction, managing unpaid work, and remote check-ins are separate from each other, but they all sit on shared common components - setting up an appointment, reviewing tasks after a check-in, updating contact details, marking someone as no longer on probation - built once and adapted, not rebuilt per product.

That creates a real tension, and it's worth naming rather than pretending it away. A component built to work for one product is rarely exactly right for the next one without adaptation, and building for "every product at once" up front tends to produce something generic that fits none of them well. MPOP's answer is to pick one product, build the common components it needs, validate them with real use, then adapt and extend for the next one - rather than either building every component generically up front, or rebuilding each component from scratch per product.

MPOP also doesn't treat "more than one product" as automatically bad UX, but it's clear-eyed about the cost: ten well-designed products is still ten things a user has to learn, switch between and remember to use for their day job. Their stance is that building bigger, journey-level products beats building lots of small ones, for the same reason nobody would want separate services to request holiday, log sick leave and review their team's requests. This sharpens the argument this document already makes about feature-products: even legitimate, well-built products can recreate the same problem if there end up being too many of them for one job.

This also sharpens the transition approach set out above. The first slice shouldn't just be the simplest case type with the fewest moving parts - it should also be the one that exercises the most common components a future product would need, so that what gets built and validated first carries forward, rather than needing to be redone when the next slice arrives.

## What this means in practice

1. Build on one shared platform, not a portfolio of disconnected feature-products. Whether that ends up being a single product or a small number of journey-level products, they all need one data model, one design system and one set of architectural rules underneath, and they need to add up to one coherent experience for the user's job - not feel like separate tools to bookmark.
2. Extend the data model once, in the new system. New fields or entities, like notes, belong to the new system's data model only, never half in CMS and half in a feature-product. Don't keep CMS and the new system in sync to allow fallback - scope who's allowed into a piece of work instead, so fallback is never needed for the people using it.
3. Make leaving CMS safe by scoping, not by feature completeness alone. A slice is ready to go live when it's complete for the case types and cohort it's scoped to, including their edge cases, not when it covers the happy path for everyone. If a case type genuinely can't be handled yet, it stays on CMS until it can, rather than being let in and then abandoned halfway through.
4. Decommission as we go, per scope. Every slice that moves fully into the new system means CMS is switched off for that case type and cohort, not left running in parallel indefinitely. If CMS keeps doing everything for everyone, it never shrinks, and we never reach the point where we can turn it off.
5. Roll out by case type and pilot cohort, not by feature flag. Start with one case type, one unit, even one user, running that work end-to-end in the new system with no CMS fallback, then widen the scope - rather than opening individual disparate features to everyone and hoping people gradually give up on CMS once enough of them exist.

## Earning adoption

Everything above is necessary but not sufficient. A scoped, well-architected slice can still fail to get adopted, and adoption is what actually replaces CMS - not the existence of working software.

### Why users abandon an MVP

Users don't drop a legacy system because a replacement exists. They drop it once the replacement is good enough that the alternative - falling back to CMS, or inventing a workaround - is worse. Common reasons an MVP isn't good enough, even when it's technically working:

- It's hard to understand
- Users get left in dead ends
- Users weren't told what it does and doesn't do, so they expect more than it delivers
- Users don't know what to do when they're stuck
- The old system and process feel easier, even though they're worse
- Users have to enter the same data more than once

If users won't use it, none of the rest follows: no real usage data, no evidence of benefit, no case for expanding scope, and no advocacy from the people whose teams are using it.

### Team leads and ops managers are a second audience

Users matter, but they aren't the only people who decide whether an MVP survives. Team leads and ops managers need to be convinced their team won't be made less productive by adopting something incomplete. That needs:

- Evidence, qualitative or quantitative, that it's working
- A clear benefits case - for example, time saved
- A rollout and scale-up plan they've actually agreed to, not just been told about
- A defined fallback
- An understanding of what they're worried about, and a direct answer to it

Without this, even a good MVP can get killed by a team lead who refuses to let their team use it, or quietly tells them not to bother.

### What this requires before rollout

- An evidence-based understanding of the service as a whole, not just the slice being built - including which scenarios are common and which are edge cases, so scope is a deliberate decision, not a guess
- A defined way for a user to still finish the journey if they hit something the new system can't do yet - fall back to CMS, contact support, whatever it is - decided in advance, not discovered by a stuck user
- Transparent communication with users about what the product does and doesn't do, when they'll see change, and how feedback gets acted on
- Strong links with early users, so the MVP's scope gets validated against real people before wider rollout, not just against assumptions

## Evidence we need before scoping

To make scope a deliberate decision rather than a guess, and to give team leads something real to evaluate, this needs producing alongside the build, not as an afterthought:

- A service map of the complete experience, as-is and to-be, not just products and features - with the as-is map capturing pain points and opportunities that feed directly into the to-be
- Process maps, as-is and to-be
- Scenarios for both happy and unhappy paths
- A clear problem statement
- A list of user needs, marked with which are validated and which have actually been delivered against - those are two different things
- A performance framework: what we're trying to improve, by how much, by when, and how we'll measure it
- A roadmap to MVP launch written in terms of user outcomes, not features and functionality
- A prioritised backlog written in terms of user needs, not functionality
- A gap analysis: given the priority journeys and scenarios, what's missing between what's designed, what's built, and what users actually need

## A working illustration

This isn't just theoretical. The prototype this team has been building (`prosecute-a-case-prototype`) is a deliberately unconstrained redesign of CMS, not another feature-product alongside it. Tasks, notes, charging decisions, documents, witnesses, hearings and case reviews all live inside one case-centred information architecture, under a single navigation model, rather than as separate feature-products stitched together by a global nav.

It's a prototype, not a built service, and it doesn't yet cover every journey CMS does. But it shows what "one coherent product, designed end-to-end" looks like in practice, and it's a concrete starting point for the kind of system this document argues for, rather than something we'd be starting from scratch.

## What needs to be true to deliver this

- The new system is built as a single product on one shared platform, not as a collection of separate feature-products
- We have the evidence assets needed to make scope a deliberate decision, not a guess - see Evidence we need before scoping
- Team leads and ops managers have agreed a rollout plan, with a defined fallback, before any pilot starts
- We agree what "complete" means for a scope - a case type and cohort - before it ships, rather than for the whole system at once, so a thinner version doesn't get treated as done because something exists, and so we're never waiting for everything before anything goes live
- We have a way to choose the first case types and pilot cohorts that gets real usage early without taking on dual-write risk
- Sync into the new system is one-way only - legacy feeds cases in as they're created or become eligible - with no path back out
- Decommissioning runs in parallel with delivery, per scope, not after everything is built. Each scope we finish needs an explicit "now switched off in CMS for these case types and users" milestone, not just a "now available in the new system" milestone

## Anticipated objections

### "We've already built feature-products - isn't this throwing away three years of work?"

Changing direction doesn't throw away three years of work. It carries forward three years of learnings - about the problem domain, about what users actually need, about what works technically and what doesn't - while stopping short of compounding the mistake by continuing down a path the evidence says won't replace CMS.

The feature-products that exist tell us what the important scenarios are, where the edge cases live, and which parts of the data model need to be richer than CMS's. That's valuable input into designing the first horizontal slice, not wasted effort. What changes is where that knowledge gets applied: into building something users can actually complete their whole job in, rather than another disconnected piece they can only do one task in.

### "What if the first slice only works for one case type or unit?"

That's the point. Starting with one case type, one unit, even one user, keeps the risk small and the feedback real. If something doesn't work, the impact is limited to that unit, not the whole system. And most units handle cases in broadly similar ways, so a complete system for one unit is likely to cover most of what other units need too.

Running two units in parallel from the start is one way to guard against over-fitting to just one unit's specific way of working. By the time you've expanded across several units, you've built something that works for everyone - and each expansion is significantly easier than the first, because the platform and common components are already there.

### "Won't building end-to-end for one slice first take longer?"

Yes, upfront. But the current path is not actually fast. Every feature-product carries the cost of bidirectional sync, legacy schema constraints and integration testing across two systems - and that cost compounds with every new one added. A horizontal slice removes it entirely.

And once a complete system works for one unit, expanding to the next is significantly easier than starting from scratch. Slow down now to go faster later. The prototype already gives us a solid base to build from, not a blank page.

## Open questions

1. Is there a real case for Option 2 that this document is missing, or does cohort and case-type scoping under Option 3 make it redundant? Worth testing with the programme's solution architects rather than settling it here.
2. Which case type and area make the best first slice, what does "working" look like before we expand to the next one, and who's the willing partner unit? The simplest case type with the fewest moving parts reduces risk while the approach is unproven. Running two units in parallel from the start is one way to guard against building something too tailored to just one unit's way of working.
3. Governance - who has the authority to say no to a team launching another disjointed feature-product, and on what grounds?
4. What's already true today that this would mean unwinding - the feature-product structure, the global nav model, any roadmaps already committed to the dual-write pattern?
5. How do we communicate this to teams currently mid-build on disparate features or on sync infrastructure, without halting useful work outright?
6. Which of the evidence assets in "Evidence we need before scoping" do we already have in some form, and which need creating from scratch before we can responsibly scope an MVP?
7. Who are the right team leads and ops managers to bring into the rollout plan for the first pilot, and what are their likely fears?
