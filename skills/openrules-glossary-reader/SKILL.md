---
name: openrules-glossary-reader
description: Read an OpenRules Glossary.xlsx and classify every decision variable for test generation — inputs, outputs, constants, formula variables, types, business concepts. Use before generating any OpenRules test cases.
---

# Glossary Reader

1. Read the `Glossary` table. Mandatory columns, in order:
   `Decision Variable` | `Business Concept` | `Attribute` | `Type`.
   Optional: `Description`, `Used As`, `Default Value`, `Domain`, `JSON Name`.
2. Output a classification table with one row per decision variable:
   Variable (exact spelling) | Concept | Type | Role | Testable?
3. Role rules:
   - `Used As` contains `in` or `required` → **input** → goes in `ActionDefine`.
   - `Used As` contains `out`, or it is the model goal/sub-goal calculated by a
     decision table → **output** → goes in `ActionExpect`.
   - `Used As` empty → temporary: not an input, include as expected only if a
     decision table calculates it and the user wants it checked.
   - `Used As` contains `const` → constant: do not vary it per test unless asked.
   - Business Concept cell is `:=` → **formula variable: NEVER put it in a test table.**
4. Record for arrays (`String[]`, `int[]`, concept arrays): element type and concept,
   because they need comma/Alt+Enter values or `DecisionData` + `ActionUseObject`.
5. Record `Default Value` and `Domain` when present — use them for boundary cases.
6. Stop and ask if: a variable in Rules.xlsx is missing from the Glossary, a Type is
   unknown, or `Used As` is absent and input/output cannot be told apart from the tables.
Never invent a variable, type, or concept.
