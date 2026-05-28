---
title: Prompt Library — Generate Spec
author: Solo Developer
created: 2026-05-28
tags: [prompt, ai, spec]
---

# Prompt Library — Generate Spec

## Prompt: Tạo SRS Document

```
Tôi cần bạn tạo SRS (Software Requirements Specification) cho module: {MODULE_NAME}

Context:
- Tech stack: {TECH_STACK}
- Business rules: {KEY_BUSINESS_RULES}

Tạo SRS theo cấu trúc sau:

---
title: SRS — {Module Name}
type: spec
module: {module-slug}
status: Draft
created: {date}
---

# SRS — {Module Name}

## Problem
(Vấn đề gì đang giải quyết?)

## Goal
(Mục tiêu cụ thể)

## Scope
**In scope:**
- ...

**Out of scope:**
- ...

## User Flow
(Luồng user step by step)

## Technical Design
(Architecture, data flow)

## API
| Method | Endpoint | Auth | Mô tả |

## Database
(SQL schema)

## Edge Cases
| Case | Handling |

## Acceptance Criteria
- [ ] ...

## Related
- [[...]]
```

## Prompt: Brainstorm Edge Cases

```
Tôi đang implement {FEATURE}.

Current design:
{PASTE SPEC HERE}

Hãy liệt kê các edge cases tôi có thể bỏ sót:
1. Concurrent access issues
2. Race conditions
3. Error states
4. Boundary values
5. Security vulnerabilities
6. Business rule violations
```
