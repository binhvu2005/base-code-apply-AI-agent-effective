---
title: Workflow — Security Audit & Hardening
version: v2.0.0
document_code: SOP-DEV-014
---

# 🔒 WORKFLOW: RÀ SOÁT & TĂNG CƯỜNG BẢO MẬT (Security Hardening)

## Khi nào dùng?
- Mỗi khi thiết kế hoặc implement một tính năng liên quan đến xác thực (Auth), phân quyền (Role/Permission), thanh toán, hoặc xử lý dữ liệu nhạy cảm của người dùng.
- Trước khi release một phiên bản chính thức (Production).
- Khi phát hiện hoặc nghi ngờ có lỗ hổng bảo mật trong hệ thống.
- **Slash command:** `/security`

---

## ⚔️ MỤC TIÊU

> **Đảm bảo ứng dụng an toàn trước các cuộc tấn công phổ biến (OWASP Top 10), bảo mật thông tin cá nhân của người dùng, và không rò rỉ bất kỳ thông tin nhạy cảm nào ra môi trường bên ngoài.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI (REJECT lập tức)

1. ❌ Chứa Secrets, API Keys, Passwords hardcode trong mã nguồn hoặc Git History.
2. ❌ Tồn tại API endpoint thay đổi dữ liệu (POST, PATCH, DELETE) mà thiếu kiểm tra phân quyền sở hữu dữ liệu (Ownership Check).
3. ❌ Không validate hoặc sanitize đầu vào từ người dùng (dẫn đến nguy cơ SQL Injection, XSS, Path Traversal).
4. ❌ API response trả về thông tin nhạy cảm (như mật khẩu đã hash, refresh token, thông tin hệ thống nội bộ).
5. ❌ Bỏ qua các cảnh báo bảo mật nghiêm trọng (`npm audit` có lỗi Critical/High) mà không có biện pháp xử lý.

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: THREAT MODELING — PHÂN TÍCH RỦI RO (AI làm)

Trước khi viết code bảo mật, hãy tự đặt câu hỏi và vẽ luồng tấn công tiềm ẩn:

```
              [ Kẻ Tấn Công (Attacker) ]
                          │
          (Thử SQL Injection, XSS, DDOS)
                          ▼
    [ Cửa ngõ API / Client Frontend ] ──(Lộ secrets?)──► [ Git/Config ]
                          │
             (Bypass Auth/Guard?)
                          ▼
            [ Service Logic / Helpers ] ──(Data Leak?)──► [ Web Logs ]
                          │
             (Bypass Ownership Check?)
                          ▼
               [ Cơ sở dữ liệu (DB) ]
```

#### 1.1 Khảo sát các điểm nhạy cảm:
- Endpoint nào xử lý input trực tiếp từ client?
- Endpoint nào cần phân quyền (Admin, User, Owner)?
- Dữ liệu nào cần mã hóa khi lưu trữ (at rest) và khi truyền tải (in transit)?

---

### BƯỚC 2: RÀ SOÁT XÁC THỰC & PHÂN QUYỀN (Auth & Authz Review)

> **Mục tiêu:** Không để người dùng chưa xác thực truy cập tài nguyên bảo mật, và không để người dùng này chỉnh sửa dữ liệu của người dùng khác.

#### 2.1 Luật Token (JWT Rules):
- Access Token: Hết hạn tối đa **15 phút**.
- Refresh Token: Hết hạn tối đa **30 ngày**, được lưu trong `httpOnly` cookie kèm cờ `Secure` và `SameSite=Strict`.
- Payload JWT không được chứa dữ liệu nhạy cảm (như số điện thoại, mật khẩu, địa chỉ...). Chỉ chứa `{ sub, email, role, iat, exp }`.

#### 2.2 Xác thực sở hữu dữ liệu (Ownership Verification):
Mọi hành động Mutation (Update, Delete) phải verify xem `userId` trong token có trùng khớp với chủ sở hữu của record đó trong DB hay không.

