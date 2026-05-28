---
title: Prompt Library — Code Review
author: Solo Developer
created: 2026-05-28
tags: [prompt, ai, code-review]
---

# Prompt Library — Code Review

## Prompt: Review Service Code

```
Review code này:

{PASTE CODE HERE}

Check for:
1. Iron Laws compliance:
   - Có spec trước code không? (Spec lưu tại .obsidian-vault/02-Specs/ hoặc .obsidian-vault/07-Superpowers/Specs/)
   - Có failing tests không?
   - Có báo cáo với evidence không?

2. Code Quality:
   - Có any type không?
   - Error handling đúng exceptions?
   - Single responsibility?
   - No business logic in controller?

3. Security Issues:
   - Route có guard không?
   - Ownership verified trước mutation?
   - Input validated với DTO?

4. Missing Tests:
   - Các case nào chưa được test?

Đưa ra: APPROVE / REQUEST_CHANGES với feedback cụ thể.
```

## Prompt: Review Database Migration

```
Review migration này:

{PASTE MIGRATION HERE}

Check for:
1. Schema đúng với spec không?
2. Indexes đầy đủ (FK, query fields)?
3. Money fields là INTEGER không?
4. Timestamps đúng type?
5. Constraints đúng (NOT NULL, UNIQUE, CHECK)?
```

## Prompt: Review API Endpoint

```
Review API endpoint này:

{PASTE CONTROLLER CODE HERE}

Check for:
1. Response format khớp với spec?
2. HTTP status codes đúng?
3. Auth guard applied?
4. Rate limiting?
5. Validation với DTO?
6. Ownership check trước mutation?
7. Pagination cho list endpoints?
```
