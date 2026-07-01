# Day 003 — 2026-07-01

## I Finally Have a Working Market Validation System

Three days. That‘s how long it took to go from “I have no idea what to build” to “I have a system that tells me exactly what to build.”

Today I closed the loop on my demand intelligence pipeline. It’s not perfect, but it works. Here‘s exactly what I built and what I learned.

---

## The Problem I’m Solving

Every founder I know struggles with the same question: **“What do I build?”**

We guess. We build. We fail. We repeat.

I wanted a system that eliminates the guesswork—something that scrapes the internet for real, unfiltered customer pain points, filters out the noise, and tells me exactly where to put my energy.

Today, that system exists.

---

## What I Built (Technical Summary)

**Two-stage pipeline architecture:**

### Stage 1 — Data Collection

- Source: Hacker News Ask Stories ([firebase.io](https://firebase.io/) API)
    
- No API key required. No rate limits. No authentication headaches.
    
- Fetches 15 posts per run with full comment threads (10 comments each)
    
- Output: `data/raw_posts.json` with title, body, and all comments merged
    

### Stage 2 — AI Analysis

- Local Qwen 7B model running via Ollama
    
- Zero API cost. Completely offline.
    
- Processes each post and extracts:
    
    - 3 specific, concrete pain points (not generic labels)
        
    - Pain intensity score (1-10)
        
    - Urgency score (1-10)
        
    - Willingness to pay score (1-10)
        
    - Product adaptability score (0 or 1)
        
    - Product title suggestion
        
    - One-line selling point
        

**Total time from raw post to scorable opportunity: ~3 minutes.**

---

## The Data That Actually Worked

For three days I kept hitting walls with Reddit. Then I switched to Hacker News and everything clicked.

HN‘s Firebase API returns clean JSON with full comment threads. No authentication. No rate limits. Just data.

Here’s what a processed post looks like now:

json

{
  "title": "Ask HN: Homeless, Former Software Developer, What Now?",
  "score": {
    "归纳的痛点": [
      "技术断层焦虑（2023年后未接触AI，无法通过面试）",
      "宠物羁绊（2只狗，1只未绝育，无钱手术）",
      "资金链断裂（剩$500，付不起租房押金）"
    ],
    "产品建议标题": "《程序员破局指南：6周从流浪到重返职场》",
    "一句话卖点": "专为有宠物、资金紧张的开发者设计，含AI速成+宠物安置方案"
  }
}

This is **copy-ready content**. I could paste this directly into a Gumroad product page. No rewriting needed.

---

## The Critical Insight I Had Today

**High pain scores do NOT equal high product fit.**

My system flagged two posts today:

|Post|Score|Adaptability|Why|
|---|---|---|---|
|“Is Codex with GPT 5.5 being dumbed down?”|31/40|❌ 0|Complaint about a proprietary service (can‘t fix OpenAI)|
|“Is aerc better than neomutt?”|26/40|✅ 1|Decision fatigue between two tools (CAN sell a comparison guide)|

**The distinction:** One problem can be solved with a PDF. The other can't.

I need to add a "solutionizability" dimension to my scoring—whether a pain point can actually be addressed through digital products like guides, templates, or checklists. Just measuring pain intensity isn't enough.

---

## What’s Actually Working

**1. The comment fetcher is finally solid**

Comments are essential. The body of a post tells you the headline. The comments tell you the real story—the specific obstacles, the failed attempts, the frustration. My system now merges both.

**2. The local model is fast enough**

Qwen 7B via Ollama takes about 15 seconds per post. With my RTX 4090 it runs comfortably. For 15 posts, that‘s ~3.7 minutes total inference time.

**3. The output is immediately usable**

The “痛点” (pain points) extraction gives me 3 concrete, actionable pain points per post. No fluff. No generalities.

---

## What’s Still Broken

**1. Qwen occasionally hallucinates**

Aerc vs. Neomutt was correctly flagged as an opportunity. But the AI added “pet programmer” to the suggestion, which came from a completely different post earlier in the batch. Prompt context bleed. Still debugging this.

**2. No feedback loop exists yet**

The system outputs “BUILD” recommendations, but has no idea if those products actually sell. I need to manually track conversions and feed them back.

**3. False positive rate is ~50%**

Today's 15 posts gave me 4 opportunities, but I'd only actually build 2 of them. The AI over-optimizes for “pain” and under-optimizes for “sellable.”

---

## What I’d Do Differently

1. **Never start with Reddit.** Two days wasted on API applications, 403 errors, and workarounds. HN was always the right source.
    
2. **Start with a smaller prompt.** The first version asked for 7 fields, which broke Qwen's output formatting. 3 fields first, then expand.
    
3. **Skip the Python `ollama` library entirely.** `subprocess` is slower but 100x more stable. No HTTP 502s. No port conflicts. No session timeouts.
    

---

## Next Steps

**Immediate (next 24 hours):**

- Run 30 posts through the current system
    
- Manually label every output in a CSV
    
- Document: title | score | AI fit | my judgment | notes
    

**Week 1:**

- Add “solutionizability” dimension to the prompt
    
- Build a simple feedback script to log which posts actually convert into sales
    

**Week 2 (if feedback loop works):**

- Build `feedback_loop.py` to auto-tune scoring weights
    
- The system should learn: “X category posts never convert. Lower their weight.”
    

**Week 3:**

- Product generator—accept a BUILD post, output a full Notion template ready for Gumroad
    

---

## Raw Log: Today's Run

text

Posts processed: 15
Posts with comments captured: 15 (100%)
Average comments per post: 8.2
AI-flagged opportunities: 4
Opportunities I'd actually build: 2
False positive rate: ~50%
Bottleneck: Qwen inference (~15 sec/post)

---

## The Bottom Line

> The system now generates product ideas faster than I can validate them.

This is both the success condition and the next bottleneck.

Idea generation is solved. Validation speed is now the constraint.

Day 4 will be about building the conversion tracking that feeds back into the scoring system. If I can close that loop, I‘ll have what I originally set out to build: an autonomous demand intelligence engine that tells me exactly what to build and when to build it.