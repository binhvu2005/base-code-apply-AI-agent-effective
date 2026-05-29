---
title: Workflow — Audit & Code Review
version: v2.0.0
document_code: SOP-DEV-009
---

# 🛡️ WORKFLOW: AUDIT & XÉT DUYỆT CODE (Review)

## Khi nào dùng?
Sau khi CI/CD pipeline đã PASS, trước khi merge vào nhánh chính. Đây là bước rà soát chất lượng cuối cùng bởi con người (hoặc AI).

---

## ⚔️ MỤC TIÊU

> **Phát hiện những thứ mà máy không bắt được: lỗi logic nghiệp vụ, lỗ hổng bảo mật, code khó bảo trì, và vi phạm thiết kế.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Merge code mà chưa qua audit
2. ❌ Bỏ qua security warning mà không ghi chú lý do
3. ❌ Audit không đối chiếu lại với Spec ban đầu
4. ❌ Chỉ đọc lướt diff mà không phân tích logic

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: ĐỐI CHIẾU VỚI SPEC & PLAN (AI làm)

> **Mục tiêu:** Đảm bảo code đã triển khai đúng và đủ những gì Spec yêu cầu.

#### 1.1 Mở lại Spec và Plan

- Spec: `.obsidian-vault/07-Superpowers/Specs/YYYY-MM-DD-<topic>-design.md`
- Plan: `.obsidian-vault/07-Superpowers/Plans/YYYY-MM-DD-<topic>-plan.md`

#### 1.2 Cross-check

```
📋 SPEC vs CODE CROSS-CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Database:
  ✅ Bảng users — đã tạo đúng schema
  ✅ Bảng orders — đã tạo đúng schema
  ⚠️ Bảng payments — thiếu cột `refund_status` so với Spec

API Endpoints:
  ✅ POST /api/v1/auth/register — đúng request/response
  ✅ POST /api/v1/auth/login — đúng
  ❌ GET /api/v1/users/me — CHƯA IMPLEMENT (có trong Spec)

Business Logic:
  ✅ Validate email format — đúng
  ✅ Password hash bcrypt — đúng
  ⚠️ Rate limiting — Spec yêu cầu 5 req/phút, code set 10 req/phút

Plan Tasks:
  ✅ 15/17 tasks hoàn thành
  ❌ Task 2.6: Error handling cho timeout — chưa làm
  ❌ Task 3.4: Loading state UI — chưa làm

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 2: SECURITY AUDIT (AI làm)

> **Mục tiêu:** Phát hiện lỗ hổng bảo mật trước khi code lên production.

#### 2.1 Đối chiếu với `.agent/rules/security-rules.md`

#### 2.2 Security Checklist chi tiết

```
🔒 SECURITY AUDIT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔐 Authentication & Authorization:
  - [ ] Login có rate limiting?
  - [ ] Password được hash (bcrypt/argon2, KHÔNG dùng MD5/SHA)?
  - [ ] Token có expiration time hợp lý?
  - [ ] Refresh token được lưu an toàn (httpOnly cookie)?
  - [ ] Mỗi endpoint có kiểm tra quyền (authorization)?
  - [ ] Admin endpoints có middleware bảo vệ?

📥 Input Validation:
  - [ ] Validate ở cả Frontend VÀ Backend?
  - [ ] SQL Injection: dùng ORM/parameterized queries?
  - [ ] XSS: output được sanitize/escape?
  - [ ] File upload: kiểm tra type, size, tên file?
  - [ ] Path traversal: không cho phép `../` trong input?

🔑 Secrets & Configuration:
  - [ ] API keys trong `.env`, KHÔNG hardcode?
  - [ ] `.env` có trong `.gitignore`?
  - [ ] Không log sensitive data (password, token)?
  - [ ] CORS cấu hình đúng domain?
  - [ ] HTTPS bắt buộc (không HTTP)?

📊 Data Protection:
  - [ ] Sensitive fields không trả về trong API response (password hash, secrets)?
  - [ ] Dữ liệu cá nhân tuân thủ quy định (PDPL Việt Nam, GDPR)?
  - [ ] Backup strategy cho database?
  - [ ] Soft delete cho dữ liệu quan trọng?

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 3: PERFORMANCE AUDIT (AI làm)

