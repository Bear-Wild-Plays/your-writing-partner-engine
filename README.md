# Your Writing Partner Engine

**A multi-stage Content Engineering architecture and LLM evaluation framework for either short-form or long-form screenplay generation.**

## Overview

Being a playwright and screenwriter for many years I find composing dramatic narrative fun. I am willing to put in the hours and weeks re-writing my ideas, or comissioned projects. Doing this, I learned all the "rules" an author needs if they want their script to be good, or hopefuly great! Implementing these rules isn't the fun part. But, I created a content engine that puts the "fun" back in screenwriting. Large Language Models routinely fail at long-form dramatic writing (screenplays and stageplays) due to context drift, structural pacing collapse, and generic theatrical clichés. **Your Writing Partner** solves this by replacing single-shot prompting with a deterministic **3-Stage Chunking Pipeline**, a hierarchical **Story Bible JSON Schema**, and an automated **5-Check Quality Gate** grounded in dramatic writing theory. Have you ever wanted to see a film noir monster movie with a sexy lesbian witch in a magical realm? Now you can. Or, perhaps you want put together your childhood story into a screenplay. Here is your chance. If you are an amateur who wants to try it out, or a seasoned professional that wants a rough draft within minutes Your Writing Partner is here for you! 

---

## System Architecture

```text
[User Intake Prompts (Tiers 1–4)]
               │
               ▼
┌──────────────────────────────┐
│  STAGE 1: Story Bible JSON   │  ◄── Enforces character arcs, flaws, stakes & A/B/C ratios
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  STAGE 2: Beat Sheet Outline │  ◄── Maps Act I / II / III page percentages & turning points
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  STAGE 3: Scene Generation   │  ◄── Drafts individual scenes calibrated to 150–225 words/page
└──────────────┬───────────────┘
               │
               ▼
┌──────────────────────────────┐
│  QUALITY GATE: 5-Check Rubric│  ◄── Evaluates dramatic failure modes (Max 2 rewrites/scene)
└──────────────────────────────┘
```

### 1. Tiered Scope & Structural Calibration
The engine scales generation constraints across four complexity tiers—from short scenes (Tier 1: 1–10 pages) to full-length features and plays (Tier 4: 90–120 pages):
* **Pacing Calibration:** Enforces industry-standard script density (**150–225 words per page**) to prevent under-written summaries or bloated prose.
* **Act Proportions:** Locks structural milestones across **Act I (~25%)**, **Act II (~50%)**, and **Act III (~25%)**.
* **Plotline Ratios:** Dynamically balances **A-Story (main plot)**, **B-Story (relationship/theme)**, and **C-Story (subplot/runner)** beat distributions based on script length.

### 2. Three-Stage Chunking Pipeline
* **Stage 1 — Story Bible Generation:** Compiles user intake parameters into a single source-of-truth JSON object defining protagonist/antagonist objectives, psychological flaws, home life, and dramatic stakes.
* **Stage 2 — Structural Beat Outline:** Generates a scene-by-scene beat sheet anchored to the Story Bible, verifying structural turning points before dialogue is written.
* **Stage 3 — Constrained Scene Drafting:** Generates one scene at a time using rolling context windows + the immutable Story Bible, eliminating mid-script hallucination and character drift.

### 3. Automated Quality Gate (5 Dramatic Failure Modes)
Before any scene is accepted into the final script, an automated LLM-as-a-Judge rubric evaluates the draft against five domain-specific failure modes (with a **hard cap of 2 targeted rewrite loops** per scene):
1. **On-the-Nose Dialogue:** Flags characters explicitly stating subtext or internal thesis statements.
2. **Static Scene Dynamics:** Flags scenes lacking a clear power shift, tactical escalation, or value change.
3. **Cliché & Stock Idioms:** Flags overused theatrical tropes (with explicit context exceptions for intentional *melodrama* or *character-mocked idioms*).
4. **Voice Homogenization:** Flags distinct characters speaking with identical cadence, syntax, or vocabulary.
5. **Pacing & Word-Count Drift:** Flags scenes falling outside the 150–225 words-per-page target window.

---

## Repository Structure

```text
your-writing-partner-engine/
├── README.md                        # System architecture & Content Engineering overview
├── schemas/
│   └── story_bible_schema.json      # Tier 1–4 character, world & structural JSON schema
├── prompts/
│   └── stage_prompts.py             # Modular Stage 1, Stage 2 & Stage 3 system prompts
└── evals/
    └── quality_gate_rubric.md       # 5-check dramatic failure mode rubric & rewrite rules
```

---

## Recommended GitHub Topics
`content-engineering` · `prompt-engineering` · `llm-evaluation` · `structured-outputs` · `dramatic-writing` · `ai-architecture`
