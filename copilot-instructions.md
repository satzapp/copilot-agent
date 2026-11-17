# OpenRules Excel-Only Test Agent — Repository Instructions

This repository contains OpenRules Decision Manager decision models.
Business logic and tests live **only in Excel**. Follow these rules in every response.

## The one rule
- Generate unit test cases **only** as OpenRules Excel tables for `rules/Test.xlsx`:
  tables of type `DecisionTest` and, when needed, `DecisionData`.
- **Never generate code of any kind:** no Java, JUnit, Python, JavaScript, shell scripts,
  macros/VBA, openpyxl/pandas scripts, JSON test files (OpenRules auto-generates those
  from Excel when `test.bat` runs — do not hand-write them), and no pom/build changes.

## Never generate these OpenRules constructs
- Tables of type `Code`, `Method`, `TemplateCondition`, `TemplateAction`
- Java snippets starting with `:=` or containing `${...}`
- `ActionAny` used to hold code
- Legacy 6.x constructs unless the repo is demonstrably a Classic/6.x repo:
  `Rules …(params)`, `C1`/`A1` columns, `DecisionTable1`, `DecisionTable2`,
  `Datatype`, `Data`, `Variable`, `DecisionTableTest`
- Invented keywords — especially `ActionExpectError` (it does not exist in OpenRules)

## Exact DecisionTest format (keywords are case-sensitive)
- Row 1 (signature, merged): `DecisionTest <UniqueNameNoSpaces>`
- Row 2 (column types): first column `#`, then only:
  `ActionDefine` = input (blue), `ActionExpect` = expected output (pink),
  `ActionUseObject` = whole object from a DecisionData array, `ActionActive` = run selector
- Keyword forms: the April-2026 manual uses the long `Action…` forms above; the
  Testing Facilities page and samples use short forms `Define / Expect / UseObject /
  Active`. Match the form already used in this project's existing Test.xlsx —
  never mix the two forms in one table. Default to the long forms for a new file.
- Row 3: decision variable names **exactly** as in Glossary column 1
  (for `ActionUseObject`, row 3 is the Business Concept name)
- Row 4+: one test case per row; column `#` = test name/number
- Values are plain literals matching the Glossary Type:
  Integer/Double = plain numbers, String = plain text, Date = text `yyyy-MM-dd`
  (never Excel date format), Boolean = `TRUE/Yes` or `FALSE/No`,
  arrays = comma-separated in one cell OR sub-rows with other columns merged,
  undefined = `null` — only for String/Date/Integer/Double/Boolean/custom types,
  never for primitive `int/double/boolean` (use 0 / 0.0 / false there)
- Prefix a leading `=` or `==` value with an apostrophe so Excel does not treat it as a formula.
  Never put an Excel formula in a test cell. Never put operators/expressions in
  `ActionDefine`/`ActionExpect` cells — plain values only.

## Exact DecisionData format
- Row 1: `DecisionData <BusinessConcept> <arrayName>` (concept exactly as in Glossary)
- Row 2: decision variable names of that concept; a cell `>otherArray` means the values
  below are keys (first column, type String) of that other DecisionData table;
  multiple keys in one cell are separated by a line break (Alt+Enter) or sub-rows
- Row 3+: one object per row
- Reference objects in DecisionTest as `arrayName[index]`, 0-based: `patients[0]`, `companies[1]`

## Glossary discipline
- Every name used in a test table must match Glossary column 1 character-for-character
  (OpenRules error: “Unknown decision variable ‘Years of Servise’”).
- Decision-variable names: letters/digits/spaces/underscores/hyphens/apostrophes only.
- A variable whose Glossary Business Concept cell is `:=` is a formula variable:
  **never** use it in a DecisionTest table.
- Respect `Used As`: test inputs are `in`/`required` variables; expected outputs are
  variables the model calculates (`out` / goal and sub-goals). Do not “expect” an input.

## Expected values
- Derive every expected value by hand-tracing the decision tables in `Rules.xlsx`,
  in top-down rule order. Single-hit (`DecisionTable`): first satisfied rule wins,
  last rule is the default. Multi-hit (`Decision`): all satisfied rules execute,
  later rules may override/accumulate (`+`, `-`, `*`, `/` conclusions).
- Never invent an expected value, never copy a value you have not traced, and if a
  rule combination is ambiguous, stop and list the ambiguity instead of guessing.

## Output format for every task
1. Coverage plan (table → rule → test name)  2. The sheet grid for Test.xlsx, row by row
3. Validator checklist result  4. “Run it: double-click test.bat, or explore.bat → Run Tests → ALL TESTS”
Nothing else. No code blocks in any programming language.

## Orchestrated workflow (v2 — mandatory when a table is named for testing)
Follow `ORCHESTRATION.md`. In short, when the user names a decision table:
1. **Impact analysis first** — variables, upstream/downstream tables,
   Execution Path, shared variables, affected existing tests, risks —
   as the Excel sheet `1_ImpactAnalysis` in `AgentOutput.xlsx`.
2. **Test plan second** — functional, positive, negative and boundary cases
   as the Excel sheet `2_TestPlan`. No test tables are generated yet.
3. **HUMAN APPROVAL GATE — hard stop.** Present impact + plan, ask for
   approval, and wait. User additions/modifications go back to planning,
   the plan version increments, and you re-ask. Only an explicit approval
   of the current version unlocks generation.
4. **Generate** the approved cases as DecisionTest/DecisionData in
   `rules/Test.xlsx`, then validate (validator skill).
5. **Run** `mvn clean install` (propose the command; the user approves
   running it). Running a build is validation, not code development.
6. **Troubleshoot failures** by fixing only Test.xlsx (classes A/B/D in
   ORCHESTRATION.md), logging each cycle in `4_TroubleshootLog`, and
   re-running — max 3 loops. A suspected rule bug is a hard stop and
   report; never edit Rules.xlsx, pom.xml or any code on your own.
7. **Coverage last, only after a fully passing run** — the Excel sheet
   `3_Coverage`, using the defined metrics in ORCHESTRATION.md (OpenRules
   publishes no coverage %; never present an undefined %).
All analysis, plan, log and coverage artifacts are Excel sheets
(`AgentOutput.xlsx`); all executable tests are Excel (`rules/Test.xlsx`).
