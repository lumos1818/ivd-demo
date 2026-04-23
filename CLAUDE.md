# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Purpose

Concept demonstration for the author's doctoral research on **Internal Value Development (IVD)**. Each script calls an LLM twice on the same real-world case (Shanghai, Oct 2025: a comatose patient with ¥2M in savings whose distant relative is denied access for ICU costs), using two different system prompts — one "standard rule-following" AI and one "IVD-trained" AI — then prints and persists a side-by-side comparison. The IVD system is simulated via prompt engineering only; no actual training is involved.

## Running

```bash
# Anthropic version
pip install anthropic
export ANTHROPIC_API_KEY=...
python ivd_demo.py

# Gemini version
pip install google-genai
export GEMINI_API_KEY=...
python ivd_demo_gemini.py
```

Each run writes a timestamped `ivd_demo_results_YYYYMMDD_HHMMSS.json`. There is no build system, test suite, or linter configured.

## Architecture

`ivd_demo.py` (Anthropic, `claude-sonnet-4-20250514`) and `ivd_demo_gemini.py` (Google, `gemini-2.5-flash`) are **intentional parallel implementations** of the same demo. They share four blocks verbatim or near-verbatim:

1. `CASE` — the user-message case description.
2. `SYSTEM_WITHOUT_IVD` — the baseline system prompt; constrains the model to PASS/FAIL steps and APPROVE/DENY.
3. `SYSTEM_WITH_IVD` — the IVD system prompt; adds ESCALATE step results, a `contextual_assessment` object, and extra decision values (`CONDITIONAL_APPROVE`, `ESCALATE_WITH_RECOMMENDATION`), plus `recommended_action` and `safeguards`.
4. `print_result` / `main` — the comparison output and JSON-persistence logic.

**When editing the case, either system prompt, the output JSON schema, or the printed comparison, apply the change to both files** unless the change is provider-specific (API client setup, model name, SDK call shape).

The two prompts produce **different JSON schemas**. `print_result` handles both in one function by treating `contextual_assessment`, `recommended_action`, `safeguards`, and `flagged_concerns` as optional — keep that optionality if you add new fields on either side. Response text is parsed by extracting the first `{...}` substring with a greedy regex (`\{[\s\S]*\}`); if you change the schema, make sure the model still returns a single top-level JSON object with no surrounding prose.

## Content Sensitivity

The case description and framing in the README reference a real, recent event and a named research program. Edits to the case text, system prompts, or the closing "The difference is not more rules..." message are substantive to the author's argument — treat them as content changes, not cosmetic ones, and confirm with the user before rewriting.
