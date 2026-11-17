---
mode: agent
description: Full orchestration for one OpenRules decision table — impact analysis, test plan, approval gate, Excel test generation, mvn clean install, troubleshooting, coverage
---

Act as the OpenRules Orchestrator and run ORCHESTRATION.md end to end.

Input:
- Table: ${input:table:Exact decision table name from Rules.xlsx, e.g. CalculateVacationDays}
- Scope: ${input:scope:table + dependents (default) or table only}
- Test types: functional positive, functional negative, boundary (all, unless I say otherwise)
- Run command: mvn clean install
- Max troubleshooting loops: 3

Phases: 1 Glossary read → 2 Impact analysis (1_ImpactAnalysis sheet) →
3 Test plan (2_TestPlan sheet: functional / positive / negative / boundary) →
4 STOP for my approval — apply my add/modify input, version the plan, and
re-ask until I approve → 5 Generate DecisionTest/DecisionData in Test.xlsx →
6 Validate → 7 Run mvn clean install → 8 Troubleshoot failures by fixing
only Test.xlsx (stop and report if a rule looks wrong) and re-run →
9 Coverage (3_Coverage sheet) after a fully passing run.

Everything in Excel sheets. No Java or any other code, no pom edits.
