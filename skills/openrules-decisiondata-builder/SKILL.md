---
name: openrules-decisiondata-builder
description: Build OpenRules DecisionData tables and ActionUseObject DecisionTest tables for complex models with business objects, nested concepts, arrays and cross-references. Use when a flat DecisionTest would be too wide.
---

# DecisionData Builder

Use when inputs are whole objects, nested concepts, or collections
(AnalyzeEmployees: companies → employees → locations; OrderPromotion: orders → orderItems).

## DecisionData table
```
Row 1:  DecisionData Patient patients        (Concept exactly as in Glossary + array name)
Row 2:  Patient Name | Patient Age | Patient Allergies | ...
Row 3+: John Smith   | 58          | Penicillin␤Streptomycin (Alt+Enter) | ...
```
- One table per business concept; array name is free but referenced exactly.
- Cross-reference: put `>otherArray` in row 2 of the referencing column
  (e.g. orders column `Order Items` has `>orderItems` in row 2). Values below are
  the **first-column keys (String)** of the target table; multiple keys in one cell
  separated by Alt+Enter, or use sub-rows with the parent cell merged.

## DecisionTest with objects
```
Row 1:  DecisionTest testCases
Row 2:  #       | ActionUseObject | ActionUseObject | ActionExpect
Row 3:  Test ID | Patient         | DoctorVisit     | Recommended Medication
Row 4:  Test 1  | patients[0]     | visits[0]       | Levofloxacin
```
- Indexes are 0-based: `patients[0]` = first row, `visits[2]` = third row.
- Row 3 under `ActionUseObject` is the **Business Concept** name, not a variable.
- Keep expected outputs as plain literals, traced from the rules as usual.
- Match long (`ActionUseObject`) vs short (`UseObject`) keyword form to the
  project's existing Test.xlsx; never mix forms in one table.
