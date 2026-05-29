---
title: Workflow — Git Commit
version: v2.0.0
document_code: SOP-DEV-007
---

# 💾 WORKFLOW: QUẢN LÝ PHIÊN BẢN (Git Commit)

## Khi nào dùng?
Sau mỗi TDD cycle hoàn tất, sau khi sửa bug, hoặc bất kỳ lúc nào bạn muốn lưu lại một trạng thái ổn định của code.

---

## ⚔️ MỤC TIÊU

> **Tạo lịch sử Git sạch sẽ, minh bạch, có thể revert bất kỳ lúc nào mà không sợ mất dữ liệu.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Commit code mà test đang FAIL
2. ❌ Commit nhầm file nhạy cảm (`.env`, API keys, secrets)
3. ❌ Commit message kiểu `"fix"`, `"update"`, `"asdf"`
4. ❌ Gộp nhiều thay đổi không liên quan vào 1 commit
5. ❌ Không review diff trước khi commit

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: KIỂM TRA TRẠNG THÁI (AI làm)

```bash
git status
```

#### 1.1 Phân loại file thay đổi

```
📋 GIT STATUS REVIEW
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Cần commit:
  M  src/services/UserService.ts        (code production)
  A  tests/services/UserService.test.ts  (test mới)

⚠️ Cần xem xét:
  M  package.json                       (dependency mới?)
  M  package-lock.json                  (auto-generated)

🚫 KHÔNG ĐƯỢC commit:
  ?  .env                               (secrets!)
  ?  node_modules/                      (dependencies)
  ?  *.log                              (log files)
  ?  .DS_Store                          (OS files)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 1.2 Kiểm tra .gitignore

Đảm bảo các file/thư mục sau có trong `.gitignore`:
```
node_modules/
.env
.env.*
*.log
.DS_Store
dist/
coverage/
.worktrees/
```

---

### BƯỚC 2: SOÁT LỖI BẰNG DIFF (AI làm)

```bash
git diff              # Xem thay đổi chưa staged
git diff --staged     # Xem thay đổi đã staged
```

#### 2.1 Checklist review diff

| Hạng mục | Kiểm tra |
|---|---|
| **Debug code** | Còn `console.log`, `debugger`, `TODO` nào quên xóa? |
| **Commented code** | Còn code bị comment out mà không cần? |
| **Hardcoded values** | Có string/number magic nào cần chuyển thành constant? |
| **Sensitive data** | Có API key, password, token nào bị lộ? |
| **Import thừa** | Có import nào không sử dụng? |
| **Formatting** | Code đã chạy qua formatter (Prettier/ESLint)? |

#### 2.2 Nếu phát hiện vấn đề

```
⚠️ PHÁT HIỆN TRONG DIFF
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 File: src/services/UserService.ts:42
🔍 Vấn đề: console.log("debug user data") còn sót
🔧 Xử lý: Xóa trước khi commit
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 3: CHẠY TEST LẦN CUỐI (AI làm)

> **LUẬT:** Không commit nếu test FAIL.

```bash
npm run lint        # Lint clean
npm test            # All tests PASS
npm run build       # Build thành công (nếu cần)
```

```
✅ PRE-COMMIT CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Lint: PASS
🧪 Test: [X/X] PASS
🏗️ Build: PASS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 4: PHÂN CHIA COMMIT (AI làm)

> **LUẬT:** Atomic Commits — Mỗi commit chỉ chứa MỘT thay đổi logic.

#### 4.1 Khi nào tách commit

| Tình huống | Số commit |
|---|---|
| Sửa 1 bug | 1 commit |
| Thêm 1 tính năng (test + code) | 1-2 commits |
| Sửa 2 bug không liên quan | 2 commits riêng |
| Thêm tính năng + sửa bug cũ | 2 commits riêng |
| Refactor + thêm tính năng | 2 commits riêng |

#### 4.2 Cách staged từng phần

```bash
# Staged từng file cụ thể (KHÔNG dùng `git add .` bừa bãi)
git add src/services/UserService.ts
git add tests/services/UserService.test.ts

# Hoặc staged từng hunk trong 1 file
git add -p src/services/UserService.ts
```

---

### BƯỚC 5: VIẾT COMMIT MESSAGE (AI làm)

#### 5.1 Conventional Commits Format

```
<type>(<scope>): <subject>

[body — optional]

[footer — optional]
```

#### 5.2 Bảng Type

| Type | Khi nào dùng | Ví dụ |
|---|---|---|
| `feat` | Tính năng mới | `feat(auth): add Google OAuth login` |
| `fix` | Sửa bug | `fix(cart): prevent checkout with empty cart` |
| `test` | Thêm/sửa test | `test(auth): add test for duplicate email` |
| `refactor` | Tái cấu trúc (không đổi hành vi) | `refactor(users): extract validation to helper` |
| `docs` | Chỉ sửa tài liệu | `docs(readme): update setup instructions` |
| `style` | Format code (không đổi logic) | `style(global): apply prettier formatting` |
| `chore` | Cấu hình, dependencies | `chore(deps): update vitest to v2.0` |
| `ci` | CI/CD pipeline | `ci(github): add test workflow` |
| `perf` | Tối ưu hiệu năng | `perf(query): add index for user lookup` |

#### 5.3 Quy tắc viết Subject

| Quy tắc | Ví dụ tốt ✅ | Ví dụ xấu ❌ |
|---|---|---|
| Bắt đầu bằng động từ | `add`, `fix`, `remove`, `update` | `added`, `fixing` |
| Viết thường chữ cái đầu | `add login button` | `Add Login Button` |
| Không dấu chấm cuối | `add login button` | `add login button.` |
| ≤ 72 ký tự | `fix cart total calculation` | `fix the bug where cart total...` (quá dài) |
| Mô tả WHAT, không phải HOW | `prevent empty cart checkout` | `add if check for cart length` |

#### 5.4 Khi nào cần Body

Viết body khi commit phức tạp, cần giải thích **WHY**:

```
fix(cart): prevent checkout with empty cart

Trước đây nút checkout không kiểm tra giỏ hàng trống,
dẫn đến tạo đơn hàng rỗng trong database.

Thêm validation check ở CartService.checkout()
trước khi gọi OrderService.create().

Closes #123
```

---

### BƯỚC 6: THỰC HIỆN COMMIT (AI làm)

```bash
git add [files]
git commit -m "type(scope): subject"
```

```
💾 COMMIT REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 Message: feat(auth): add Google OAuth login
📂 Files: 3 files changed, 87 insertions(+), 2 deletions(-)
🔗 Hash: abc1234
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CHECKLIST TỔNG THỂ

```
- [ ] git status đã kiểm tra?
- [ ] Không có file nhạy cảm (.env, secrets)?
- [ ] git diff đã review?
- [ ] Debug code đã xóa (console.log, debugger)?
- [ ] Lint PASS?
- [ ] Test PASS?
- [ ] Build PASS (nếu cần)?
- [ ] Commit atomic (1 thay đổi logic / commit)?
- [ ] Message đúng Conventional Commits?
- [ ] Plan đã cập nhật tiến độ?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Review diff trước commit** — Đọc lại mọi dòng thay đổi. Đây là cơ hội cuối để bắt lỗi.
2. **Atomic commits** — 1 commit = 1 mục đích. Không gộp bừa.
3. **Message có nghĩa** — Người đọc git log phải hiểu được bạn đã làm gì mà không cần mở code.
4. **Không commit code lỗi** — Test phải PASS. Không ngoại lệ.
5. **Không commit secrets** — Kiểm tra `.gitignore` mỗi khi thêm file mới vào dự án.
