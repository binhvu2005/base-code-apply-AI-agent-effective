---
title: Entity Relationship Diagram
author: Solo Developer
created: 2026-05-28
---

# Entity Relationship Diagram (ERD)

## 1. Visual ERD
<!-- Sử dụng Mermaid syntax để biểu diễn các thực thể và quan hệ -->

```mermaid
erDiagram
    users {
        uuid id PK
        string email UK
        string password_hash
        string display_name
        string role
        timestamp created_at
    }
    
    sessions {
        uuid id PK
        uuid user_id FK
        string token UK
        timestamp expires_at
    }

    users ||--o{ sessions : "has"
```

## 2. Table List

| Table Name | Description | Key Relationships |
|------------|-------------|-------------------|
| `users` | Lưu thông tin người dùng | 1:N với `sessions` |
| `sessions` | Quản lý phiên đăng nhập | N:1 với `users` |

## 3. Related Documents
- [[Database-Design]]
