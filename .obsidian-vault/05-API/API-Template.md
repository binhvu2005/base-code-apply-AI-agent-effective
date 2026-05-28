---
title: API Spec — [Module Name]
type: api
module: [module-slug]
status: Draft
created: 2026-05-28
---

# API Spec — [Module Name]

## 1. Overview
<!-- Mô tả chung về cụm API này -->

## 2. Base Path
```
/api/v1/[module-path]
```

## 3. Endpoint Specifications

### 3.1 [Endpoint Name]
<!-- Ví dụ: Đăng nhập người dùng -->

- **URL**: `POST /login`
- **Auth required**: No / Yes (JWT)
- **Role required**: User / Admin / None

#### Request Headers
```http
Content-Type: application/json
```

#### Request Body
```json
{
  "username": "string",
  "password": "hashed/string"
}
```

#### Response (200 OK)
```json
{
  "success": true,
  "data": {
    "accessToken": "jwt-token-string",
    "expiresIn": 900
  }
}
```

#### Error Responses
- **400 Bad Request**: Input invalid
- **401 Unauthorized**: Mật khẩu hoặc tài khoản sai

---

### 3.2 [Another Endpoint Name]
- **URL**: `GET /resource`
- **Auth required**: Yes
- **Query Parameters**:
  - `page` (optional, default: 1)
  - `limit` (optional, default: 10)
