
_Day 001 — 2026-06-27_

## What I'm solving
I'm building a two-stage Reddit demand intelligence pipeline that uses rule-based scoring to surface high-signal posts from the noise, then enriches them with LLM-driven monetization analysis to identify actionable product opportunities.

## The model (two-stage filter)
**Stage 1 — Rule-based title scoring (local, zero cost)**
A Python function scores every Reddit title across four dimensions:
- Directness (question or explicit request?)
- Action/state (describes a concrete action or failure?)
- Scenario specificity (industry, tool, context mentioned?)
- Confusion/failure clarity (how clearly stuck is the person?)

Output: Tier A (≥70), Tier B (≥30), Tier C (discard).

**Stage 2 — LLM deep analysis (currently manual)**
I merge the title, body, and comments of Tier A/B posts and send them to ChatGPT with a structured prompt. The prompt returns:
- pain_strength, money_signal, switching_intent (0–10)
- competition_mentioned (true/false)
- workaround_intensity (0–10)
- final_score (0–100)
- decision: IGNORE | WATCH | BUILD
- mvp_direction: one-sentence product idea

## What I built today
- Wrote and tested the rule-based title scoring engine. It runs against Reddit RSS feeds (r/SaaS, r/Entrepreneur, etc.) and filters out most noise.
- Defined the second-stage LLM prompt focused on real pain, money signals, and switching intent.
- Manually merged the contents of several Tier A/B posts with their comments and sent them to ChatGPT. First results are promising.
- Saved candidate posts into a local JSON file (`daily_leads.txt`).

## Where I'm stuck
- The pipeline is still manual between stages. I need to script the merging of title+body+comments and automate the LLM call.
- Haven't decided on an alert channel yet (Telegram vs local file).
- The rule engine works, but I don't know yet how many false positives survive Stage 1. I'll only know after labeling enough Stage 2 outputs.
- Reddit rate limiting makes frequent comment fetching expensive. Currently, comment fetching is disabled by default.

## Next
- Run Stage 2 on at least 20 posts and manually verify: how many BUILD decisions are actually monetizable?
- Build a small script that automates the handoff from Stage 1 to Stage 2.
- Start a false-positive log to tune both stages.