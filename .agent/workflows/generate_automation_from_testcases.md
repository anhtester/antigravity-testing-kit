---
description: Convert manual test cases into automation scripts.
skills:
  - qa-automation-engineer
---

> **BẮT BUỘC (MANDATORY SKILL):** Bạn PHẢI nạp và đọc kỹ nội dung của skill **`qa-automation-engineer`** (tại `.agent/skills/qa-automation-engineer/SKILL.md`) trước khi bắt đầu thực hiện tác vụ này.

Convert the provided manual test case into automation tests.

Steps:

1. Analyze the test case steps.
2. Identify UI actions required.
3. Discover UI elements and stable locators.
4. Generate Page Object methods.
5. Generate TestNG test class.

Framework:

Language: Java
UI Automation: Selenium WebDriver
Test Framework: TestNG
Pattern: Page Object Model

Locator priority:

1. id
2. data-testid
3. name
4. css selector
5. xpath

Output:

- Page Object class
- Test class
- Assertions validating expected results