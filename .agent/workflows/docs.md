---
title: Workflow — Update Documentation
version: v2.0.0
document_code: SOP-DEV-010
---

# 📚 WORKFLOW: CẬP NHẬT TÀI LIỆU (Update Docs)

## Khi nào dùng?
Sau khi code đã merge thành công, hoặc bất kỳ khi nào có thay đổi ảnh hưởng đến cách sử dụng, cấu hình, hoặc kiến trúc dự án.

---

## ⚔️ MỤC TIÊU

> **Code chạy nhưng không ai biết xài = Code vô dụng. Tài liệu tốt giúp đồng đội hiện tại và tương lai tiết kiệm hàng giờ.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Thêm tính năng mới mà không cập nhật README
2. ❌ Thêm biến môi trường mới mà không ghi vào `.env.example`
3. ❌ Thay đổi API mà không cập nhật API docs
4. ❌ Tài liệu viết sai so với code thực tế (outdated)
5. ❌ Kết thúc task mà không ghi Knowledge Item cho bài học rút ra

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: XÁC ĐỊNH PHẠM VI CẬP NHẬT (AI làm)

Dựa trên những gì vừa code xong, xác định tài liệu nào cần cập nhật:

```
📋 DOCS IMPACT ANALYSIS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Thay đổi vừa thực hiện:
  [Tóm tắt tính năng/bugfix vừa hoàn thành]

Tài liệu cần cập nhật:
  ✅ README.md               — [Có/Không] Lý do: [...]
  ✅ .env.example             — [Có/Không] Lý do: [...]
  ✅ API Documentation         — [Có/Không] Lý do: [...]
  ✅ CHANGELOG.md              — [Có/Không] Lý do: [...]
  ✅ Architecture docs         — [Có/Không] Lý do: [...]
  ✅ Inline code comments      — [Có/Không] Lý do: [...]
  ✅ Knowledge Items (KIs)     — [Có/Không] Lý do: [...]
  ✅ Onboarding guide          — [Có/Không] Lý do: [...]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 2: CẬP NHẬT README (AI làm, nếu cần)

#### 2.1 Checklist README

| Mục | Cần cập nhật khi... |
|---|---|
| **Prerequisites** | Thêm dependency hệ thống mới (Node version, DB, Redis...) |
| **Installation** | Bước cài đặt thay đổi |
| **Environment Variables** | Thêm biến `.env` mới |
| **Running** | Cách chạy app thay đổi |
| **Project Structure** | Thêm thư mục/module quan trọng |
| **Features** | Tính năng mới đáng kể |

#### 2.2 Cập nhật `.env.example`

Nếu có biến môi trường mới:
```bash
# .env.example — KHÔNG chứa giá trị thật
GOOGLE_CLIENT_ID=your_google_client_id_here
GOOGLE_CLIENT_SECRET=your_google_client_secret_here
VNPAY_MERCHANT_ID=your_merchant_id_here
```

---

### BƯỚC 3: CẬP NHẬT API DOCUMENTATION (AI làm, nếu cần)

#### 3.1 Nếu thêm/sửa API endpoint

Cập nhật tài liệu API theo format đã chuẩn hóa trong Spec:

```
📌 [METHOD] /api/v1/[resource]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 Mô tả: [Mục đích endpoint]
🔒 Auth: [Public / Bearer Token / Admin only]
📥 Request: [Body/Params]
📤 Response: [Success + Error examples]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 3.2 Nếu dùng Swagger/OpenAPI

Cập nhật file `swagger.yaml` hoặc decorators trong code.

#### 3.3 Nếu dùng Postman

Export collection mới và lưu vào dự án: `docs/postman/`

---

### BƯỚC 4: CẬP NHẬT CHANGELOG (AI làm)

