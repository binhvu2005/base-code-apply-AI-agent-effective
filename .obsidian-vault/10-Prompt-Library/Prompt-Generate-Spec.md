---
title: Prompt Library — Generate Spec
author: Solo Developer
created: 2026-05-28
tags: [prompt, ai, spec]
---

# Prompt Library — Generate Spec

## Prompt: Tạo SRS Document

```
Tôi cần bạn tạo tài liệu Đặc tả kỹ thuật (Specs - Design Doc) cho module: {MODULE_NAME}

Context:
- Tech stack: {TECH_STACK}
- Yêu cầu chính: {KEY_REQUIREMENTS}

Hãy tạo tài liệu Đặc tả kỹ thuật theo cấu trúc sau:

---
title: Specs — {Module Name}
type: spec
module: {module-slug}
status: Draft
created: {date}
---

# Specs — {Module Name}

## 1. Context & Objectives (Bối cảnh & Yêu cầu)
- Vấn đề cốt lõi cần giải quyết là gì?
- Mục tiêu cụ thể.

## 2. Design Alternatives & Selection (Lên ý tưởng & So sánh phương án)
- **Ý tưởng/Phương án 1:** [Mô tả cách tiếp cận] - Ưu/nhược điểm.
- **Ý tưởng/Phương án 2:** [Mô tả cách tiếp cận] - Ưu/nhược điểm.
- **Lựa chọn tối ưu:** Chọn phương án nào và lập luận kỹ thuật tại sao chọn.

## 3. Database Design (Thiết kế Cơ sở dữ liệu)
- Sơ đồ quan hệ thực thể bằng Mermaid (ERD).
- Định nghĩa chi tiết các bảng (tên bảng, các cột kèm kiểu dữ liệu, PK, FK, Constraints, Indexes).

## 4. API Design (Thiết kế API Endpoints)
- Danh sách API Map.
- Chi tiết từng endpoint (URL, HTTP Method, Request Headers/Body, Response mẫu Success + Error kèm HTTP status codes).

## 5. Flow & State Machine (Luồng xử lý)
- Vẽ Sequence Diagram bằng Mermaid cho các luồng xử lý hoặc State Machine cho các thực thể.

## 6. Spec to Plan Checklist (Đẩy thiết kế sang Plan)
- Liệt kê danh sách các đầu việc kỹ thuật cụ thể (checklists) từ thiết kế trên để làm đầu vào lập Plan.

## 7. Edge Cases
- Liệt kê các edge cases (Race conditions, validation errors, timeouts) và hướng xử lý.

## 8. Acceptance Criteria
- Các tiêu chí nghiệm thu xanh.
```

## Prompt: Brainstorm Edge Cases

```
Tôi đang implement {FEATURE}.

Current design:
{PASTE SPEC HERE}

Hãy liệt kê các edge cases tôi có thể bỏ sót:
1. Concurrent access issues
2. Race conditions
3. Error states
4. Boundary values
5. Security vulnerabilities
6. Business rule violations
```
