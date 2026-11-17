---
mode: agent
description: Generate OpenRules Excel unit tests for a decision model — DecisionTest sheet grid only, no code
---

Using the OpenRules Test Agent skills, generate unit tests for the decision model
in this repo:

1. Read `rules/Glossary.xlsx` with the glossary-reader skill and show the
   input/output classification. Stop if any variable is unclassified.
2. Read `rules/Rules.xlsx` and show the coverage-analyser matrix
   (every rule, boundaries, default rules).
3. Write the `rules/Test.xlsx` sheet grid with the decisiontest-writer skill —
   or the decisiondata-builder skill if the model uses objects/arrays.
   Match the keyword form (long `ActionDefine…` or short `Define…`) already
   used in the existing Test.xlsx.
4. Run the test-validator checklist and show the result.
5. End with: “Paste this grid into Test.xlsx, then double-click test.bat
   (or explore.bat → Run Tests → ALL TESTS) and paste the result back here.”

Excel only. No code, no JSON, no scripts.
