---
title: System Architecture
author: Solo Developer
created: 2026-05-28
---

# System Architecture

## 1. High-Level Diagram
<!-- TODO: Vẽ diagram kiến trúc hệ thống (Mermaid hoặc hình ảnh) -->

```mermaid
graph TD
    Client[Client App] --> Gateway[API Gateway / Load Balancer]
    Gateway --> MainApp[Main Application Service]
    MainApp --> DB[(Database)]
    MainApp --> Cache[(Redis Cache / Queue)]
    MainApp --> BackgroundWorker[Background Workers]
    BackgroundWorker --> DB
```

## 2. Component Directory

| Component | Responsibility | Tech Stack |
|-----------|----------------|------------|
| **Frontend** | Giao diện người dùng | |
| **Backend** | Xử lý logic nghiệp vụ chính | |
| **Database** | Lưu trữ dữ liệu quan hệ | |
| **Cache/Queue** | Caching & Hàng đợi tác vụ | |
| **Storage** | Lưu trữ file tĩnh/media | |

## 3. Data Flow
<!-- Mô tả cách dữ liệu đi qua hệ thống -->
1. Client gửi request qua Gateway.
2. Gateway chuyển tiếp đến Backend.
3. Backend xử lý nghiệp vụ, đọc/ghi DB và Cache.
4. Trả kết quả về cho Client.
5. (Nếu có tác vụ nặng) Đưa vào Queue để Background Worker xử lý bất đồng bộ.
