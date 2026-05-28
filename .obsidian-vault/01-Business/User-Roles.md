---
title: User Roles
author: Solo Developer
created: 2026-05-28
---

# User Roles

> Phân định các vai trò (roles) trong hệ thống và ma trận quyền hạn tương ứng.

## 1. Actor Definitions

### 👤 Guest (Chưa đăng nhập)
- Người dùng vãng lai truy cập hệ thống.
- Quyền hạn: Chỉ xem các tài nguyên công khai, đăng ký tài khoản, đăng nhập.

### 👤 User / Client (Người dùng thông thường)
- Người dùng đã xác thực danh tính.
- Quyền hạn: Quản lý thông tin cá nhân, thực hiện các thao tác nghiệp vụ cơ bản.

### 👑 Admin (Quản trị viên)
- Người quản trị hệ thống.
- Quyền hạn: Quản lý người dùng, cấu hình hệ thống, xem dashboard phân tích, giải quyết tranh chấp.

---

## 2. Permission Matrix

| Tác vụ | Guest | User | Admin |
|--------|-------|------|-------|
| Đăng ký / Đăng nhập | ✅ | ➖ | ➖ |
| Xem nội dung public | ✅ | ✅ | ✅ |
| Quản lý profile cá nhân | ❌ | ✅ | ✅ |
| Thực hiện thao tác nghiệp vụ chính | ❌ | ✅ | ✅ |
| Quản trị hệ thống | ❌ | ❌ | ✅ |
