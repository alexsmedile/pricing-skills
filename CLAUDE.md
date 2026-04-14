# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Is

Skill database for a pricing strategy system. Three skills + one orchestrator agent. No code, no build pipeline — pure markdown skill definitions consumed by Claude Code.

## Structure

```
skills/
  at-what-price/SKILL.md       — cost-based price spectrum builder
  price-intelligence/SKILL.md  — lead signal extraction → scenario → full price strategy
  pricing-scenario/SKILL.md    — scenario matcher → single final price
agents/
  pricing-expert.md            — orchestrator: runs all three skills as internal advisors
_archive/                      — deprecated/source material (do not modify)
```

## Skill Format

Every skill lives in its own folder as `SKILL.md` with YAML frontmatter:

```yaml
---
name: skill-name
description: >
  Trigger description used by Claude to decide when to invoke this skill.
  Include explicit trigger phrases here.
---
```

Body = step-by-step instructions Claude follows when skill is active.

## Three-Skill System

| Skill | Role | Trigger |
|-------|------|---------|
| `at-what-price` | Builds cost/market/budget spectrum → 3 price options | User needs a number from scratch |
| `pricing-scenario` | Selects one final price using scenario logic + psychological rounding | User has a range, needs a specific number |
| `price-intelligence` | Full end-to-end: lead notes → signals → scenario → price → proposal | User shares client notes or is pre-proposal |

`price-intelligence` is the superset — it includes all logic from the other two. Use the focused skills when the user has a narrower question.

## Orchestrator Agent (`pricing-expert.md`)

Runs all three skills as internal advisors (Analyst / Strategist / Interpreter), has them critique each other, then synthesizes. Outputs `output/PRICE.md`. Use when user wants a full, collaborative pricing session rather than a single-skill answer.

## Pricing Logic (shared across skills)

- **Profitability floor**: `Min price = Cost ÷ (1 − target margin)`. Default margin: 30–50%. Never output price < cost.
- **Market multipliers**: Basic = 0.8×, Standard = 1.2×, Premium = 1.5–3× cost-based price
- **Psychological rounding**: <2k → nearest 25; 2k–10k → nearest 50; >10k → nearest 250. Prefer left-digit perception (4,950 not 5,000).
- **Scenario rules**: 10 scenarios mapped to explicit pricing formulas (see `pricing-scenario/SKILL.md` Step 2)

## Modifying Skills

- Trigger phrases in `description:` frontmatter control when each skill fires — keep them specific and non-overlapping
- Scenario table in `pricing-scenario` and `price-intelligence` must stay in sync
- `_archive/` contains source material (Italian-language psychology of pricing content) — reference only, do not promote to `skills/`
