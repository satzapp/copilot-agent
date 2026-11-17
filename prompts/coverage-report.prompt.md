---
mode: agent
description: Generate the Excel coverage sheet for the last passing OpenRules run
---

Act as the OpenRules Coverage agent. Confirm the last `mvn clean install`
run passed fully; if it did not, stop and say so. Then build the
3_Coverage sheet from the Execution Report in target/reports using the
five defined metrics (rule, table, variable, scenario, boundary) and list
every unexecuted rule and untested threshold as gaps with a suggested
extra case each. State that these are the project's defined metrics —
OpenRules publishes no coverage %.