```
⚡ PERFORMANCE AUDIT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🗃️ Database:
  - [ ] Có N+1 query không? (SELECT trong vòng lặp)
  - [ ] Các cột thường query đã có index?
  - [ ] Pagination cho list endpoints?
  - [ ] Không SELECT * — chỉ lấy cột cần thiết?

🌐 API:
  - [ ] Response time < 200ms cho các endpoint thường dùng?
  - [ ] Có caching cho data ít thay đổi?
  - [ ] File upload có limit size?
  - [ ] Large payload có compress (gzip)?

🖥️ Frontend:
  - [ ] Lazy loading cho images/components?
  - [ ] Bundle size hợp lý?
  - [ ] Không re-render không cần thiết?
  - [ ] Debounce cho search/input?

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 4: CODE QUALITY AUDIT (AI làm)

#### 4.1 Đối chiếu với `.agent/rules/coding-rules.md`

```
📝 CODE QUALITY AUDIT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🏗️ Kiến trúc:
  - [ ] Tách biệt rõ layers (Controller → Service → Repository)?
  - [ ] Không business logic trong Controller/Route?
  - [ ] Dependency injection đúng cách?

📛 Naming:
  - [ ] Tên biến/hàm mô tả đúng mục đích?
  - [ ] Không viết tắt khó hiểu (ví dụ: `usr` → `user`)?
  - [ ] Convention nhất quán (camelCase, PascalCase...)?

📦 Modularity:
  - [ ] Hàm ≤ 20 dòng (ngoại trừ config)?
  - [ ] File ≤ 200 dòng?
  - [ ] Mỗi file/module có 1 trách nhiệm duy nhất (SRP)?

🧹 Clean Code:
  - [ ] Không code trùng lặp (DRY)?
  - [ ] Không magic number/string?
  - [ ] Không dead code (code comment out)?
  - [ ] Error handling đầy đủ (try/catch, error boundaries)?
  - [ ] Logging đầy đủ cho debugging?

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 5: TỔNG HỢP BÁO CÁO AUDIT (AI làm)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 AUDIT SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Spec Coverage:  [X/Y] requirements implemented
🔒 Security:       [X] issues found ([Z] critical)
⚡ Performance:    [X] issues found
📝 Code Quality:   [X] issues found

🔴 MUST FIX (Blocking — không merge nếu chưa sửa):
  1. [Mô tả issue] — File: [path]
  2. ...

🟡 SHOULD FIX (Nên sửa trước merge):
  1. [Mô tả issue] — File: [path]
  2. ...

🟢 NICE TO HAVE (Tạo task riêng cho sprint sau):
  1. [Mô tả issue] — File: [path]
  2. ...

🎯 Kết luận:
  ✅ APPROVED — Sẵn sàng merge
  ⚠️ APPROVED WITH CONDITIONS — Sửa [X] items rồi merge
  ❌ REJECTED — Cần sửa [X] critical issues

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 6: XỬ LÝ SAU AUDIT (Bạn quyết định)

| Kết quả | Hành động |
|---|---|
| ✅ APPROVED | → Tạo PR, merge vào nhánh chính |
| ⚠️ APPROVED WITH CONDITIONS | → Sửa items cần thiết → Chạy lại CI/CD → Merge |
| ❌ REJECTED | → Sửa critical issues → Chạy lại từ `/cicd` → Audit lại |

---

## CHECKLIST TỔNG THỂ

```
- [ ] Đã đối chiếu code vs Spec?
- [ ] Tất cả Spec requirements đã implement?
- [ ] Security checklist đã review?
- [ ] Performance checklist đã review?
- [ ] Code quality checklist đã review?
- [ ] MUST FIX items = 0?
- [ ] Báo cáo audit đã tổng hợp?
- [ ] User/Tech Lead đã review kết quả audit?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Spec là nguồn sự thật** — Code phải match Spec. Nếu code khác Spec, phải có lý do và ghi chú.
2. **Security first** — 1 lỗ hổng bảo mật có thể phá hủy toàn bộ dự án. Không bao giờ bỏ qua.
3. **Phân loại severity rõ ràng** — MUST FIX / SHOULD FIX / NICE TO HAVE. Không gộp chung.
4. **Không merge khi có MUST FIX** — Dù deadline có gấp. An toàn hơn nhanh.
5. **Audit là cơ hội học hỏi** — Ghi nhận các pattern lỗi thường gặp để tránh lặp lại.
