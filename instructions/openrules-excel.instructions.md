---
applyTo: "**/rules/**"
description: "OpenRules Excel rules repository — test tables only, no code"
---

# Rules Repository Instructions

- Files here are OpenRules Excel workbooks: `DecisionModel.xlsx` (Environment),
  `Glossary.xlsx`, `Rules.xlsx`, `Test.xlsx`. Do not propose renaming, converting,
  or replacing them with code, CSV rule engines, or JSON.
- `Test.xlsx` is **not** listed in the Environment table; it is selected by
  `test.file` in `project.properties`. Do not add it to Environment.
- When editing a test sheet, preserve: signature row, row-2 keywords, row-3 variable
  names, blue `ActionDefine` / pink `ActionExpect` column meaning, and one test per row.
- Multiple `DecisionTest` tables are allowed if names are unique. Use `ActionActive`
  to run a subset (if every ActionActive cell is empty, all tests run) — never delete
  a test case just to skip it.
- After any change, remind the user to re-run `test.bat` (OpenRules advice: re-run
  after every small change).
