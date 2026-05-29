---
title: Testing Rules
version: 2.0.0
---

# Testing Rules

## Philosophy

> Tests are not optional. Tests are the proof that code works.
> Without proof, there is no done.

## Test Types

| Type | Location | Coverage |
|------|---------|---------| 
| Unit | `*.spec.ts` / `*.test.ts` next to file | 85%+ |
| Integration | `test/*.e2e-spec.ts` | Key flows |
| E2E | Future | Critical paths |

---

## TDD Workflow (MANDATORY)

```
1. 🔴 RED: Write failing test
   - Test describes the BEHAVIOR, not implementation
   - Run: test command → see RED
   
2. 🟢 GREEN: Write minimal code to pass
   - Absolutely minimal — no extras
   - Run: test command → see GREEN
   
3. 🔵 REFACTOR: Clean up
   - Remove duplication
   - Improve names
   - Run: test command → still GREEN
```

---

## Key Principles

```
✅ Test BEHAVIOR, not implementation
   it('should reject invalid email format', ...)

❌ Test implementation details
   it('should call repository.findOne once', ...)
```

---

## What to Mock

| Dependency | Mock Strategy |
|-----------|-------------|
| Database/ORM | Mock or in-memory DB |
| Cache/Queue | Mock functions |
| External APIs | Mock or stub |
| File Storage | Mock functions |

**NEVER** use real database in unit tests.
**ALWAYS** use real database in integration tests (test database).

---

## Coverage Requirements

```
Overall: ≥ 85%
Critical paths: ≥ 95%
Auth: ≥ 90%
API handlers: ≥ 80%
```

---

## Test File Organization

```
src/
├── feature/
│   ├── feature.service.ts
│   └── feature.service.spec.ts      ← Unit test here
└── test/
    └── feature.e2e-spec.ts          ← Integration test here
```

---

## Before Marking Any Task Done

```bash
# Run full test suite
npm test

# Check coverage
npm test -- --coverage

# Check lint
npm run lint

# Verify build
npm run build
```

All must pass with **0 failures** before reporting completion.
