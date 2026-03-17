---
description: Analyze automation tests and detect flaky test causes.
skills:
  - flaky_test_analyzer
---

> **BẮT BUỘC (MANDATORY SKILL):** Bạn PHẢI nạp và đọc kỹ nội dung của skill **`flaky_test_analyzer`** (tại `.agent/skills/flaky_test_analyzer/SKILL.md`) trước khi bắt đầu thực hiện tác vụ này.

Analyze automation tests to identify flaky test causes and suggest improvements.

Steps:

1. Review the provided automation code.
2. Identify unstable locators.
3. Detect synchronization issues.
4. Identify timing problems.

Common flaky causes:

- dynamic DOM elements
- missing waits
- unstable selectors
- race conditions
- environment dependencies

Provide recommendations to stabilize tests.

Output:

1. Flaky test root causes
2. Suggested fixes
3. Improved locator strategies
4. Improved wait strategies