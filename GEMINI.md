# Hướng Dẫn Kỷ Luật AI Agent (GEMINI.md)

Chào Antigravity, đây là tài liệu quy định cách làm việc, quy trình và kỷ luật lập trình của bạn trong dự án này. Bạn BẮT BUỘC phải đọc và tuân thủ tuyệt đối các nguyên tắc này trong mọi lượt hội thoại.

---

## ⚔️ 3 ĐIỀU LUẬT THÉP (THE 3 IRON LAWS)

Vi phạm bất kỳ điều luật nào dưới đây đồng nghĩa với việc sản phẩm bị REJECT hoàn toàn.

1.  **NO SPEC ──> NO CODE (Không viết code khi chưa có Đặc tả):**
    *   Tuyệt đối KHÔNG viết bất kỳ dòng logic code production nào nếu chưa có Spec tương ứng được phê duyệt.
    *   Tất cả các Spec phải được lưu vào thư mục: `.obsidian-vault/07-Superpowers/Specs/` dưới định dạng `<YYYY-MM-DD>-<topic>-design.md`.
2.  **NO FAILING TEST ──> NO PRODUCTION CODE (TDD nghiêm ngặt):**
    *   Mọi tính năng mới hoặc sửa lỗi bắt buộc phải bắt đầu bằng việc viết test lỗi trước (RED Phase trong quy trình TDD).
    *   Chạy test suite, xác nhận test lỗi (FAIL), sau đó mới viết lượng code tối thiểu để pass test (GREEN Phase), cuối cùng mới tối ưu hóa (REFACTOR).
3.  **EVIDENCE BEFORE REPORT (Có bằng chứng thực tế mới báo cáo):**
    *   Tuyệt đối cấm sử dụng các từ ngữ mang tính ước đoán hoặc thiếu căn cứ như: *"Có lẽ đã chạy tốt"*, *"Nên chạy được"*, *"Hình như xong rồi"*, *"It should work"*, *"Probably fine"*.
    *   Mọi báo cáo hoàn thành tính năng hoặc sửa lỗi bắt buộc phải đi kèm với log chạy test thực tế thành công 100% từ terminal.

---

## 🔄 BẢN ĐỒ QUY TRÌNH (WORKFLOW MAPPING)

Mỗi loại công việc có một **Workflow** riêng nằm trong thư mục `.agent/workflows/`. Bạn BẮT BUỘC phải đọc và tuân theo đúng file workflow tương ứng trước khi bắt tay vào làm.

| Loại công việc | Workflow cần dùng | Skill hỗ trợ |
|---|---|---|
| 🔄 Toàn bộ vòng đời (End-to-End) | [full-lifecycle.md](./.agent/workflows/full-lifecycle.md) | `brainstorming` → `writing-plans` → `test-driven-development` → `verification-before-completion` |
| ✨ Phát triển tính năng mới | [feature-development.md](./.agent/workflows/feature-development.md) | `brainstorming` → `writing-plans` → `executing-plans` + `test-driven-development` |
| 🐛 Sửa lỗi (Bug Fix) | [bug-fix.md](./.agent/workflows/bug-fix.md) | `systematic-debugging` + `test-driven-development` |
| ♻️ Tái cấu trúc (Refactor) | [refactor.md](./.agent/workflows/refactor.md) | `writing-plans` + `test-driven-development` |
| 🚀 Phát hành (Release) | [release.md](./.agent/workflows/release.md) | `verification-before-completion` + `finishing-a-development-branch` |
| 🧩 Bước 1: Nghiên cứu | [research.md](./.agent/workflows/research.md) | `brainstorming` |
| 🧩 Bước 2: Đặc tả (Specs) | [specs.md](./.agent/workflows/specs.md) | `writing-plans` |
| 🧩 Bước 3: Lên kế hoạch | [planning.md](./.agent/workflows/planning.md) | `writing-plans` |
| 🧩 Bước 4: Code TDD | [tdd.md](./.agent/workflows/tdd.md) | `test-driven-development` |
| 🧩 Bước 5: Git Commit | [commit.md](./.agent/workflows/commit.md) | `finishing-a-development-branch` |
| 🧩 Bước 6: Tích hợp CI/CD | [cicd.md](./.agent/workflows/cicd.md) | `verification-before-completion` |
| 🧩 Bước 7: Audit & Review | [audit.md](./.agent/workflows/audit.md) | `requesting-code-review` |
| 🧩 Bước 8: Update Docs | [docs.md](./.agent/workflows/docs.md) | `writing-skills` |
| 🔒 Bảo mật (Security) | [security.md](./.agent/workflows/security.md) | `brainstorming` + `requesting-code-review` |
| ⚡ Tối ưu hiệu năng (Performance) | [performance.md](./.agent/workflows/performance.md) | `writing-plans` + `test-driven-development` |

