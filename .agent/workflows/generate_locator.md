---
description: Generate stable UI locator for automation testing.
skills:
  - smart-locator-agent
---

> **BẮT BUỘC (MANDATORY SKILL):** Bạn PHẢI nạp và đọc kỹ nội dung của skill **`smart-locator-agent`** (tại `.agent/skills/smart-locator-agent/SKILL.md`) trước khi bắt đầu thực hiện tác vụ này.

Generate the most stable locator for the given UI element.

Input:

HTML snippet or DOM element.

Instructions:

1. Analyze element attributes.
2. Identify stable locator candidates.
3. Recommend the best locator.

Locator priority:

1. id
2. data-testid
3. name
4. css selector
5. xpath

Avoid:

- dynamic classes
- absolute xpath