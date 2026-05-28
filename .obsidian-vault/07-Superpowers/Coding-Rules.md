---
title: Coding Rules
version: 1.0.0
created: 2026-05-28
---

# Coding Rules

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

```
✅ CORRECT: Specific, meaningful exceptions
❌ WRONG: Generic errors like "Something went wrong"
```

### Async/Await

```
✅ CORRECT: Use async/await
❌ WRONG: Callback hell or .then() chains
```

---

## 🏗 Architecture Rules

### 1. Single Responsibility
Mỗi service chỉ làm một việc.

### 2. Validate at Boundary
Validate input tại controller/handler, không tại service.

### 3. No Business Logic in Controllers
Controllers chỉ nhận input, gọi service, trả response.

---

## 🗄 Database Rules

### 1. Always Use ORM (avoid raw SQL when possible)

### 2. Always Use Transactions for Multi-Step Operations

### 3. Money = Integer Cents (never float)

```
✅ Store as cents: 2999 = $29.99
❌ Never store as float: 29.99
```

---

## 🔒 Security Rules

### 1. Never Trust Client Input
Always validate with DTO + guards.

### 2. Rate Limiting on All Endpoints

### 3. No Secrets in Code
Use environment variables, never commit `.env` files.

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

Examples:
  ✅ feat(auth): add JWT refresh token rotation
  ✅ test(users): add unit tests for create user
  ✅ fix(payment): handle edge case for zero amount
```

---

## 🧪 Testing Rules (Summary)

See [[Testing-Rules]] for full testing guidelines.

Summary:
- Unit tests next to source file
- Integration tests in `test/` directory
- Test coverage minimum: **85%**
- Always mock external services in unit tests
