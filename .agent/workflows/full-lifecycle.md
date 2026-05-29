---
title: Workflow — Full Lifecycle Development
version: v2.0.0
document_code: SOP-DEV-002
---

# 🔄 WORKFLOW: VÒNG ĐỜI PHÁT TRIỂN ĐẦU CUỐI (Full Lifecycle)

## Khi nào dùng?
Khi bạn muốn phát triển một tính năng hoặc dự án hoàn chỉnh từ A đến Z, đi qua đầy đủ 8 bước chuẩn hóa.

---

## ⚔️ MỤC TIÊU

> **Đảm bảo mọi sản phẩm ra lò đều đã qua 8 cửa kiểm duyệt. Không có bước nào bị bỏ qua.**

---

## 🗺️ SƠ ĐỒ TOÀN BỘ VÒNG ĐỜI

```
[1. Research] ➔ [2. Specs] ➔ [3. Plan] ➔ [4. TDD] ➔ [5. Commit] ➔ [6. CI/CD] ➔ [7. Audit] ➔ [8. Docs]
```

---

## 🛠️ CHI TIẾT TỪNG BƯỚC

### 🔍 Bước 1: Nghiên cứu (`/research`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI nghiên cứu, Bạn cung cấp ý tưởng |
| **Input** | Ý tưởng / yêu cầu từ User |
| **Output** | Báo cáo nghiên cứu đầy đủ (pháp lý, thị trường, kỹ thuật) |
| **Tiêu chí chuyển bước** | User đồng ý với hướng giải pháp |
| **Workflow chi tiết** | Xem [research.md](./.agent/workflows/research.md) |

Bao gồm: Tìm kiếm đa nguồn (Google, GitHub, X, Facebook...), phân tích pháp lý VN + quốc tế, so sánh thị trường, đánh giá kỹ thuật, phân tích rủi ro.

---

### 📐 Bước 2: Đặc tả Specs (`/specs`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI thiết kế, Bạn review |
| **Input** | Kết quả nghiên cứu + yêu cầu cụ thể |
| **Output** | File Spec: DB Schema + API Endpoints + Error Handling + Security |
| **Tiêu chí chuyển bước** | User/Tech Lead duyệt Spec |
| **Workflow chi tiết** | Xem [specs.md](./.agent/workflows/specs.md) |

Bao gồm: ERD diagram, chi tiết từng bảng, API Map, chi tiết từng endpoint (request/response), sequence diagram, error codes, security checklist.

---

### 📝 Bước 3: Kế hoạch (`/planning`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI chia task, Bạn review |
| **Input** | Spec đã duyệt |
| **Output** | File Plan: checklist micro-tasks (2-5 phút mỗi task) |
| **Tiêu chí chuyển bước** | User đồng ý thứ tự và ước tính |
| **Workflow chi tiết** | Xem [planning.md](./.agent/workflows/planning.md) |

Bao gồm: Phân rã Epic → Task → Subtask, dependency graph, ước tính thời gian, bảng ưu tiên.

---

### 🔴🟢🔵 Bước 4: TDD (`/tdd`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI code theo TDD cycle |
| **Input** | Plan đã duyệt |
| **Output** | Code production + Test suite PASS |
| **Tiêu chí chuyển bước** | Tất cả subtask trong Plan đã `[x]` |
| **Workflow chi tiết** | Xem [tdd.md](./.agent/workflows/tdd.md) |

Bao gồm: RED (test FAIL) → GREEN (code tối thiểu) → REFACTOR → commit, lặp lại cho mỗi subtask.

---

### 💾 Bước 5: Git Commit (`/commit`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI commit, Bạn verify |
| **Input** | Code đã pass test |
| **Output** | Git history sạch, Conventional Commits |
| **Tiêu chí chuyển bước** | Tất cả thay đổi đã committed |
| **Workflow chi tiết** | Xem [commit.md](./.agent/workflows/commit.md) |

Bao gồm: Review diff, kiểm tra secrets, atomic commits, Conventional Commits format.

---

### 🚀 Bước 6: CI/CD (`/cicd`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI chạy pipeline |
| **Input** | Code đã committed |
| **Output** | Lint PASS + Test PASS + Build PASS |
| **Tiêu chí chuyển bước** | Toàn bộ pipeline xanh |
| **Workflow chi tiết** | Xem [cicd.md](./.agent/workflows/cicd.md) |

Bao gồm: Auto lint + fix, full test suite, build check, security audit (npm audit).

---

### 🛡️ Bước 7: Audit (`/audit`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI audit, Bạn quyết định |
| **Input** | Code đã qua CI/CD |
| **Output** | Báo cáo audit (security, performance, code quality) |
| **Tiêu chí chuyển bước** | 0 MUST FIX items |
| **Workflow chi tiết** | Xem [audit.md](./.agent/workflows/audit.md) |

Bao gồm: Cross-check vs Spec, security audit, performance audit, code quality audit, phân loại severity.

---

### 📚 Bước 8: Update Docs (`/docs`)

| Mục | Chi tiết |
|---|---|
| **Ai làm** | AI cập nhật docs |
| **Input** | Code đã merge |
| **Output** | README + API docs + CHANGELOG + Knowledge Items |
| **Tiêu chí hoàn tất** | Tất cả docs phản ánh đúng code hiện tại |
| **Workflow chi tiết** | Xem [docs.md](./.agent/workflows/docs.md) |

Bao gồm: README, .env.example, API docs, CHANGELOG, inline comments, Knowledge Items, báo cáo hoàn tất toàn diện.

---

## 🔁 QUY TẮC CHUYỂN BƯỚC

```
Bước N chỉ được bắt đầu khi Bước N-1 đã hoàn tất và được User xác nhận.
Nếu phát hiện vấn đề ở bước sau, có thể quay lại bước trước để sửa.
```

| Từ | Quay lại | Khi nào |
|---|---|---|
| Bước 3 (Plan) | Bước 2 (Specs) | Phát hiện Spec thiếu sót |
| Bước 4 (TDD) | Bước 3 (Plan) | Task chia chưa hợp lý |
| Bước 6 (CI/CD) | Bước 4 (TDD) | Test FAIL → cần fix code |
| Bước 7 (Audit) | Bước 4 (TDD) | Security issue → cần fix code |

---

## RULES CỦA WORKFLOW NÀY

1. **Không skip bước** — 8 bước là 8 bước. Không "tiện tay" gộp hay bỏ qua.
2. **Mỗi bước có output** — Không chuyển bước nếu output chưa rõ ràng.
3. **User xác nhận tại các checkpoint** — Bước 1, 2, 3, 7 cần User review trước khi tiếp.
4. **Có thể quay lại** — Full lifecycle không phải one-way. Phát hiện sai thì quay lại sửa.
5. **Evidence before report** — Mọi báo cáo hoàn tất phải có log thực tế.