```typescript
// ❌ DANGEROUS: Cho phép update bất kỳ order nào nếu biết ID
async function updateOrder(orderId: string, data: UpdateOrderDto) {
  return db.order.update({ where: { id: orderId }, data });
}

// ✅ SAFE: Phải xác minh chủ sở hữu hoặc role Admin
async function updateOrder(orderId: string, userId: string, data: UpdateOrderDto) {
  const order = await db.order.findUnique({ where: { id: orderId } });
  if (!order) throw new NotFoundError("Order not found");
  if (order.userId !== userId && userRole !== "ADMIN") {
    throw new ForbiddenError("You do not own this resource");
  }
  return db.order.update({ where: { id: orderId }, data });
}
```

---

### BƯỚC 3: PHÒNG CHỐNG TIÊM NHIỄM & INPUT VALIDATION (Injection & XSS Prevention)

#### 3.1 Validate & Sanitize Input ở lớp Boundary:
- Sử dụng schema validator (Zod, Joi, class-validator) để ép kiểu chặt chẽ đầu vào.
- Tuyệt đối không nhận payload dư thừa (sử dụng whitelist thay vì blacklist).

#### 3.2 SQL Injection Prevention:
- Luôn dùng ORM (Prisma, TypeORM, Mongoose) hoặc parameterized query.
- Cấm string concatenation để build câu truy vấn DB.

```javascript
// ❌ DANGEROUS: String concatenation
const query = `SELECT * FROM users WHERE email = '${inputEmail}'`;

// ✅ SAFE: Parameterized Query
const query = {
  text: 'SELECT * FROM users WHERE email = $1',
  values: [inputEmail],
};
```

#### 3.3 Path Traversal Prevention:
Khi xử lý đường dẫn file từ client upload hoặc download:
- Khử sạch các ký tự đặc biệt như `../` hoặc `..\\`.
- Sử dụng `path.basename()` để chỉ lấy tên file an toàn.

---

### BƯỚC 4: QUẢN LÝ SECRETS & CẤU HÌNH (Secrets Management)

#### 4.1 Quét lộ lọt thông tin (Secrets Scan):
AI phải quét qua toàn bộ các thay đổi trong diff để chắc chắn không chứa:
- API Keys (AWS Access Key, Stripe Secret, Google API Key...)
- Passwords, DB Connection strings
- Private Keys (.pem, .key)

#### 4.2 Cấu hình môi trường an toàn:
- Toàn bộ config nhạy cảm phải nạp từ biến môi trường `process.env`.
- File `.env` chứa giá trị thật phải nằm trong danh sách `.gitignore`.
- Chỉ commit `.env.example` chứa các biến rỗng mẫu.

---

### BƯỚC 5: MÃ HÓA & BẢO VỆ DỮ LIỆU (Data Protection)

#### 5.1 Mã hóa Mật khẩu:
- Sử dụng thuật toán băm mạnh: `bcrypt` (với salt rounds >= 10) hoặc `argon2`.
- Tuyệt đối cấm dùng các hàm băm lỗi thời hoặc nhanh: `MD5`, `SHA1`, `SHA256` (không muối).

#### 5.2 Mã hóa dữ liệu nhạy cảm trong DB:
- Dữ liệu như số thẻ tín dụng, số định danh cá nhân cần được mã hóa đối xứng (ví dụ AES-256-GCM) trước khi ghi xuống database.

#### 5.3 Lọc dữ liệu đầu ra (Output Masking/Sanitization):
Trước khi trả dữ liệu về phía Frontend qua API, bắt buộc phải loại bỏ các trường nhạy cảm thông qua DTO serialization.

```typescript
// ✅ SAFE: Định nghĩa rõ ràng các trường được trả về
function toUserResponse(user: User) {
  return {
    id: user.id,
    email: user.email,
    fullName: user.fullName,
    role: user.role,
    // Không bao giờ bao gồm: passwordHash, salt, tempToken
  };
}
```

---

### BƯỚC 6: VÁ LỖ HỔNG & XỬ LÝ SỰ CỐ (Patching & Incident Response)

Khi phát hiện thư viện bên thứ ba bị cảnh báo bảo mật nặng (Vulnerability):

