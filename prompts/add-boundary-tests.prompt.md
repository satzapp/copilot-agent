---
mode: agent
description: Add boundary and negative OpenRules Excel tests for thresholds already in Rules.xlsx
---

Look at every numeric, date, and interval condition in `rules/Rules.xlsx`.
For each threshold T, add DecisionTest rows for T-1, T, and T+1 (for intervals,
both ends), keeping all other inputs at a known passing baseline.
Negative tests are ordinary rows: flip one condition at a time and expect the
next-rule or default result, hand-traced. Do not invent an expected-error
construct — OpenRules has none. Output only the new rows for the existing
DecisionTest table, in the same keyword form, plus the updated coverage matrix.
