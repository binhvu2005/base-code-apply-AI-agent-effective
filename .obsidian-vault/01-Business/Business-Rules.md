---
title: Business Rules
author: Solo Developer
created: 2026-05-28
---

# Business Rules

> Các quy tắc nghiệp vụ cốt lõi bắt buộc hệ thống phải tuân thủ. Mọi thay đổi code làm vi phạm các quy tắc này sẽ không được chấp nhận.

## 1. User & Identity Rules
<!-- TODO: Quy tắc liên quan đến tài khoản người dùng, phân quyền -->
- Người dùng chỉ được kích hoạt tài khoản khi đã xác thực email (nếu có yêu cầu).
- Mật khẩu lưu trữ bắt buộc phải được mã hóa (dùng bcrypt hoặc tương đương).

## 2. Transaction & Money Rules
- **Money representation**: Mọi giá trị tiền tệ trong hệ thống phải được lưu trữ dưới dạng số nguyên (integer cents/VND) để tránh lỗi làm tròn dấu phẩy động.
- Mọi giao dịch tài chính phải có trạng thái rõ ràng (`PENDING`, `SUCCESS`, `FAILED`) và có lịch sử log không thể chỉnh sửa.

## 3. Data Integrity & Invariants
- Các bản ghi mang tính lịch sử giao dịch tuyệt đối **không được cập nhật hoặc xóa** sau khi đã được lưu thành công.
- Các liên kết khóa ngoại bắt buộc phải được duy trì tính toàn vẹn (Cascade hoặc Restrict tùy trường hợp nghiệp vụ).

## 4. Operation Rules
<!-- TODO: Thêm quy tắc về quy trình xử lý, nghiệp vụ đặc thù -->
- Các tác vụ tốn thời gian (như gửi email, xử lý file nặng, gọi AI service) bắt buộc phải xử lý bất đồng bộ thông qua hàng đợi (queue).
