---
title: Workflow — CI/CD Pipeline
version: v2.0.0
document_code: SOP-DEV-008
---

# 🚀 WORKFLOW: TÍCH HỢP LIÊN TỤC (CI/CD)

## Khi nào dùng?
Sau khi hoàn tất code và commit, trước khi tạo Pull Request hoặc merge vào nhánh chính. Đây là bước kiểm tra tự động cuối cùng.

---

## ⚔️ MỤC TIÊU

> **Không có lỗi nào lọt qua. Nếu Lint lỗi, Test lỗi, hoặc Build lỗi thì KHÔNG ĐƯỢC tạo PR.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Tạo PR khi lint còn lỗi
2. ❌ Tạo PR khi test còn FAIL
3. ❌ Tạo PR khi build không thành công
4. ❌ Báo cáo "CI pass" mà không có log terminal thực tế
5. ❌ Bỏ qua warning mà không giải thích lý do

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: AUTO LINTING & FORMATTING (AI làm)

> **Mục tiêu:** Code phải tuân thủ 100% quy chuẩn format của dự án.

#### 1.1 Chạy Lint

```bash
npm run lint
```

#### 1.2 Xử lý kết quả

| Kết quả | Hành động |
|---|---|
| ✅ 0 errors, 0 warnings | Chuyển sang Bước 2 |
| ⚠️ 0 errors, N warnings | Xem xét từng warning, sửa nếu cần hoặc ghi chú lý do bỏ qua |
| ❌ N errors | **BẮT BUỘC sửa hết** trước khi tiếp tục |

#### 1.3 Auto-fix (nếu có thể)

```bash
npm run lint -- --fix         # ESLint auto-fix
npx prettier --write .        # Prettier format
```

#### 1.4 Báo cáo

```
🔍 LINT REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Kết quả: [PASS / FAIL]
❌ Errors: [số lượng]
⚠️ Warnings: [số lượng]
🔧 Auto-fixed: [số lượng files]

Chi tiết lỗi (nếu có):
  📍 src/services/UserService.ts:42
     ❌ no-unused-vars: 'tempData' is defined but never used
  📍 src/routes/auth.ts:15
     ⚠️ no-console: Unexpected console.log

📋 Log terminal: [paste output]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 2: AUTO TESTING — TOÀN BỘ TEST SUITE (AI làm)

> **Mục tiêu:** Chạy TẤT CẢ tests trong hệ thống, không chỉ tests của tính năng mới.

#### 2.1 Chạy Test

```bash
npm test                      # Toàn bộ test suite
npm test -- --coverage        # Kèm báo cáo coverage (nếu có)
```

#### 2.2 Phân tích kết quả

```
🧪 TEST REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Kết quả: [PASS / FAIL]

Test Suites: [X] passed, [Y] failed, [Z] total
Tests:       [X] passed, [Y] failed, [Z] total
Time:        [X.XX]s

Coverage (nếu có):
  Statements: [XX]%
  Branches:   [XX]%
  Functions:  [XX]%
  Lines:      [XX]%

Chi tiết test FAIL (nếu có):
  ❌ UserService > should validate email format
     Expected: "invalid_email" to throw ValidationError
     Received: No error thrown
     File: tests/services/UserService.test.ts:28

📋 Log terminal: [paste output]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 2.3 Nếu test FAIL

```
⚠️ TEST FAILURE ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
❌ Test: [tên test bị fail]
📍 File: [đường dẫn]

🔍 Phân loại:
  [ ] Test cũ bị fail → REGRESSION → Quay lại sửa code
  [ ] Test mới bị fail → Code chưa đúng → Quay lại TDD cycle
  [ ] Test flaky (lúc pass lúc fail) → Fix test stability

🚫 KHÔNG ĐƯỢC tạo PR khi còn test FAIL
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 3: BUILD CHECK (AI làm)

> **Mục tiêu:** Đảm bảo TypeScript compile thành công, không có lỗi type.

```bash
npm run build
```

```
🏗️ BUILD REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Kết quả: [SUCCESS / FAIL]
⏱️ Thời gian: [X.XX]s
📦 Output: [dist/ hoặc build/]

Chi tiết lỗi (nếu có):
  ❌ src/services/UserService.ts:42:10
     error TS2345: Argument of type 'string' is not assignable
     to parameter of type 'number'.

📋 Log terminal: [paste output]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 4: SECURITY CHECK (AI làm — nếu có tool)

#### 4.1 Kiểm tra Dependencies

```bash
npm audit                     # Kiểm tra vulnerabilities
npm outdated                  # Kiểm tra dependencies cũ
```

#### 4.2 Báo cáo

```
🔒 SECURITY REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 npm audit:
  🔴 Critical: [số lượng]
  🟠 High: [số lượng]
  🟡 Moderate: [số lượng]
  🟢 Low: [số lượng]

Chi tiết Critical/High (nếu có):
  🔴 lodash < 4.17.21 — Prototype Pollution
     Fix: npm audit fix

📊 npm outdated:
  [package] current: X.X → latest: Y.Y
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 5: TỔNG HỢP BÁO CÁO CI/CD (AI làm)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 CI/CD PIPELINE SUMMARY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔍 Lint:      ✅ PASS (0 errors, 0 warnings)
🧪 Test:      ✅ PASS (42/42 tests, 3.2s)
🏗️ Build:     ✅ PASS (2.1s)
🔒 Security:  ✅ PASS (0 vulnerabilities)

📊 Coverage:  87% statements, 82% branches

🎯 Kết luận:  ✅ SẴN SÀNG TẠO PR
              ❌ CẦN SỬA TRƯỚC KHI TẠO PR (nếu có lỗi)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 6: QUYẾT ĐỊNH TIẾP THEO (Bạn quyết định)

| Kết quả CI/CD | Hành động |
|---|---|
| ✅ Tất cả PASS | → Chuyển sang `/audit` để review code |
| ❌ Lint FAIL | → Sửa lint errors → Chạy lại từ Bước 1 |
| ❌ Test FAIL | → Quay lại `/tdd` để fix → Chạy lại từ Bước 2 |
| ❌ Build FAIL | → Fix TypeScript errors → Chạy lại từ Bước 3 |
| ⚠️ Security warnings | → Đánh giá mức độ, quyết định fix ngay hay tạo task riêng |

---

## CHECKLIST TỔNG THỂ

```
- [ ] Lint: 0 errors?
- [ ] Lint: warnings đã xem xét?
- [ ] Test: toàn bộ test suite PASS?
- [ ] Test: không có test bị skip/pending quên?
- [ ] Build: compile thành công?
- [ ] Build: không có TypeScript error?
- [ ] Security: không có critical/high vulnerabilities?
- [ ] Log terminal đã lưu làm bằng chứng?
- [ ] Sẵn sàng tạo PR?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Chạy thật, không giả định** — Phải chạy lệnh và paste log. Không "chắc là pass".
2. **Lint trước Test trước Build** — Theo đúng thứ tự. Sửa lint trước mới chạy test.
3. **0 errors = bắt buộc** — Warnings có thể chấp nhận nếu có lý do. Errors thì không.
4. **Không tạo PR khi pipeline FAIL** — Dù chỉ 1 step fail cũng phải sửa xong mới được tiếp.
5. **Evidence before report** — Mọi báo cáo kèm log terminal thực tế.
