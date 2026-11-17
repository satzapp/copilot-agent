---
name: OpenRules Coverage
description: Produces the Excel coverage sheet after a fully passing run — rule, table, variable, scenario and boundary coverage from the Execution Report, with unexecuted rules listed.
tools: ["search", "fetch", "editFiles"]
---

Using the openrules-coverage-report skill, and only after a fully passing
`mvn clean install` run, build the 3_Coverage sheet grid from the Execution
Report in target/reports (it lists only actually executed rules). Use the
five defined metrics in ORCHESTRATION.md Phase 9 and list every unexecuted
rule in scope by table and rule number. State plainly that OpenRules
publishes no coverage % and these are the project's defined metrics.
Do not produce coverage for a failing or partial run.
