---
name: openrules-coverage-report
description: Generate the OpenRules test coverage sheet in Excel after a passing run — rule, table, variable, scenario and boundary coverage computed from the Execution Report, with unexecuted rules listed. Use as the final orchestration phase.
---

# Coverage Report

Precondition: a fully passing `mvn clean install`. No passing run, no report.

Source of truth: the Execution Report (target/reports) — it lists only
actually executed rules, with Excel cell refs and variable values at
execution time. Cross-check it against the scope defined in
`1_ImpactAnalysis` and the cases in `2_TestPlan`.

Output sheet `3_Coverage`:
- Header: Table | Scope | Plan version | Run date | Build result.
- Metrics (state the definition next to each number — OpenRules publishes
  no coverage %, these are this project's defined metrics):
  - Rule coverage % = executed rules ÷ total rules in scope (headline)
  - Table coverage % = exercised tables ÷ scope tables
  - Variable coverage % = (inputs exercised + outputs asserted) ÷ in-scope variables
  - Scenario coverage % = planned cases passed ÷ planned, split by
    functional / positive / negative / boundary
  - Boundary coverage % = thresholds tested at -1/exact/+1 ÷ total thresholds
- **Gap list (mandatory)**: every unexecuted rule (table + rule # + its
  conditions), every untested threshold, every planned-but-not-generated
  case — each with a suggested additional case for the next plan version.
- Verdict line: “Full coverage” only if rule coverage is 100% AND the gap
  list is empty; otherwise “Partial — see gaps”.