#### 6.1 Chạy công cụ kiểm định phụ thuộc:
```bash
npm audit
```

#### 6.2 Thực hiện nâng cấp an toàn:
1. Xem thông báo lỗ hổng ảnh hưởng đến package nào.
2. Nâng cấp lên bản vá tối thiểu bằng `npm audit fix` hoặc cài đặt package cụ thể với phiên bản an toàn:
   ```bash
   npm install package-name@latest
   ```
3. Chạy lại toàn bộ test suite để đảm bảo việc nâng cấp không gây ra lỗi tương thích ngược (Regression).

---

### BƯỚC 7: TỔNG HỢP BÁO CÁO BẢO MẬT (Security Audit Report)

Khi kết thúc quy trình rà soát, AI tổng hợp báo cáo bảo mật theo format sau:

```
🛡️ SECURITY AUDIT REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 Thời điểm: [YYYY-MM-DD HH:MM]
👨‍💻 Thực hiện: AI Agent

🔍 KẾT QUẢ ĐÁNH GIÁ:
  - [x] Không phát hiện API Keys/Secrets hardcode trong code.
  - [x] Đã áp dụng Auth Guard cho các API Endpoints mới: [Danh sách endpoint]
  - [x] Đã thiết lập Ownership Check cho hành động Mutation.
  - [x] Input validation đã được kiểm tra (Zod/class-validator).
  - [x] npm audit kết quả: 0 Critical, 0 High vulnerabilities.

🚨 LỖ HỔNG PHÁT HIỆN & ĐÃ XỬ LÝ (nếu có):
  1. Lỗ hổng: [Tên lỗ hổng, ví dụ: SQL Injection qua Search]
     - Mức độ: [High / Medium / Low]
     - File: [Đường dẫn file]
     - Cách xử lý: [Chuyển sang dùng parameterized query]

  2. Thư viện lỗi: [Ví dụ: lodash < 4.17.21]
     - Mức độ: [High]
     - Cách xử lý: [Chạy npm install lodash@4.17.21 để vá lỗi]

🎯 ĐÁNH GIÁ CHUNG: ✅ ĐẠT TIÊU CHUẨN AN TOÀN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CHECKLIST TỔNG THỂ BẢO MẬT

```
- [ ] Không có secrets, passwords, hay API keys bị hardcode trong code?
- [ ] Mật khẩu người dùng được hash bằng bcrypt (rounds >= 10) hoặc argon2?
- [ ] Mọi API thay đổi dữ liệu đều có middleware kiểm tra quyền hạn (Auth & Roles)?
- [ ] Đã kiểm tra quyền sở hữu (Ownership Check) trước khi cập nhật hoặc xóa dữ liệu?
- [ ] Đầu vào từ API đã qua validation schema chặt chẽ (Zod/Joi...)?
- [ ] Không sử dụng câu lệnh truy vấn SQL ghép chuỗi trực tiếp từ input client?
- [ ] Dữ liệu đầu ra API đã loại bỏ hết các cột nhạy cảm (password, tokens)?
- [ ] Config nhạy cảm nằm trong .env và .env đã được đưa vào .gitignore?
- [ ] npm audit chạy không còn lỗi bảo mật Critical hay High?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Zero Trust** — Luôn giả định dữ liệu gửi từ Frontend lên có thể bị làm giả. Mọi kiểm tra phân quyền bắt buộc phải làm lại ở Backend.
2. **Secrets Never Go to Git** — Nếu lỡ tay commit secrets lên Git, bạn phải đổi key đó ngay lập tức và tiến hành lọc lịch sử Git (dùng `git-filter-repo` hoặc BFG) trước khi push.
3. **Least Privilege** — Phân quyền ở mức tối thiểu cần thiết để chức năng hoạt động. Không cấp quyền Admin cho user thường.
4. **Fail Securely** — Khi xảy ra lỗi hoặc ngoại lệ (Exception), hãy log chi tiết ở server nhưng chỉ trả về message chung chung cho client. Tránh hiển thị Stack Trace của server cho user ngoài.