### Cách kích hoạt Workflow

Gõ `/` trong chat để xem danh sách workflow có sẵn. Ví dụ các workflow tổng hợp:
- `/full-lifecycle` → mở workflow vòng đời phát triển End-to-End (8 bước)
- `/feature-development` → mở workflow phát triển tính năng
- `/bug-fix` → mở workflow sửa lỗi
- `/refactor` → mở workflow tái cấu trúc
- `/release` → mở workflow phát hành

Hoặc kích hoạt từng bước riêng lẻ hoặc độc lập trong quá trình phát triển:
- `/research` → phân tích yêu cầu, khảo sát
- `/specs` → thiết kế database, API
- `/planning` → chia nhỏ task list
- `/tdd` → áp dụng red-green-refactor
- `/commit` → kiểm tra và commit code chuẩn
- `/cicd` → chạy kiểm thử và build tự động
- `/audit` → rà soát bảo mật và hiệu năng
- `/docs` → cập nhật tài liệu dự án
- `/security` → mở workflow rà soát bảo mật và vá lỗ hổng
- `/performance` → mở workflow phân tích và tối ưu hiệu năng

### Luồng Toàn bộ vòng đời (Full Lifecycle)

```
[1. Nghiên cứu] ──> [2. Specs] ──> [3. Plan] ──> [4. TDD] ──> [5. Commit] ──> [6. CI/CD] ──> [7. Audit] ──> [8. Docs]
```

Chi tiết từng bước: xem [full-lifecycle.md](./.agent/workflows/full-lifecycle.md).

### Luồng mặc định cho tính năng mới (Feature Development)

```
[BƯỚC 1: Brainstorming & Spec] ──> [BƯỚC 2: Plan] ──> [BƯỚC 3: Môi trường cô lập] ──> [BƯỚC 4: TDD] ──> [BƯỚC 5: Tích hợp]
```

Chi tiết từng bước: xem [feature-development.md](./.agent/workflows/feature-development.md).

### Luồng sửa lỗi (Bug Fix)

```
[Reproduce] ──> [Viết test FAIL] ──> [Fix code] ──> [Verify không regression] ──> [Document]
```

Chi tiết từng bước: xem [bug-fix.md](./.agent/workflows/bug-fix.md).

---

## 🛠️ THÔNG TIN CẤU HÌNH DỰ ÁN (PROJECT STACK)

*   **Ngôn ngữ lập trình:** Node.js (TypeScript)
*   **Thư viện kiểm thử (Testing):** Vitest
*   **Thư mục lưu trữ tài liệu đặc tả (Specs):** `.obsidian-vault/07-Superpowers/Specs/`
*   **Thư mục lưu trữ kế hoạch (Plans):** `.obsidian-vault/07-Superpowers/Plans/`
*   **Quy chuẩn viết Code:** Xem chi tiết tại [coding-rules.md](./.agent/rules/coding-rules.md)
*   **Quy chuẩn viết Test:** Xem chi tiết tại [testing-rules.md](./.agent/rules/testing-rules.md)
*   **Quy tắc bảo mật:** Xem chi tiết tại [security-rules.md](./.agent/rules/security-rules.md)
*   **Review Checklist:** Xem chi tiết tại [review-checklist.md](./.agent/rules/review-checklist.md)
*   **API Key mặc định:** Cấu hình qua file `.env` (Không bao giờ commit file này lên Git).
