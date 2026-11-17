---
name: openrules-troubleshooting
description: Classify and fix OpenRules test-run failures — validation, mismatch, build-environment, execution — by editing only the Excel test sheet, logging each cycle, and re-running. Use whenever mvn clean install fails.
---

# Troubleshooting

For each failure from the maven-runner skill, classify and act:

| Class | Signal | Fix (Excel only) |
|---|---|---|
| A Validation | “Unknown decision variable …”, wrong type, keyword/structure error, each with an Excel location | Correct the name/keyword/type in Test.xlsx to match Glossary.xlsx exactly |
| B Mismatch | `expected:<[X]> but was:<[Y]>` | Hand-trace the rule. Expectation wrong → fix the Expect cell in Test.xlsx. Trace says X but engine gives Y → **rule bug: STOP**, report table + rule + cell ref, wait for human approval before any Rules.xlsx change |
| C Build/env | Maven/JDK/dependency/pom errors, no OpenRules validation message | Report verbatim summary. No pom or code edits — environment is the human's |
| D Execution | Required input undefined, null on primitive type, conversion error | Fix the test data in Test.xlsx (supply the required input; 0/0.0/false for primitives) |

Loop rules:
- Log every cycle in `4_TroubleshootLog`: Run # | Failure (test + cell) |
  Class | Root cause | Excel fix applied | Re-run result.
- Fix, then re-run `mvn clean install` (maven-runner skill). One class at
  a time, smallest Excel change that addresses the root cause.
- Never weaken an expected value, delete a failing case, or mark a case
  inactive (`ActionActive`) just to get a green build — that is hiding a
  failure, not fixing it. Deactivating a case needs human approval.
- Stop at max loops (default 3): report remaining failures, their class,
  and what you tried. A stopped loop is a valid outcome; a forced pass is not.
