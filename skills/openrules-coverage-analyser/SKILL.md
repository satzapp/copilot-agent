---
name: openrules-coverage-analyser
description: Build a rule-coverage plan for OpenRules decision tables before writing tests — one or more tests per rule, boundary values at every threshold, single-hit vs multi-hit logic, default rules. Use after the glossary is classified.
---

# Coverage Analyser

For every decision table in Rules.xlsx, output: Table | Hit policy | Rule # | Conditions | Test(s) that hit it.

## Hit policy
- `DecisionTable` / `DecisionSingleHit`: rules top-down, **first hit wins**; the last
  rule is the default — always add a test that falls through to it.
- `Decision` / `DecisionMultiHit`: **all** satisfied rules execute in order; later
  actions may override or accumulate. Add tests for: no extra rule, each extra rule
  alone, and combinations that accumulate (e.g. VacationDays: 22, 22+5, 22+3, 22+2,
  22+5+3 — noting +3 requires the +5 conditions, so trace, don't assume).

## Per rule
- One **positive** test where all its AND conditions are true and (single-hit) all
  earlier rules are false.
- **Boundary tests** at every numeric/date threshold: value-1, value, value+1.
  Intervals `[15..30)`: test 14, 15, 29, 30. Operators from the manual:
  Is, Is Not, >, >=, <=, <, Within, Outside, Is One Of, Contains, Like, Match, Is Empty.
- **Negative tests** are ordinary rows: flip one condition at a time and expect the
  next rule / default result. There is no expected-error construct in OpenRules —
  never invent one.
- String conditions: exact value, different case (comparisons are case-sensitive by
  default unless `model.ignoreCase`), empty, and `null` (non-primitive types only).
- Boolean conditions: TRUE/Yes and FALSE/No variants.
- Collections: empty, one element, several elements, element present/absent for
  Include/Exclude/Intersect/Is One Of.

## Output
A coverage matrix only — no test sheet yet. Flag any rule you cannot hit
(unreachable, contradictory, or missing Glossary data) instead of forcing a test.
Coverage is judged later by the Execution Report (it shows only executed rules);
there is no OpenRules coverage percentage.
