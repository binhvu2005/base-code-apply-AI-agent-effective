---
title: Security Rules
version: 1.0.0
created: 2026-05-28
---

# Security Rules

## 1. Authentication & Authorization

### JWT Rules
- Access token expire: **15 minutes**
- Refresh token expire: **30 days**
- Payload: `{ sub, email, role, iat, exp }` — NO sensitive data

### Route Protection
- Every protected route MUST have auth guards
- Admin routes need both auth + role guards

### Ownership Checks (CRITICAL)
Before any mutation, verify the user owns the resource.

---

## 2. Input Validation

### All Input MUST Be Validated
- Use DTO/schema validation at the boundary
- Validate MIME types for file uploads (not just extension)

---

## 3. Rate Limiting

<!-- TODO: Điền rate limiting cho từng endpoint -->

| Endpoint | Limit | Window |
|----------|-------|--------|
| `/auth/login` | 10 requests | 1 minute |
| `/auth/register` | 5 requests | 1 minute |
| Public API | 100 requests | 1 minute |

---

## 4. SQL Injection Prevention

Use ORM with parameterized queries by default.

For raw queries (only when necessary):
```
✅ SAFE: Use parameterized/tagged templates
❌ DANGEROUS: String concatenation
```

---

## 5. Environment Variables

**NEVER** commit secrets to Git:

```bash
# .env.example (commit this)
DATABASE_URL=postgresql://user:password@localhost:5432/mydb
JWT_SECRET=your-secret-key

# .env (NEVER commit — in .gitignore)
```

`.gitignore` must contain:
```
.env
.env.local
.env.production
```

---

## 6. CORS Policy

Configure CORS to allow only trusted origins.

---

## 7. Security Checklist (Before Each PR)

- [ ] No secrets in code or commits
- [ ] All routes have appropriate guards
- [ ] Ownership verified before mutations
- [ ] Input validated with DTOs
- [ ] Rate limiting applied
- [ ] Error messages don't leak internal details
