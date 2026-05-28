---
title: Coding Convention
version: 1.0.0
created: 2026-05-28
---

# Coding Convention

> **THE 3 IRON LAWS** — Vi phạm bất kỳ luật nào = Code bị reject

## ⚔️ Iron Law 1: NO SPEC → NO CODE

Tuyệt đối **KHÔNG** viết bất kỳ logic code nào nếu chưa có Spec được phê duyệt.

- Spec file ở: `.obsidian-vault/07-Superpowers/Specs/` hoặc `.obsidian-vault/02-Specs/`
- Spec phải có: Problem, Goal, API, Database, Edge Cases, Acceptance Criteria
- **Action**: Nếu không có Spec → Tạo Spec trước, hỏi xác nhận, sau đó mới code

## ⚔️ Iron Law 2: NO FAILING TEST → NO PRODUCTION CODE

Mọi code mới PHẢI bắt đầu bằng failing test (Red Phase).

```
RED: Write failing test first
  ↓
GREEN: Write minimal code to pass
  ↓
REFACTOR: Clean up
```

**Violation examples** (không được làm):
- "Let me write the service first, then add tests"
- "Tests can be added later"
- "This is simple enough to skip tests"

## ⚔️ Iron Law 3: EVIDENCE BEFORE REPORT

**CẤM TUYỆT ĐỐI** dùng các cụm từ:
- "Should work"
- "Hình như xong rồi"
- "Probably fine"
- "I think it's working"

Mọi báo cáo hoàn thành PHẢI kèm test output thực tế:

```
✅ CORRECT: "All 24 tests pass: npm test -- --coverage shows 87% coverage"
❌ WRONG: "The service should be working now"
```

---

## 📐 Code Style Rules

### Naming Conventions

| Type | Convention | Example |
|------|-----------|---------|
| Variables | camelCase | `userId`, `itemTitle` |
| Constants | UPPER_SNAKE | `MAX_FILE_SIZE_MB` |
| Classes | PascalCase | `UserService` |
| Interfaces | PascalCase (no I prefix) | `CreateUserDto` |
| Files | kebab-case | `users.service.ts` |
| DB columns | snake_case | `owner_id`, `created_at` |

### Error Handling

```typescript
// ✅ CORRECT: Specific, meaningful exceptions
throw new NotFoundException(`User ${userId} not found`);

// ❌ WRONG: Generic errors like "Something went wrong"
throw new Error("Something went wrong");
```

### Async/Await

```typescript
// ✅ CORRECT: Use async/await
const user = await this.userService.findOne(id);

// ❌ WRONG: Callback hell or .then() chains
this.userService.findOne(id).then(user => { ... });
```

---

## 🏗 Architecture Rules

### 1. Single Responsibility
Mỗi service/class chỉ làm một việc duy nhất.

### 2. Validate at Boundary
Validate input tại controller/handler (dùng class-validator/DTO/schema), không validate ở tầng service.

### 3. No Business Logic in Controllers
Controllers chỉ nhận input, gọi service và trả về response.

---

## 🗄 Database Rules

### 1. Always Use ORM
Tránh sử dụng SQL thuần trừ phi có yêu cầu tối ưu hiệu năng đặc biệt hoặc truy vấn phức tạp.

### 2. Always Use Transactions for Multi-Step Operations
Đảm bảo tính toàn vẹn của dữ liệu khi thao tác ghi trên nhiều bảng.

### 3. Money = Integer Cents
Không lưu giá trị tiền tệ bằng số thực (`float`/`double`). Luôn quy đổi ra đơn vị nhỏ nhất (ví dụ: cents, VND).

---

## 🔒 Security Rules

### 1. Never Trust Client Input
Luôn kiểm tra quyền sở hữu (ownership check) trước khi cập nhật hoặc xóa dữ liệu.

### 2. Rate Limiting
Áp dụng rate limiting trên các API nhạy cảm (auth, upload, public endpoints).

### 3. No Secrets in Code
Không bao giờ commit file chứa secrets (như `.env`). Dùng `.env.example` làm template.

---

## 📝 Git Commit Rules

```
Format: <type>(<scope>): <description>

Types:
  feat     New feature
  fix      Bug fix
  test     Add tests
  refactor Refactor code
  docs     Documentation
  chore    Config, build
```
