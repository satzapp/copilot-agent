---
name: openrules-impact-analysis
description: Impact analysis for a named OpenRules decision table — direct variables, upstream and downstream tables, Execution Path, shared variables, affected existing tests and risk flags, as an Excel sheet. Use when the user mentions a table to analyse or test.
---

# Impact Analysis

Input: one exact table name. Read Glossary.xlsx, Rules.xlsx,
DecisionModel.xlsx (Environment), Test.xlsx (existing tests).

Output sheet `1_ImpactAnalysis` with these sections, one row per item:
1. **Target table** — name, type (Decision multi-hit / DecisionTable
   single-hit / Big…), hit policy, rule count, goal variable it serves.
2. **Rules inventory** — Rule # | Conditions (variables + operators +
   thresholds) | Conclusions (variable + operator + value).
3. **Direct variables** — Variable (exact Glossary spelling) | Concept |
   Type | Used As | Direction (reads / writes).
4. **Upstream** — tables that produce its inputs, in Execution Path order
   (the order OpenRules auto-computes: sub-goals before goals).
5. **Downstream** — tables/goals that consume its outputs, and the business
   effect if this table's result changes.
6. **Shared variables** — other tables reading/writing the same variables
   (change here can silently change behaviour there — flag each).
7. **Existing tests affected** — Test.xlsx rows exercising this table or
   its dependents: keep / update / superseded, with reason.
8. **Risks** — Glossary formula variables (:=, never test), required
   inputs, arrays/collections, CSV-backed Big tables, default-rule
   behaviour, variables with the same name in different concepts.

Rules: point to the sheet/cell source for every claim; if a dependency
cannot be confirmed from the sheets, mark it “unverified” — never guess.
This sheet feeds the test plan; it contains no test cases itself.
