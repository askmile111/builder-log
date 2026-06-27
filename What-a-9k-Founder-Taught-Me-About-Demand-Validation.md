# What a $9k/mo Founder Taught Me About Demand Validation

_Day 002 — 2026-06-27_

## What I'm solving
My two-stage pipeline is technically functional, but it only looks at Reddit. I need to know: can I make Stage 2 decisions sharper by pulling in signals from *outside* Reddit? Today I dissected a founder's $9k/mo playbook and found three concrete upgrades for my system.

## The source
A founder posted a detailed 30-day restart plan on r/SaaS. He built a product from zero to $9k/month with 700 paying users. His core thesis: "The best SaaS ideas in 2026 are complaints that keep repeating across multiple platforms from people already spending money on bad solutions."

His method has three layers I haven't integrated yet.

## Layer 1 — Cross-platform signal triangulation (upgrades Stage 2)
**What he does:** Takes the top 3 complaints from G2/Capterra 1-2 star reviews, then searches Reddit for the same frustrations. A complaint only counts if it appears on *both* G2 AND Reddit AND App Store reviews.

**What my system does now:** Stage 2 scores a single Reddit post in isolation.

**Upgrade:** Add a `cross_platform_signal` boolean to Stage 2. If a Reddit post mentions a pain point that also appears frequently in G2 negative reviews, boost the BUILD decision weight. This filters out "Reddit-only complaints" that don't generalize.

## Layer 2 — Upwork as a paid-demand oracle (new Stage 2 dimension)
**What he does:** Checks Upwork for completed jobs where companies repeatedly hire freelancers to do the same manual task. "Repetitive tasks being outsourced = someone is literally paying humans to do something software could handle."

**What my system does now:** Stage 2 has no Upwork dimension.

**Upgrade:** Add `upwork_signal` to Stage 2. For each BUILD candidate, search Upwork for related recurring tasks. If the same task appears across multiple completed contracts, it's not just a Reddit complaint — it's a verified paid demand signal.

## Layer 3 — Pricing validation via cold DMs (validates Step 4)
**What he does:** Finds 10 people who left negative reviews or posted complaints, DMs them directly: "Hey, I saw you mentioned X problem. I'm thinking about building something that fixes this. Would you pay $30-50/month?" If 3/10 say yes, he builds.

**What this means for my Step 4:** My "$49 manual analysis" pricing is in the right ballpark. His $30-50/month for a SaaS translates to roughly $49 for a one-off diagnostic report. And his 3/10 conversion benchmark means I don't need 100 people on a waitlist. I need 10 DMs, 3 yeses, and I'm validated.

## What I built today
- Dissected the $9k/mo founder's 30-day playbook into three transferable layers.
- Drafted two new Stage 2 dimensions: `cross_platform_signal` (boolean) and `upwork_signal` (boolean).
- Validated my Step 4 pricing: $49 for a manual analysis report is in the right zone.
- Identified that my current pipeline is "Reddit-only" — this is a structural weakness I need to fix before scaling.

## Where I'm stuck
- I don't have a G2 review scraper. Adding this as a data source will require new code, not just a prompt tweak.
- Upwork signal detection is manual for now. I need to figure out how to query Upwork data programmatically without getting rate-limited.
- The 3/10 cold DM benchmark assumes the person knows how to write non-spammy outreach. I'm still learning this.

## Next
- Add `cross_platform_signal` and `upwork_signal` as optional fields in the Stage 2 prompt, even if they default to "unknown" for now.
- Manually check Upwork for 3 BUILD candidates from yesterday's batch. Do any of them have corresponding paid tasks?
- Write Day 003 on the content-idea detection angle: the pipeline's potential as a content inspiration engine for SaaS founders.