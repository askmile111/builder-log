builder-log
Public logbook of how I'm building a demand intelligence pipeline — a system that surfaces monetizable pain points from online conversations and turns them into actionable product opportunities.

The pipeline (two stages)
Stage 1 — Data collection & pre-filtering

Source: Hacker News Ask Stories (Firebase API, no authentication, no rate limits)

Fetches posts with full comment threads (10 comments per post)

Merges post body and comments into a single content block for analysis

Output: raw_posts.json

Stage 2 — Local LLM deep analysis

Runs on Qwen 7B via Ollama (zero API cost, completely offline)

Extracts from each post:

3 concrete, specific pain points

Scores: pain intensity, urgency, willingness to pay (1–10 each)

Product adaptability (0/1)

Product title suggestion

One-line selling point (copy-ready for Gumroad/Twitter)

Decision: IGNORE / BUILD (adaptability = 0/1)

Key insight added today: High pain score ≠ high product fit. A post can score 31/40 (high pain) but be unadaptable (complaint about a proprietary service), while a 26/40 post (decision fatigue between tools) is perfectly adaptable. I'm adding a "solutionizability" dimension to future prompts.

What's working
End-to-end pipeline from HN Ask → scored JSON in ~3 minutes (manual trigger)

100% comment capture rate (15/15 posts today)

Copy‑ready output (pain points + product title + selling point)

Zero external API costs (local Qwen 7B on RTX 4090)

What needs work
~50% false positive rate – AI over‑optimizes for "pain" and under‑optimizes for "sellable"

No feedback loop yet – the system doesn't know which "BUILD" posts actually convert

Occasional hallucination (e.g., "pet programmer" suggestion from unrelated post)

Log entries
File	Stage	One sentence
How-to-Find-Real-Problems-Worth-Solving-on-Reddit.md	Discovery & evaluation	Building the original two-stage filter concept on Reddit (now superseded by HN)
What-a-9k-Founder-Taught-Me-About-Demand-Validation.md	Evaluation	Adding cross-platform signals (G2, Upwork) to strengthen BUILD decisions
instant-pricing-intake.md	Action	Designing a pricing format for manual demand analysis reports
2026-07-01-local-ai-demand-pipeline.md	Pivot & build	Switched from Reddit to HN, integrated Qwen 7B, achieved working pipeline with structured pain point extraction
What this is
A raw, unpolished record of a solo builder figuring things out in public

Some files are newly written; others are older work cleaned up and archived here

No products, no landing pages, no waitlists — just an engineering logbook

What's next
Run 30 posts manually label every output (build a truth dataset)

Add "solutionizability" dimension to Stage 2 prompt

Build feedback_loop.py to auto‑tune scoring weights based on actual conversion data

Product generator: accept a BUILD post → output full Notion template ready for Gumroad

Last updated: 2026-07-01

