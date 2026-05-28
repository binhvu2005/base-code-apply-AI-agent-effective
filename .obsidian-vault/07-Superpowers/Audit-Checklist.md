---
title: Audit Checklist
version: 1.0.0
created: 2026-05-28
---

# Tech Lead Audit Checklist

> Tiêu chí đánh giá tuân thủ quy trình phát triển cho Tech Lead trước khi merge code vào main branch.

## 1. Requirement & Planning Compliance
- [ ] **Spec File**: Có spec mô tả nghiệp vụ tại `.obsidian-vault/02-Specs/` hoặc `.obsidian-vault/07-Superpowers/Specs/` trước khi bắt đầu code.
- [ ] **Plan File**: Có kế hoạch triển khai bite-sized chia nhỏ các task (2-5 phút) tại `.obsidian-vault/07-Superpowers/Plans/`.
- [ ] **Checklist Updates**: Tiến độ trong file Plan phải được cập nhật thường xuyên.

## 2. Test-Driven Development (TDD) Compliance
- [ ] **Red Phase Verification**: Có bằng chứng (logs/commits) chứng tỏ test được viết trước và fail trước khi viết code.
- [ ] **Test Coverage**:
  - [ ] Coverage tổng thể đạt ít nhất **85%**.
  - [ ] Các path quan trọng (auth, transactions, security) đạt ít nhất **95%**.
- [ ] **Mocking integrity**: Không gọi DB thực tế trong unit tests. External APIs phải được mock.

## 3. Code & Architecture Compliance
- [ ] **Zero errors**:
  - [ ] `npm run lint` chạy thành công không có lỗi/warnings nào.
  - [ ] `npm run build` không bị vấp lỗi biên dịch.
- [ ] **Types & Interfaces**: Không lạm dụng kiểu dữ liệu `any`.
- [ ] **Single Responsibility**: Mỗi class/service/function chỉ chịu một trách nhiệm duy nhất.
- [ ] **Clean Controllers**: Không có business logic hay câu truy vấn DB trực tiếp trong controller.

## 4. Security & Sensitive Data
- [ ] **No Hardcoded Secrets**: Không có mật khẩu, API keys hay token bị hardcode trong code hoặc commit history.
- [ ] **Input Validation**: Dữ liệu đầu vào tại Controller được validate đầy đủ bằng DTO/schema.
- [ ] **Ownership Verification**: Đảm bảo có bước kiểm tra quyền sở hữu đối với tài nguyên trước khi cho phép Mutation (Update/Delete).

## 5. Git Commit & Messages
- [ ] **Commit Messages**: Tuân thủ định dạng Conventional Commits `type(scope): description`.
- [ ] **Clean History**: Không chứa các commit nháp như "wip", "fix bug", "update".
