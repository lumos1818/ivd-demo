# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository purpose

Concept demonstration for a research paper by Lusi Chen (2026) on "Internal Value Development" (IVD). Each script issues two API calls with the same withdrawal-request case but different system prompts (standard rule-follower vs. IVD-trained) and prints a side-by-side comparison. This is a demo, not a library — there are no modules, tests, package config, or CI.

## Commands

Anthropic version (`ivd_demo.py`):
```bash
pip install anthropic
export ANTHROPIC_API_KEY=...
python ivd_demo.py
```

Gemini version (`ivd_demo_gemini.py`):
```bash
pip install google-genai
export GEMINI_API_KEY=...
python ivd_demo_gemini.py
```

Each run writes `ivd_demo_results_YYYYMMDD_HHMMSS.json` to the working directory. There is no lint, type-check, or test command in this repo.

## Architecture

Two **parallel implementations** of the same demo, one per provider. They share identical structure and three critical constants that are **duplicated verbatim** between the files:

- `CASE` — the user message describing Ms. Jiang's withdrawal request
- `SYSTEM_WITHOUT_IVD` — baseline rule-follower system prompt, returns a smaller JSON schema (`steps`, `decision` ∈ {APPROVE, DENY}, `reason`, `flagged_concerns`)
- `SYSTEM_WITH_IVD` — IVD-trained system prompt, returns an expanded schema adding `contextual_assessment`, `recommended_action`, `safeguards`, and widens `decision` to {APPROVE, DENY, CONDITIONAL_APPROVE, ESCALATE_WITH_RECOMMENDATION}

**When editing CASE or either system prompt, update both files** or the two scripts will diverge. The comparison only makes sense when they run the same case and prompts.

Per-script flow is: `call_*()` → `print_result()` → `main()` → save JSON. Response parsing relies on `re.search(r'\{[\s\S]*\}', text)` — a greedy match from the first `{` to the last `}`. If the model emits prose around the JSON, the regex still extracts it; if it emits multiple JSON objects, only the outermost span is captured.

Model/config differences between the two scripts:
- Anthropic: `claude-sonnet-4-20250514`, `max_tokens=2000`, no temperature set
- Gemini: `gemini-2.5-flash`, `temperature=0.2`, no max-tokens cap

The rhetorical payoff of the demo depends on the two systems actually producing different decisions. If you change models, prompts, or temperature, re-run end-to-end and confirm the `COMPARISON` block still shows `DENY` vs. a more nuanced decision — otherwise the README's "Patient dies / Patient treated" framing no longer holds.

## Conventions

- Keep the Anthropic and Gemini scripts behaviorally equivalent; any case/prompt change goes in both.
- Preserve the exact JSON schemas in the system prompts — `print_result` reads specific keys (`steps`, `contextual_assessment`, `recommended_action`, `safeguards`, `flagged_concerns`) and missing keys are silently skipped.
- Box-drawing characters and emoji marks (`✓ ✗ ⚠ ═ ║ ╔ ╚`) in console output are intentional; the demo is meant to be visually read in a terminal.
