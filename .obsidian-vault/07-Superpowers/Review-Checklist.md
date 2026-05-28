---
title: Review Checklist
version: 1.0.0
created: 2026-05-28
---

# Review Checklist

> Chạy checklist này TRƯỚC KHI báo cáo hoàn thành bất kỳ feature nào.

## ✅ Iron Laws Check

- [ ] **Spec tồn tại** trong `.obsidian-vault/07-Superpowers/Specs/` hoặc `.obsidian-vault/02-Specs/` và đã được xem qua trước khi code.
- [ ] **Failing tests viết trước** (không có code nào viết trước test).
- [ ] **Có evidence**: test output với 0 failures (copy paste actual output).

---

## ✅ Code Quality

- [ ] TypeScript: không có `any` type.
- [ ] Không có `console.log` còn sót lại trong production code.
- [ ] Không có `TODO`, `FIXME`, `HACK` comments chưa được xử lý.
- [ ] Error handling: dùng đúng exceptions.
- [ ] Tất cả async functions có `await` đúng chỗ.
- [ ] Không có unused imports.
- [ ] `npm run lint` pass 0 errors.

---

## ✅ Testing

- [ ] Unit tests cho tất cả service methods (public).
- [ ] Integration test cho API endpoints.
- [ ] Test suite pass với 0 failures.
- [ ] Coverage ≥ 85% cho module mới.
- [ ] Edge cases đã được test (null, empty, boundary values).
- [ ] External services đã được mock.

---

## ✅ Security

- [ ] Route có auth guard nếu cần auth.
- [ ] Ownership verified trước khi mutation.
- [ ] Input validated với DTO.
- [ ] Không có secrets trong code.
- [ ] Rate limiting applied.

---

## ✅ Database

- [ ] Migration file được tạo đúng.
- [ ] Indexes đã được thêm cho FK và query fields.
- [ ] Transactions dùng cho multi-step operations.
- [ ] Money stored as integer cents (không phải float).

---

## ✅ API Contract

- [ ] Response format khớp với spec.
- [ ] HTTP status codes đúng (200, 201, 400, 401, 403, 404).
- [ ] Pagination implemented cho list endpoints.
- [ ] Error response format nhất quán.

---

## ✅ Git

- [ ] Commit message format đúng: `type(scope): description`.
- [ ] Không có merge conflicts.
- [ ] Branch name: `feature/{feature-name}` hoặc `fix/{bug-name}`.

---

## ✅ Documentation Update

- [ ] Tài liệu dự án trong `.obsidian-vault` được cập nhật nếu có thay đổi.

---

## 📋 Done Template

Khi báo cáo hoàn thành, dùng template này:

```
## Feature: [Feature Name]

### Evidence
```
test output (paste actual output):
✓ Tests: X passed, 0 failed
Coverage: XX%
```

### Spec Compliance
- [x] Acceptance criteria #1: [description]
- [x] Acceptance criteria #2: [description]

### Changes Made
- `src/path/file.ts` — Description
- `src/path/file.spec.ts` — X unit tests

### Notes
[Any important notes, trade-offs, or things to watch out for]
```
