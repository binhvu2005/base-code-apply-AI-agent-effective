---
title: Workflow — Feature Development
version: v1.0.0
document_code: SOP-DEV-001
---

# Feature Development Workflow

> Quy trình 5 bước phát triển tính năng từ ý tưởng đến tích hợp chính thức.

---

## 🔄 QUY TRÌNH 5 BƯỚC PHÁT TRIỂN BẮT BUỘC

```
[BƯỚC 1: Spec] ──> [BƯỚC 2: Plan] ──> [BƯỚC 3: Môi trường] ──> [BƯỚC 4: TDD] ──> [BƯỚC 5: Tích hợp]
```

### 📝 BƯỚC 1: Động não & Đặc tả thiết kế (Brainstorming & Spec)

Trước khi làm bất kỳ task nào (tính năng mới hoặc sửa lỗi phức tạp):

1. **Thảo luận & Làm rõ**: Dev phối hợp với Tech Lead/PM (hoặc AI Agent) để đặt câu hỏi làm rõ: Mục tiêu là gì? Giới hạn kỹ thuật? Các trường hợp biên (edge cases)?
2. **Viết Đặc tả (Spec)**: Tạo file đặc tả thiết kế tại đường dẫn: `.obsidian-vault/07-Superpowers/Specs/YYYY-MM-DD-<ten-tinh-nang>-design.md`
3. **Nội dung Spec phải có**:
   - **Mục tiêu**: 1 câu ngắn gọn mô tả tính năng.
   - **Kiến trúc dữ liệu**: Data flow, Database schema (nếu có).
   - **Giao diện/API**: Các endpoint, request/response payload, hoặc mô tả UI component.
   - **Error Handling**: Cách hệ thống xử lý khi có lỗi xảy ra.
4. **Phê duyệt**: Gửi file Spec cho Tech Lead/PM duyệt qua Git Pull Request hoặc review trực tiếp. Chỉ khi Spec được phê duyệt mới đi tiếp.

---

### 🗺️ BƯỚC 2: Lập kế hoạch thực thi chi tiết (Implementation Plan)

Sau khi Spec được duyệt, Dev phải lập một kế hoạch thực hiện "mịn" đến từng phút.

1. **Tạo file Kế hoạch (Plan)**: Tạo file tại: `.obsidian-vault/07-Superpowers/Plans/YYYY-MM-DD-<ten-tinh-nang>-plan.md`
2. **Phân rã thành các Task siêu nhỏ (2-5 phút thực hiện)**:
   - *Ví dụ không hợp lệ*: Task 1: Code màn hình đăng nhập (Quá chung chung).
   - *Ví dụ hợp lệ*:
     - Task 1.1: Viết test lỗi xác thực email trống.
     - Task 1.2: Viết code tối thiểu để pass test email trống.
     - Task 1.3: Commit.
3. **Mỗi Task bắt buộc phải chứa**:
   - Đường dẫn chính xác của file cần tạo/sửa.
   - Đoạn code test mẫu mong muốn.
   - Lệnh chạy kiểm thử cụ thể và kết quả mong đợi.
4. **Duyệt kế hoạch**: Tech Lead/PM sẽ review nhanh qua file Plan này để đảm bảo Dev không bị đi lạc hướng.

---

### 🛡️ BƯỚC 3: Tạo môi trường cô lập (Isolated Workspace)

Tránh việc code trực tiếp trên nhánh chính gây xung đột và hỏng hóc hệ thống hiện tại.

1. **Sử dụng Git Worktree (hoặc tạo Branch mới)**:
   ```bash
   # Tạo môi trường làm việc cô lập trên branch mới
   git worktree add .worktrees/feature-<ten-tinh-nang> -b feature/<ten-tinh-nang>
   cd .worktrees/feature-<ten-tinh-nang>
   ```
2. **Cài đặt Dependency & Thiết lập**: Chạy lệnh setup dự án để đảm bảo môi trường sạch.
3. **Chạy Baseline Test**: Chạy test suite gốc của dự án để đảm bảo môi trường bắt đầu hoàn toàn sạch và không có lỗi từ trước.
   ```bash
   npm test # Hoặc lệnh tương đương của dự án
   ```
   *(Nếu baseline test lỗi, phải báo cáo ngay lập tức cho Tech Lead, không được tự ý sửa đè).*

---

### 🔴🟢🔵 BƯỚC 4: Thực thi kỷ luật bằng TDD & Xác thực lỗi (TDD & Verification)

Đây là giai đoạn viết code chính. Dev phải thực hiện theo vòng lặp Red-Green-Refactor:

1. **🔴 RED Phase (Viết test lỗi)**:
   - Viết một test case nhỏ kiểm tra hành vi mong muốn.
   - Chạy test và bắt buộc phải thấy test đó bị báo đỏ (FAIL) do chưa có code xử lý logic.
   - *Tại sao?* Nếu viết test xong chạy vẫn xanh (PASS), nghĩa là bạn đang test sai thứ hoặc test vô dụng.
2. **🟢 GREEN Phase (Viết code tối thiểu)**:
   - Viết đoạn code logic đơn giản nhất, tối thiểu nhất để test case trên chuyển sang màu xanh (PASS).
   - Cấm viết thêm các tính năng dư thừa không liên quan đến test case hiện tại (Tuân thủ triệt để YAGNI - You Aren't Gonna Need It).
3. **🔵 REFACTOR Phase (Tối ưu hóa)**:
   - Dọn dẹp code, tối ưu cấu trúc, xóa bỏ trùng lặp dữ liệu.
   - Chạy lại test suite để đảm bảo code tối ưu vẫn chạy đúng (vẫn xanh).
4. **Tích xanh tiến độ**: Sau mỗi task hoàn thành, cập nhật trạng thái `- [x]` trong file `.obsidian-vault/07-Superpowers/Plans/YYYY-MM-DD-<ten-tinh-nang>-plan.md` và thực hiện git commit.

---

### 🏁 BƯỚC 5: Hoàn thành & Tích hợp (Finishing & Integration)

Khi toàn bộ các task trong file Kế hoạch đã hoàn thành:

1. **Kiểm tra linter & Xây dựng thử (Build check)**:
   ```bash
   npm run lint  # Đảm bảo code sạch lỗi format
   npm run build # Đảm bảo biên dịch thành công
   ```
2. **Chạy toàn bộ Test Suite toàn diện**: Đảm bảo tất cả các bài test (cũ và mới) đều xanh 100%.
3. **Đẩy code & Tạo Pull Request (PR)**:
   ```bash
   git push -u origin feature/<ten-tinh-nang>
   ```
   - Tạo PR trên GitHub/GitLab kèm theo mô tả ngắn gọn và liên kết tới file Spec và Plan đã duyệt.
4. **Dọn dẹp môi trường (Cleanup)**: Sau khi PR được merge thành công vào nhánh chính, tiến hành xóa bỏ worktree tạm thời để giải phóng dung lượng máy tính:
   ```bash
   git worktree remove .worktrees/feature-<ten-tinh-nang>
   git worktree prune
   ```
