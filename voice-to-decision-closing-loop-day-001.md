# Voice‑to‑Decision Filter: Closing the Loop: How I Merged Voice Memos with a Decision Filter

_Day 001 — 2026-06-27_
## What I observed

- I now have two separate tools: **Idea Anchor** (voice recording + AI summarization) and **Idea Disposer** (4‑dimension scoring + decision filtering).
    
- They live in isolation. Users have to manually copy/paste content from one to the other — high friction.
    
- A typical user journey should be: **record → AI summarize → score → decide → (if action) start engineering log**.
    
- The current chain breaks between "AI summarize" and "score" — users need to copy text across.
    

## My current hypotheses

**H₀:**  
If I merge the two tools into a single unified interface, the number of steps from "recording" to "verdict" can be reduced from 6 to 3, and completion rate will increase measurably.

**H₁:**  
The structured output from AI (title + summary + keywords) provides better context for the 4‑dimension scoring, because users see a distilled version rather than raw voice fragments. This leads to more consistent scoring.

**H₂:**  
When the disposer returns "act", automatically generating a draft engineering log and storing it in localStorage — with a one‑click copy to Obsidian — lowers the activation energy from "I should do this" to "I start writing about it".

## Current model (v0.1 of unified flow)

**Unified data pipeline:**

text

Step 1: Record / input
   (Idea Anchor STT → raw_text)
        ↓
Step 2: AI summarize (real API or mock)
   → title + summary + keywords
        ↓
Step 3: 4‑dimension scoring
   Cost (C) / Scope (S) / Edge (E) / Urgency (U)
   ActionScore = C + S + E + U   (all 1‑5)
        ↓
Step 4: Verdict
   ≥16 → Act   |   ≥10 → Watch   |   <10 → Discard
        ↓
   ┌────┴────┐
   ↓         ↓
Save to     If "Act" → auto‑generate
timeline    engineering log draft
(localStorage)  (copy‑to‑clipboard ready)

**Unified data structure (TypeScript):**

typescript

interface UnifiedIdea {
  id: number;
  // from memo side
  rawText: string;
  aiTitle: string;
  aiSummary: string;
  aiKeywords: string[];
  // from disposer side
  source: string;          // context trigger
  who: string;             // whose pain point
  dimensions: {
    cost: 1-5;
    scope: 1-5;
    edge: 1-5;
    urgency: 1-5;
  };
  total: number;
  verdict: "act" | "watch" | "discard";
  verdictLabel: string;
  // system
  timestamp: string;
  status: "new" | "processed" | "archived";
  logGenerated: boolean;
}

**Auto‑generated engineering log draft (when verdict == "act"):**

## Open questions

1. **UI layout:** How to fit both the recording/summary block and the scoring dimensions on one page without clutter? Tentative: top = record + AI result, bottom = scoring + verdict, with a clear visual separator.
    
2. **Unified history:** Currently each tool maintains its own list. Merged version should show a single timeline with full context (raw + AI + verdict). Need a compact card design.
    
3. **Log draft storage:** Should the draft be stored in localStorage alongside the idea, or should I only provide a "copy to clipboard" button? Since Obsidian is external, copy‑to‑clipboard is more practical. But storing a reference (`logGenerated: true`) helps avoid duplicates.
    
4. **Will users find 4‑dimension scoring too heavy?** Should I add a "quick mode" in settings that pre‑fills scores and only asks for Edge and Urgency adjustments?