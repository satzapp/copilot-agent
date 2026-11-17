---
mode: agent
description: Troubleshoot a failed mvn clean install run for OpenRules tests — classify, fix Excel only, re-run
---

Here is the output of `mvn clean install` (pasted below my message, or read
it from the terminal). Act as the Runner-Troubleshooter: extract build
result, tests executed/passed/failed and every failure with its Excel cell
reference; classify each as A validation / B mismatch / C build-env /
D execution; fix A, D and wrong-expectation B in Test.xlsx only; log each
cycle in 4_TroubleshootLog; re-run and repeat up to 3 loops. If a trace
shows the rule (not the test) is wrong, stop and report it — do not edit
Rules.xlsx or any code without my separate approval.
