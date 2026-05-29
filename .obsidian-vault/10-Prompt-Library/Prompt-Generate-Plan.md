---
title: Prompt Library — Generate Implementation Plan
author: Solo Developer
created: 2026-05-28
tags: [prompt, ai, plan]
---

# Prompt Library — Generate Implementation Plan

## Prompt: Tạo Implementation Plan

```
Tôi đang implement {FEATURE} theo quy trình Superpowers.

Tài liệu Spec đã được duyệt (Approved Spec):
{PASTE SPEC HERE}

Hãy tạo Implementation Plan (Kế hoạch thực thi) chi tiết dựa trên phần **Spec to Plan Checklist** ở Bước 6 của tài liệu Spec.

Yêu cầu kế hoạch:
- Phân rã mỗi đầu việc thành các subtasks cực nhỏ (2-5 phút thực hiện).
- Mỗi task phải có: file cần tạo/sửa + test case mong muốn (nếu có) + lệnh chạy test.
- Tuân thủ quy trình TDD nghiêm ngặt (viết test RED trước, viết code GREEN sau, sau đó REFACTOR).

---
type: implementation-plan
status: ready
module: {module}
date: {date}
---

# Plan: {Feature}

## Objective
1 câu.

## Tasks

### Task 1.1: Write failing test for [behavior]
- File: `src/{service}/{service}.spec.ts`
- Test:
```typescript
it('should ...', async () => {
  // arrange
  // act
  // assert
});
```
- Command: `npm test -- --testNamePattern="should ..."`
- Expected: FAIL (RED)

### Task 1.2: Implement minimal code
- File: `src/{service}/{service}.ts`
- Logic: [minimal description]
- Command: `npm test -- --testNamePattern="should ..."`
- Expected: PASS (GREEN)

[continue...]
```

## Prompt: Estimate Task Complexity

```
Tôi đang plan sprint (solo developer).

Features cần implement:
{LIST OF FEATURES}

Estimate cho mỗi feature:
- Complexity: S/M/L/XL
- Estimated days (1 person)
- Dependencies
- Risks
```
