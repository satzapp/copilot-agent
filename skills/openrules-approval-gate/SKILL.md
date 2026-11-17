---
name: openrules-approval-gate
description: The human approval gate between test planning and test generation for OpenRules — present impact and plan, stop, collect approve/modify/add input, version the plan, and only then unlock generation. Use at every approval point.
---

# Approval Gate

Protocol (the orchestrator must follow it literally):
1. Present: `1_ImpactAnalysis` summary, `2_TestPlan` (full sheet), plan
   version, counts by type (functional / positive / negative / boundary),
   and every case marked “TO CONFIRM”.
2. End with exactly one question: “Approve this plan (vN), or tell me what
   to add / modify / remove?” Then **STOP**. No generation, no Maven run,
   no “preparing in the background”.
3. Interpret the reply:
   - “Approve” / “Approved” / “Go ahead” for vN → record Approved by + date
     in the sheet header, unlock generation for vN only.
   - Add / modify / remove instructions → send them verbatim to the Test
     Planner, get plan vN+1 with a “Changes requested” line summarising
     them, re-present, go to step 2.
   - A question about the plan → answer it, re-ask, stay stopped.
   - Silence / unrelated message → do not treat as approval.
4. A second, separate gate exists in troubleshooting: if a failure points
   to a wrong **rule** (not a wrong test), stop and get explicit approval
   before anyone edits Rules.xlsx. Test-sheet fixes inside an approved
   plan do not need a new gate.
Approval is per plan version: approving v1 never approves v2.
