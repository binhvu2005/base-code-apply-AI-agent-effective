# Kế hoạch Kiểm thử — Test Plan

Tài liệu này định nghĩa chiến lược kiểm thử toàn bộ dự án bao gồm Frontend, Backend và các dịch vụ tích hợp bên ngoài.

---

## 🧪 Chiến lược Kiểm thử (Testing Strategy)

Chúng ta tuân thủ nghiêm ngặt nguyên lý **Test-Driven Development (TDD)** theo quy trình:
1. Viết Test lỗi trước (RED).
2. Viết Code tối thiểu để Test qua (GREEN).
3. Tối ưu hóa & Làm sạch code (REFACTOR).

---

## 🛠 Bộ công cụ kiểm thử (Test Stack)

- **Backend**: Jest / Vitest làm Test Runner và Mocking library.
- **Frontend**: Jest / Vitest kết hợp với React Testing Library cho unit test component, và Playwright/Cypress cho End-to-End (E2E) testing.
- **Database**: Test database riêng biệt cho kiểm thử tích hợp (Integration tests).

---

## 📋 Các đầu mục kiểm thử chính

### 1. Unit Tests (Kiểm thử đơn vị)
- **Mã nguồn**: Đặt cạnh file logic (ví dụ: `users.service.spec.ts`).
- **Nghiệp vụ quan trọng cần test**:
  - Các hàm tính toán số liệu, logic nghiệp vụ phức tạp.
  - Các hàm validate đầu vào và phân quyền truy cập.

### 2. Integration Tests (Kiểm thử tích hợp)
- **Thư mục**: `test/` hoặc `src/**/test/` (e2e testing).
- **Luồng tích hợp cần test**:
  - Đăng ký → Đăng nhập → Lấy Token → Xác thực endpoint bảo mật.
  - CRUD các thực thể chính kết nối trực tiếp với Test Database.

### 3. End-to-End (E2E) Tests
- **Công cụ**: Playwright hoặc Cypress.
- **Nghiệm vụ cần giả lập**:
  - Luồng mua hàng/thanh toán thành công.
  - Luồng tạo tài khoản và cài đặt cá nhân.
