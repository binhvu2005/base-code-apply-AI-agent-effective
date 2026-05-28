---
title: Database Design
author: Solo Developer
created: 2026-05-28
---

# Database Design

## 1. Database Philosophy
- Sử dụng Database nào (PostgreSQL, MySQL, MongoDB, v.v.)?
- Tại sao chọn? (ACID compliance, JSON support, scale, ...)

## 2. Naming Conventions
- Bảng: `snake_case` số nhiều (ví dụ: `users`, `user_profiles`).
- Cột: `snake_case` (ví dụ: `created_at`, `owner_id`).
- Khóa ngoại: `target_table_id` (ví dụ: `user_id`).

## 3. General Best Practices
- **Primary Keys**: Khuyên dùng UUID hoặc ULID thay vì Auto-Increment Integer để tăng tính bảo mật và dễ scale.
- **Indexes**:
  - Tự động tạo index cho Khóa ngoại (Foreign Keys).
  - Thêm index cho các cột thường xuyên xuất hiện trong mệnh đề `WHERE` hoặc `ORDER BY`.
- **Soft Deletes**:
  - Không xóa vật lý dữ liệu quan trọng. Sử dụng cột `deleted_at` (nullable timestamp).
- **Money representation**: Lưu trữ dưới dạng số nguyên (integer cents).

## 4. Migration Strategy
- Quản lý phiên bản cơ sở dữ liệu thông qua công cụ Migration (Prisma, TypeORM, Liquibase, v.v.).
- Mỗi thay đổi schema bắt buộc phải đi kèm với file migration. Tuyệt đối không thay đổi trực tiếp trên DB production.
