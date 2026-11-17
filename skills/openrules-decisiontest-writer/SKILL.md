---
name: openrules-decisiontest-writer
description: Write OpenRules DecisionTest tables for Test.xlsx — exact signature, ActionDefine/ActionExpect columns, plain literal values per Glossary type. Use for simple/flat decision models.
---

# DecisionTest Writer

Write the sheet grid exactly like this (VacationDays-style, flat inputs):

```
Row 1:  DecisionTest testCases                                  (merged, unique name, no spaces)
Row 2:  #        | ActionDefine | ActionDefine | ActionExpect
Row 3:  Test ID  | Age in Years | Years of Service | Vacation Days
Row 4:  Test 1   | 17           | 5              | 27
...
```

Rules:
- Keywords are case-sensitive. The April-2026 manual uses the long forms
  `ActionDefine / ActionExpect / ActionUseObject / ActionActive`; the Testing
  Facilities page and samples use short forms `Define / Expect / UseObject / Active`.
  **Match the form already used in this project's existing Test.xlsx. Never mix
  the two forms in one table.** Default to the long forms for a brand-new file.
- Blue = inputs (`ActionDefine`), pink = expected outputs (`ActionExpect`).
- Derive each expected value by hand-tracing Rules.xlsx top-down before writing it.
- Types: Date as text `yyyy-MM-dd`; Boolean `TRUE/Yes`, `FALSE/No`; numbers plain;
  leading `=`/`==` prefixed with an apostrophe; no Excel formulas, no expressions,
  no operators in value cells.
- Arrays (e.g. `Patient Allergies`, String[]): values in one cell separated by
  Alt+Enter line breaks, or comma-separated in one cell.
- `null` only for String/Date/Integer/Double/Boolean/custom types — never for
  primitive `int/double/boolean` (use 0 / 0.0 / false).
- Use the `ActionActive` (short: `Active`) column with `X` to run a subset;
  if all its cells are empty, all tests run. Never delete tests to skip them.
- Multiple DecisionTest tables are allowed — names must be unique.
- Do NOT use `UseJson` columns or JSON test files: this project is Excel-only,
  even though OpenRules supports JSON tests as an alternative.

Worked reference (PatientTherapy, from the Testing Facilities page — 3 tests):
inputs Encounter Diagnosis / Patient Age / Patient Weight / Patient Allergies /
Patient Active Medication / Patient Creatinine Level → expected Recommended
Medication / Recommended Dose / Drug Interaction Warning / Patient Creatinine
Clearance. Test 1: Acute Sinusitis, 58, 78, Penicillin+Streptomycin (Alt+Enter),
Coumadin, 2.00 → Levofloxacin, “500mg every 24 hours for 14 days”, Coumadin warning,
44.42. Test 2 → Amoxicillin, 48.03. Test 3 (Diabetes) → no medication expected.
Use it as a shape example only — never copy its values into another model.
