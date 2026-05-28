---
title: Review Checklist
version: v1.0.0
document_code: SOP-DEV-001
---

# Review Checklist

> Chạy checklist này TRƯỚC KHI báo cáo hoàn thành bất kỳ feature nào.

## ✅ Iron Laws Check

- [ ] **SOP-DEV-001 Quy trình 5 bước tuân thủ**:
  - [ ] **Spec tồn tại**: `.obsidian-vault/07-Superpowers/Specs/YYYY-MM-DD-<ten-tinh-nang>-design.md` đã được duyệt.
  - [ ] **Plan tồn tại**: `.obsidian-vault/07-Superpowers/Plans/YYYY-MM-DD-<ten-tinh-nang>-plan.md` đã được chia nhỏ và cập nhật tích xanh `- [x]` tiến độ.
  - [ ] **Môi trường cô lập**: Thực hiện trên branch cô lập hoặc worktree riêng.
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
- [ ] Lịch sử commit rõ ràng, chia nhỏ theo từng task trong Plan.
- [ ] Không có merge conflicts.
- [ ] Branch name: `feature/<ten-tinh-nang>` hoặc `fix/<bug-name>`.

---

## ✅ Documentation Update

- [ ] Tài liệu dự án cập nhật nếu có thay đổi trong `.obsidian-vault/`.

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
