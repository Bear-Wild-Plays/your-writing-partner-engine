# Automated Quality Gate & Dramatic Failure Mode Rubric

## Overview
Every scene generated in **Stage 3** must pass a deterministic **5-Check LLM-as-a-Judge Evaluation** before being committed to the assembled script. This gate converts subjective dramatic principles into auditable pass/fail criteria with bounded self-correction.

---

## Execution & Retry Policy (The 2-Rewrite Cap)

* **Pass Condition:** A scene must score `PASS` on all 5 checks (`5/5`) to advance.
* **Targeted Rewrite Loop:** If a scene scores `FAIL` on one or more checks, the evaluator outputs the failed check IDs, exact line citations, and remediation instructions into `STAGE_3_TARGETED_REWRITE_PROMPT`.
* **Hard Iteration Ceiling (`max_retries = 2`):** To prevent infinite generation loops, token cost spikes, and over-edited "flattened" prose, each scene is capped at **2 rewrite attempts**.
* **Escalation Fallback:** If a scene still fails after 2 rewrites, the engine locks the highest-scoring draft, appends a `[HUMAN_REVIEW_FLAG]` metadata tag with the unresolved failure note, and proceeds to the next beat.

---

## The 5 Dramatic Failure Mode Checks

### Check 1: On-the-Nose Dialogue (`FAIL_ON_THE_NOSE`)
* **Definition:** Characters explicitly verbalize their internal psychological flaw, the story's thematic thesis, or pure exposition without tactical motivation.
* **Pass Criteria:** Dialogue operates as action (persuading, deflecting, concealing, testing) while subtext carries emotional or thematic weight.
* **Exception Rule:** None. Even heightened genres require subtextual tactics.

### Check 2: Static Scene Dynamics (`FAIL_STATIC_SCENE`)
* **Definition:** The power balance, emotional temperature, or informational state between characters at the end of the scene is identical to the beginning.
* **Pass Criteria:** The scene executes the value shift defined in the Stage 2 Beat Outline (`entering_value` $\neq$ `exiting_value`) through observable turning points.
* **Exception Rule:** Dedicated transitional montage beats explicitly flagged in Stage 2 as `transition_only: true`.

### Check 3: Cliché & Stock Idioms (`FAIL_CLICHE_IDIOM`)
* **Definition:** Reliance on worn-out dramatic tropes, stock thriller/romance phrasing, or unmotivated melodramatic outbursts.
* **Pass Criteria:** Imagery, metaphors, and reactions feel specific to the character's world, era, and psychological profile.
* **Context Exceptions:**
  1. **Intentional Melodrama (`allow_melodrama_exception: true`):** When the Stage 1 Story Bible explicitly designates a melodramatic form (e.g., soap opera, Douglas Sirk-style domestic melodrama, heightened farce), heightened emotional expression is permitted provided it remains structurally motivated.
  2. **Character-Mocked Idioms:** Stock clichés are permitted when a character deploys them ironically, sarcastically, or to mock another character's hollow speech.

### Check 4: Voice Homogenization (`FAIL_VOICE_DRIFT`)
* **Definition:** Two or more characters speak with interchangeable sentence length, syntax, rhythm, or vocabulary register, erasing their Stage 1 `voice_signature`.
* **Pass Criteria:** Removing character cues would still allow a reader to distinguish speakers by cadence, diction, and verbal tactics.
* **Exception Rule:** Moments of deliberate mimicry or choral/ritualized speech specified in the beat sheet.

### Check 5: Pacing & Word-Count Drift (`FAIL_PACING_DRIFT`)
* **Definition:** Scene length falls outside the calibrated industry density window of **150–225 words per target page** (e.g., collapsing a 3-page confrontation into a 180-word summary or bloating a 1-page beat to 600 words).
* **Pass Criteria:** Total scene word count falls within `[target_pages * 150, target_pages * 225]` ($\pm 10\%$ tolerance).
* **Exception Rule:** Silent physical sequences or stage-direction-heavy set pieces explicitly tagged `visual_set_piece: true` in Stage 2.

---

## Sample Evaluator Output Payload (JSON)

```json
{
  "scene_number": 4,
  "attempt": 1,
  "overall_status": "FAIL",
  "checks": {
    "FAIL_ON_THE_NOSE": {
      "status": "FAIL",
      "citation": "MARA: 'I push everyone away because I'm terrified of losing control like my father did.'",
      "remediation": "Cut self-diagnosis. Have Mara deflect by changing the locks or auditing David's receipts instead."
    },
    "FAIL_STATIC_SCENE": { "status": "PASS" },
    "FAIL_CLICHE_IDIOM": { "status": "PASS" },
    "FAIL_VOICE_DRIFT": { "status": "PASS" },
    "FAIL_PACING_DRIFT": {
      "status": "PASS",
      "actual_words": 412,
      "target_range": [300, 450]
    }
  }
}
```