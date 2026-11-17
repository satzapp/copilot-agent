---
mode: agent
description: Review existing OpenRules Test.xlsx against Glossary and Rules — Excel fixes only
---

Review the existing `rules/Test.xlsx` against `rules/Glossary.xlsx` and
`rules/Rules.xlsx` using the test-validator skill:
- names that don't match the Glossary exactly, wrong keyword case/form,
  formula variables used in tests, wrong-typed values, Excel-formatted dates,
  `null` on primitive types, reversed intervals, broken `array[index]` or
  `>array` references;
- rules with no test hitting them (use the Execution Report if provided);
- expected values that don't match a hand-trace of the rules.
Report each finding as: sheet cell → problem → exact Excel fix. Change nothing
outside the test sheet, and write no code.
