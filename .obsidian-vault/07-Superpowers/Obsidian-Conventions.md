---
title: Obsidian Conventions
version: 1.0.0
created: 2026-05-28
---

# Obsidian Knowledge Base Conventions

> Hướng dẫn tổ chức và quản lý tài liệu tri thức dự án trong `.obsidian-vault/`.

## 1. Directory Structure

Hệ thống tài liệu được chia thành các thư mục tiền tố số (00-13) để sắp xếp thứ tự:
- `00-Dashboard/`: Dashboard tổng quan dự án và Coding Conventions.
- `01-Business/`: Yêu cầu nghiệp vụ, actors, user flows.
- `02-Specs/`: Tài liệu SRS đặc tả kỹ thuật của từng tính năng.
- `03-Architecture/`: Thiết kế hệ thống, backend, DB và hướng dẫn cài đặt môi trường.
- `04-ADR/`: Lưu trữ các quyết định thiết kế kiến trúc quan trọng.
- `05-API/`: Tài liệu API chi tiết.
- `06-Database/`: ERD và đặc tả chi tiết các bảng cơ sở dữ liệu.
- `07-Superpowers/`: Quy tắc SOP của AI Agent, Workflows, và nhật ký code (Brainstorm, Spec, Plan, TDD, Review).
- `08-Testing/`: Kịch bản và chiến lược kiểm thử.
- `09-Meeting/`: Nhật ký các cuộc họp.
- `10-Prompt-Library/`: Thư viện prompt tương tác với AI.
- `11-UI-UX-Style-Guideline/`: Quy tắc thiết kế UI/UX (CSS, Colors, Icons).
- `12-Roadmap/`: Lộ trình phát triển MVP.
- `13-Backlogs/`: Danh sách các đầu việc đang chờ triển khai.

## 2. Naming Conventions
- Tên File: Sử dụng `Pascal-Case-With-Hyphens` hoặc tên mô tả rõ ràng, ví dụ: `System-Architecture.md`, `SRS-Authentication.md`.
- Tránh đặt tên quá chung chung như `Untitled.md` hay `notes.md`.

## 3. Markdown Formatting & Metadata
Mỗi tài liệu mới bắt buộc phải chứa phần Metadata YAML ở đầu trang:
```yaml
---
title: [Tên Tài Liệu]
author: [Tên tác giả hoặc Solo Developer]
created: YYYY-MM-DD
tags:
  - tag-chinh
  - tag-phu
---
```

## 4. Wiki Links
- Tận dụng tối đa liên kết wiki song phương của Obsidian để liên kết các tài liệu với nhau, ví dụ: `[[Coding-Convention]]`, `[[System-Architecture]]`.
- Khi nhắc đến một module, hãy liên kết trực tiếp tới file SRS của module đó.

## 5. Git Integration
- Tránh commit các file cache và workspace cá nhân của Obsidian.
- Đảm bảo `.gitignore` đã có cấu hình loại bỏ `.obsidian/workspace.json`, `.obsidian/workspace-mobile.json`, `.obsidian/cache/`.
