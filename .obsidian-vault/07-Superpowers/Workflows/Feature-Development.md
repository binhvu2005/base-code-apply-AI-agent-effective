---
title: Workflow — Feature Development
version: 1.0.0
created: 2026-05-28
---

# Feature Development Workflow

> Quy trình phát triển feature từ ý tưởng đến production.

## Prerequisites
- [ ] Có Spec cho feature này trong `.obsidian-vault/07-Superpowers/Specs/` hoặc `.obsidian-vault/02-Specs/`
- [ ] Có ADR nếu cần quyết định kỹ thuật tại `.obsidian-vault/04-ADR/`

## Steps

### 1. 📋 Brainstorming
```
Lưu tại: .obsidian-vault/07-Superpowers/Brainstorming/
```
- Xác định vấn đề cần giải quyết.
- Liệt kê ý tưởng giải pháp.
- Chọn approach tốt nhất.

### 2. 📐 Spec
```
Lưu tại: .obsidian-vault/07-Superpowers/Specs/ hoặc .obsidian-vault/02-Specs/
```
- Viết spec chi tiết (Problem, Goal, API, DB, Edge cases, AC).
- Xác định actors, input, output.
- Liệt kê edge cases và acceptance criteria.

### 3. 📝 Plan
```
Lưu tại: .obsidian-vault/07-Superpowers/Plans/
```
- Liệt kê files cần tạo/sửa.
- Ước tính số tests.
- Xác định dependencies.
- Tạo step-by-step checklist.

### 4. 🔴 TDD — RED Phase
- Viết tất cả test cases mô tả hành vi.
- Chạy tests → tất cả FAIL (expected).
- **Evidence:** Lưu output test vào `.obsidian-vault/07-Superpowers/TDD/`.

### 5. 🟢 TDD — GREEN Phase
- Viết code tối thiểu để pass tests.
- Chạy tests → tất cả PASS.
- **Evidence:** Lưu output test vào `.obsidian-vault/07-Superpowers/TDD/`.

### 6. 🔵 TDD — REFACTOR Phase
- Clean up code.
- Improve naming.
- Thêm tests nếu cần.
- Chạy tests → vẫn PASS.
- Kiểm tra coverage ≥ target.

### 7. 🔍 Code Review
```
Lưu tại: .obsidian-vault/07-Superpowers/Code-Review/
```
- Self-review theo `[[Review-Checklist]]`.
- Kiểm tra security theo `[[Security-Rules]]`.

### 8. ✅ Verification
```bash
npm test                    # All tests pass
npm test -- --coverage      # Coverage ≥ target
npm run lint                # No lint errors
npm run build               # Build succeeds
```

### 9. 📚 Knowledge
- Cập nhật API docs tại `.obsidian-vault/05-API/`.
- Cập nhật DB docs nếu schema thay đổi tại `.obsidian-vault/06-Database/`.

## Checklist Tóm tắt

```
[ ] Spec exists?
[ ] Plan written?
[ ] Tests written (RED)?
[ ] Code written (GREEN)?
[ ] Refactored (BLUE)?
[ ] Code reviewed?
[ ] All tests pass?
[ ] Coverage ≥ target?
[ ] Build clean?
[ ] Knowledge updated?
```
