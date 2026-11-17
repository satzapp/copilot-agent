---
name: openrules-test-validator
description: Validate generated OpenRules Excel test tables against OpenRules validation errors, then guide the human run with test.bat or Explorer and triage expected-vs-actual mismatches. Use after writing any test sheet.
---

# Test Validator

## Pre-check (all must pass before handing over the sheet)
- [ ] Signature is `DecisionTest <uniqueNoSpaceName>`; keywords case-sensitive;
      one keyword form (long `Action…` or short) used consistently per project.
- [ ] Row 3 names match Glossary column 1 character-for-character
      (prevents “Unknown decision variable ‘Years of Servise’”).
- [ ] No Glossary formula variable (`:=` in Business Concept) appears in the table.
- [ ] Every value matches its Glossary Type; dates are text `yyyy-MM-dd`;
      booleans are TRUE/Yes/FALSE/No; no Excel formulas anywhere.
- [ ] `null` used only for non-primitive types; intervals never reversed (lower < upper).
- [ ] Array values use Alt+Enter or commas; `array[index]` references exist and are 0-based;
      every `>array` reference points at a real DecisionData table whose first column is String.
- [ ] Each expected value was hand-traced; coverage matrix shows every rule hit,
      including each single-hit table's last (default) rule.
- [ ] No code, no `Code`/`Method` tables, no `:=`, no JSON files were produced.

## Human run (the agent instructs, the human clicks — no code is written)
1. Confirm `project.properties` has `model.file="rules/DecisionModel.xlsx"` and
   `test.file="rules/Test.xlsx"` (do not edit Environment to add Test.xlsx).
2. Double-click `test.bat` — or `explore.bat` → Run Tests → ALL TESTS / selected.
3. Read the Execution Console / Execution Report (HTML reports in `target/reports`):
   it lists only actually executed rules, with Excel cell refs and variable values.

## Mismatch triage — format: `expected:<[X]> but was:<[Y]>`
Classify, then fix **only the Excel sheet** (or report the rule bug — never “fix” it in code):
1. **Wrong expectation** — re-trace the rules; if the trace gives Y, correct the Expect cell.
2. **Wrong rule** — trace gives X but engine gives Y: report table + rule # + cell ref
   from the Execution Report to the user as a likely Rules.xlsx issue; do not change rules unless asked.
3. **Typo/structure** — unknown variable, wrong keyword case, date reformatted by Excel:
   fix the test sheet and re-run.
