---
title: Test Strategy
created: 2026-05-28
status: Active
tags:
  - testing
  - tdd
  - strategy
---

# Test Strategy

## Philosophy

> Tests are not optional. Tests are the proof that code works.
> Without proof, there is no done.

---

## Test Pyramid

```
          ╱ E2E Tests ╲          (Playwright / Cypress)
         ╱─────────────╲
        ╱ Integration    ╲       (Supertest / API Testing)
       ╱───────────────────╲
      ╱ Unit Tests           ╲   (Jest / Vitest / Mocha)
     ╱─────────────────────────╲
```

---

## Test Types

| Type | Location | Tools | Coverage Target |
|------|---------|-------|-----------------|
| Unit | `*.spec.ts` / `*.test.ts` cùng folder | Jest / Vitest | ≥ 85% |
| Integration | `test/*.e2e-spec.ts` | Supertest | Key flows |
| E2E | Thư mục `e2e/` | Playwright / Cypress | Critical paths |

---

## TDD Workflow (MANDATORY)

```
1. 🔴 RED:     Viết test mô tả hành vi → chạy → FAIL
2. 🟢 GREEN:   Viết code tối thiểu để pass → chạy → PASS
3. 🔵 REFACTOR: Clean up, improve names → chạy → still PASS
```

> **Rule:** NO failing test → NO production code

---

## Coverage Requirements

| Module | Target |
|--------|--------|
| Overall | ≥ 85% |
| Critical paths (auth, transactions) | ≥ 95% |
| API handlers | ≥ 80% |

---

## What to Mock

| Dependency | Strategy |
|-----------|----------|
| Database/ORM | Mock DB Client / Deep Mock / In-memory DB |
| Cache/Queue | Mock functions / Redis Mock |
| External APIs | Mock HTTP client (nock / msw) |

**NEVER** dùng real database trong unit tests.
**ALWAYS** dùng real database trong integration tests.

---

## Commands

```bash
# Run all tests
npm test

# Run with coverage
npm test -- --coverage

# Watch mode
npm run test:watch
```

---

## Related
- [[Coding-Convention]]
- [[Testing-Rules]]