Format CHANGELOG theo [Keep a Changelog](https://keepachangelog.com/):

```markdown
## [Unreleased]

### Added
- Đăng nhập bằng Google OAuth (#42)
- Rate limiting cho API authentication endpoints

### Changed
- Cập nhật response format cho user endpoints

### Fixed
- Nút thanh toán không phản hồi khi giỏ hàng trống (#38)

### Security
- Thêm CSRF protection cho form submissions
```

---

### BƯỚC 5: CẬP NHẬT INLINE COMMENTS & DOCSTRINGS (AI làm)

#### 5.1 Khi nào cần comment

| Cần comment ✅ | Không cần comment ❌ |
|---|---|
| Logic phức tạp, thuật toán | Code đã rõ ràng qua tên biến/hàm |
| Workaround cho bug thư viện | Getter/setter đơn giản |
| Business rule đặc biệt | `// increment counter` trên dòng `counter++` |
| Lý do chọn giải pháp này (WHY) | Mô tả code đang làm gì (WHAT) |

#### 5.2 Format JSDoc cho hàm quan trọng

```typescript
/**
 * Xử lý đăng ký tài khoản mới.
 * 
 * Validate input → kiểm tra email trùng → hash password → lưu DB → gửi email xác thực.
 * 
 * @param data - Dữ liệu đăng ký từ request body
 * @returns User object đã tạo (không chứa password hash)
 * @throws ConflictError - Nếu email đã tồn tại
 * @throws ValidationError - Nếu input không hợp lệ
 */
async function registerUser(data: RegisterInput): Promise<User> {
```

---

### BƯỚC 6: GHI NHẬN BÀI HỌC — KNOWLEDGE ITEMS (AI làm)

> **Mục tiêu:** Biến kinh nghiệm thành tài sản. Mỗi bug hóc búa, mỗi pattern mới, mỗi quyết định thiết kế quan trọng đều cần được ghi lại.

#### 6.1 Khi nào tạo Knowledge Item

- Phát hiện bug khó debug (mất > 30 phút)
- Tìm ra workaround cho limitation của thư viện
- Quyết định thiết kế quan trọng (trade-off)
- Pattern/anti-pattern mới học được
- Cấu hình phức tạp lần đầu setup

#### 6.2 Format Knowledge Item

```
📘 KNOWLEDGE ITEM
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 Tiêu đề: [Tóm tắt 1 dòng]
📅 Ngày: YYYY-MM-DD
🏷️ Tags: [typescript, database, auth, ...]

🔍 Bối cảnh (Context):
  [Đang làm gì? Trong dự án nào? Ở phần nào?]

❓ Vấn đề (Problem):
  [Vấn đề cụ thể gặp phải là gì?]
  [Triệu chứng? Error message?]

✅ Giải pháp (Solution):
  [Cách giải quyết chi tiết]
  [Code snippet nếu cần]

⚠️ Lưu ý (Gotchas):
  [Những điều cần tránh, edge case, limitation]

🔗 Tham khảo:
  [Link bài viết, SO answer, docs đã tham khảo]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 7: BÁO CÁO HOÀN TẤT TOÀN DIỆN (AI làm)

Đây là bước cuối cùng của toàn bộ quy trình 8 bước:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🏁 BÁO CÁO HOÀN TẤT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📌 Tính năng/Bug: [Tiêu đề]
📋 Spec: [link file Spec]
📋 Plan: [link file Plan]

📊 Kết quả:
  🧪 Tests:    [X/X] PASS
  🔍 Lint:     PASS
  🏗️ Build:    PASS
  🔒 Security: [X] issues (0 critical)
  📝 Docs:     Đã cập nhật

📂 Files changed:  [số file]
📝 Commits:        [số commit]
🧪 Tests added:    [số test mới]

📚 Docs updated:
  ✅ README.md
  ✅ API docs
  ✅ CHANGELOG.md
  ✅ Knowledge Items: [X] bài học mới

🎯 Trạng thái: ✅ HOÀN TẤT — Sẵn sàng release
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CHECKLIST TỔNG THỂ

```
- [ ] README đã cập nhật (nếu cần)?
- [ ] .env.example đã cập nhật (nếu có biến mới)?
- [ ] API documentation đã cập nhật?
- [ ] CHANGELOG đã ghi nhận?
- [ ] Inline comments cho logic phức tạp?
- [ ] Knowledge Items đã tạo (nếu có bài học)?
- [ ] Báo cáo hoàn tất đã tổng hợp?
- [ ] User/Tech Lead đã xác nhận release?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Docs là phần của Definition of Done** — Tính năng chưa có docs = tính năng chưa xong.
2. **Viết cho người khác, không phải cho mình** — Tưởng tượng đồng đội mới join đọc docs của bạn.
3. **Cập nhật, không viết mới** — Nếu đã có docs, sửa chứ không tạo file trùng.
4. **Code comments giải thích WHY, không phải WHAT** — Tên hàm/biến tốt không cần comment WHAT.
5. **Knowledge Items là vàng** — Mỗi KI tiết kiệm giờ cho tất cả mọi người trong tương lai.
