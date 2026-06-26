# builder-log

Public logbook of how I'm building a **Reddit demand intelligence pipeline** — a system that surfaces monetizable pain points from online conversations and turns them into actionable product opportunities.

## The pipeline (two stages)

**Stage 1 — Rule-based title scoring**
A local engine scores Reddit posts on directness, action, scenario specificity, and confusion clarity. Posts that pass the threshold move to the next stage. The rest are discarded.

**Stage 2 — LLM deep analysis**
Surviving posts are analyzed for real pain, money signals, switching intent, workaround intensity, and competition. The output is a decision: IGNORE, WATCH, or BUILD — plus a one-sentence MVP direction.

## Log entries

| File | Stage | One sentence |
|------|-------|--------------|
| `How to Find Real Problems Worth Solving on Reddit.md` | Discovery & evaluation | Building the two-stage filter: rule-based scoring → LLM monetization analysis |
| `instant-pricing-intake.md` | Action | Designing a simple pricing and delivery format for manual demand analysis |

## What this is

- A raw, unpolished record of a solo builder figuring things out in public
- Some files are newly written; others are older work cleaned up and archived here
- No products, no landing pages, no waitlists — just an engineering logbook