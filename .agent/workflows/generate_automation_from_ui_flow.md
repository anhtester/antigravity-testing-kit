---
description: Execute UI steps using browser debugging and generate automation scripts.
skills:
  - ui-debug-agent
---

> **BẮT BUỘC (MANDATORY SKILL):** Bạn PHẢI nạp và đọc kỹ nội dung của skill **`ui-debug-agent`** (tại `.agent/skills/ui-debug-agent/SKILL.md`) trước khi bắt đầu thực hiện tác vụ này.

Execute the provided UI flow and generate Selenium automation code.

Instructions:

1. Open the target URL.
2. Inspect DOM elements using Chrome debug.
3. Identify stable locators.
4. Execute the provided UI steps.
5. Capture element locators.
6. Generate Selenium automation code.

Locator priority:

1. id
2. data-testid
3. name
4. css selector
5. xpath

Framework:

Java  
Selenium WebDriver  
TestNG  
Page Object Model

Output:

- Page Object class
- Test class