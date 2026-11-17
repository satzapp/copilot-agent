---
name: openrules-maven-runner
description: Run and read mvn clean install for an OpenRules project to validate that all Excel test cases execute and pass. Use after test generation and after every troubleshooting fix.
---

# Maven Runner

Running a build is validation, not code development: this skill never
writes or edits Java, pom.xml or any code file.

1. Confirm the working folder is the project root (contains pom.xml,
   project.properties, rules/). If not, ask — do not guess the path.
2. Propose the exact command `mvn clean install` and let the user approve
   running it in the VS Code terminal (Copilot terminal-command approval).
3. Read the full output and extract, in this order:
   - BUILD SUCCESS or BUILD FAILURE
   - OpenRules validation errors (they name the Excel file + place)
   - Test cases executed / passed / failed
   - Each mismatch verbatim: test name, cell ref (e.g. B7:L7),
     `expected:<[X]> but was:<[Y]>`
   - Execution Report location (target/reports)
4. Report those facts only. Hand failures to the troubleshooting skill;
   hand a clean BUILD SUCCESS to the coverage skill.
Note: `test.bat` is the project's own wrapper around the same Maven build;
if the user prefers it, treat its output exactly the same way.
