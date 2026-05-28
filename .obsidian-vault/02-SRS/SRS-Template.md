---
title: SRS — [Module Name]
type: spec
module: [module-slug]
status: Draft
created: 2026-05-28
---

# SRS — [Module Name]

## 1. Problem
<!-- Mô tả vấn đề/nỗi đau của người dùng mà tính năng này giải quyết -->

## 2. Goal
<!-- Mục tiêu cụ thể của tính năng/module này -->

## 3. Scope
**In scope:**
- ...

**Out of scope:**
- ...

## 4. User Flow
<!-- Các bước trải nghiệm của người dùng (step-by-step) -->
1. ...
2. ...

## 5. Technical Design
<!-- Sơ đồ luồng dữ liệu, cách các component giao tiếp với nhau -->

## 6. API Specifications
<!-- Đặc tả các endpoints mới hoặc thay đổi -->

| Method | Endpoint | Auth | Description |
|--------|----------|------|-------------|
| `POST` | `/api/v1/...` | JWT | ... |

### Request Payload Example
```json
{
  "field": "value"
}
```

### Response Payload Example (200 OK)
```json
{
  "success": true,
  "data": {}
}
```

## 7. Database Changes
<!-- Các bảng cơ sở dữ liệu cần tạo mới hoặc cập nhật -->
```sql
CREATE TABLE example (
    id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
    name VARCHAR(255) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT CURRENT_TIMESTAMP
);
```

## 8. Edge Cases
<!-- Các trường hợp biên và cách xử lý tương ứng -->

| Edge Case | Handling |
|-----------|----------|
| Request trùng lặp (Idempotency) | Trả về kết quả cũ kèm Header thích hợp |
| Dữ liệu rỗng/Null | Validate ở Controller và trả về 400 Bad Request |

## 9. Acceptance Criteria
- [ ] AC 1: ...
- [ ] AC 2: ...

## 10. Related Documents
- [[Project-Dashboard]]
