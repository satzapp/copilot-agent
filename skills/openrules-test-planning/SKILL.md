---
name: openrules-test-planning
description: Create the human-approvable test plan for an OpenRules table — functional, positive, negative and boundary cases with inputs, expected results and rules hit, as an Excel sheet. Use after impact analysis, before any test generation.
---

# Test Planning

Input: `1_ImpactAnalysis` + Glossary classification.
Output sheet `2_TestPlan`:
- Header block: Table | Scope | Plan version | Status (Draft / In review /
  Approved) | Approved by | Date | Changes requested (one line per round).
- One row per planned case:
  `Plan ID | Type | Rule(s) hit (Table #Rule) | Inputs (Glossary name = value) |
  Expected result (variable = value) | Rationale`

Case types — plan all four unless the input contract excludes one:
- **Functional** — realistic end-to-end cases for the goal variable,
  combining sub-goals as a real business case would.
- **Positive** — every rule in scope hit at least once, all AND conditions
  true, earlier single-hit rules false.
- **Negative** — one condition flipped at a time; expected result is the
  next rule / default output. Also: empty input, null (non-primitive types
  only), value outside every interval, case-different string. Negative
  means “the rule must NOT fire / the default must apply” — OpenRules has
  no expected-error construct; never plan an `ActionExpectError`.
- **Boundary** — every numeric/date threshold at -1 / exact / +1, both
  ends of every interval (e.g. [15..30) → 14, 15, 29, 30).

Derive every expected result by hand-tracing Rules.xlsx top-down
(single-hit: first hit wins; multi-hit: all hits accumulate/override).
If a trace is ambiguous, keep the case but mark Expected = “TO CONFIRM”
and list it for the human at the approval gate — do not guess.
Stop at the plan. Generation happens only after approval.
