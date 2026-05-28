---
title: Backend Architecture
author: Solo Developer
created: 2026-05-28
---

# Backend Architecture

## 1. Architectural Patterns
<!-- TODO: Mô tả pattern được sử dụng, ví dụ: Layered Architecture, Clean Architecture, CQRS, MVC -->
Hệ thống sử dụng kiến trúc phân lớp (Layered Architecture):
```
Client Request
      ↓
Controller/Handler  (Validate input, route requests)
      ↓
Service Layer       (Execute core business logic)
      ↓
Repository/DAO      (Database queries & mutations)
      ↓
Database / Storage
```

## 2. Directory Structure
<!-- Giải thích ý nghĩa của các thư mục trong backend -->
```
src/
├── config/         # Cấu hình môi trường, database, v.v.
├── modules/        # Các module nghiệp vụ (auth, users, ...)
│   ├── [module]/
│   │   ├── [module].controller.ts
│   │   ├── [module].service.ts
│   │   ├── [module].repository.ts
│   │   └── dto/
├── common/         # Middlewares, guards, decorators dùng chung
└── main.ts         # Điểm khởi chạy ứng dụng
```

## 3. Dependency Injection
<!-- Nếu sử dụng NestJS, Spring Boot, v.v., mô tả cách quản lý dependency -->
Các service được inject tự động thông qua Container. Tránh khởi tạo trực tiếp (`new Service()`).

## 4. Key Libraries & Frameworks
- **Framework**: ...
- **ORM/Query Builder**: ...
- **Validation**: ...
- **Testing**: ...
