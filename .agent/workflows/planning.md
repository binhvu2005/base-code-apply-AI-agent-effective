---
title: Workflow — Planning (Chia nhỏ Task)
version: v2.0.0
document_code: SOP-DEV-005
---

# 📋 WORKFLOW: LẬP KẾ HOẠCH THỰC THI (Planning)

## Khi nào dùng?
Sau khi Spec đã được duyệt, bạn cần chia nhỏ thiết kế thành các task cực nhỏ (micro-tasks) để kiểm soát tiến độ và đảm bảo không bỏ sót.

---

## ⚔️ MỤC TIÊU

> **Biến Spec thành danh sách việc cần làm rõ ràng đến từng phút, từng file, từng dòng test.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Task quá chung chung (ví dụ: "Làm màn hình Login")
2. ❌ Task không ghi rõ file nào cần tạo/sửa
3. ❌ Task không có tiêu chí hoàn thành (Done Criteria)
4. ❌ Không có Spec được duyệt trước đó → Quay lại `/specs`

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: ĐỌC LẠI SPEC (AI làm)

- Mở file Spec đã duyệt: `.obsidian-vault/07-Superpowers/Specs/YYYY-MM-DD-<topic>-design.md`
- Liệt kê tất cả các thành phần cần triển khai:
  - Database tables
  - API endpoints
  - UI components
  - Business logic
  - Tích hợp bên ngoài

---

### BƯỚC 2: PHÂN RÃ THÀNH EPIC → TASK → SUBTASK (AI làm, bạn review)

#### 2.1 Cấu trúc phân rã

```
🏗️ EPIC: [Tên tính năng lớn]
│
├── 📦 Task 1: Database Layer
│   ├── Subtask 1.1: Tạo migration cho bảng users
│   │   📝 File: src/db/migrations/001_create_users.ts
│   │   🧪 Test: Chạy migration thành công, rollback thành công
│   │   ⏱️ Ước tính: 3 phút
│   │
│   ├── Subtask 1.2: Tạo migration cho bảng orders
│   │   📝 File: src/db/migrations/002_create_orders.ts
│   │   🧪 Test: Chạy migration thành công, FK constraint hoạt động
│   │   ⏱️ Ước tính: 3 phút
│   │
│   └── Subtask 1.3: Tạo seed data cho development
│       📝 File: src/db/seeds/dev_seed.ts
│       🧪 Test: Seed chạy xong, query đúng data
│       ⏱️ Ước tính: 5 phút
│
├── 📦 Task 2: Backend API
│   ├── Subtask 2.1: Viết test cho POST /api/v1/users (RED)
│   ├── Subtask 2.2: Implement POST /api/v1/users (GREEN)
│   ├── Subtask 2.3: Refactor + validation logic (REFACTOR)
│   ├── Subtask 2.4: Viết test cho GET /api/v1/users/:id (RED)
│   ├── Subtask 2.5: Implement GET /api/v1/users/:id (GREEN)
│   └── ...
│
├── 📦 Task 3: Frontend UI
│   ├── Subtask 3.1: Component LoginForm (UI only, no logic)
│   ├── Subtask 3.2: Viết test cho LoginForm validation
│   ├── Subtask 3.3: Kết nối LoginForm với API
│   └── ...
│
└── 📦 Task 4: Integration & E2E
    ├── Subtask 4.1: E2E test: đăng ký → đăng nhập → xem profile
    └── Subtask 4.2: Smoke test trên staging
```

#### 2.2 Quy tắc chia Task

| Quy tắc | Ví dụ tốt ✅ | Ví dụ xấu ❌ |
|---|---|---|
| **2-5 phút / subtask** | "Viết test validate email trống" | "Làm hệ thống validate" |
| **1 file / subtask** | "Tạo file `UserService.ts`" | "Code backend" |
| **Có test đi kèm** | "RED: test login fail khi sai password" | "Viết code login" |
| **Có done criteria** | "Test pass + API trả 201" | "Xong thì xong" |
| **Thứ tự logic** | DB → Backend → Frontend → E2E | Random |

---

### BƯỚC 3: ƯỚC TÍNH THỜI GIAN & ƯU TIÊN (AI làm, bạn review)

#### 3.1 Bảng ước tính

| # | Task | Subtasks | Ước tính | Ưu tiên | Phụ thuộc |
|---|---|---|---|---|---|
| 1 | Database Layer | 3 | 11 phút | 🔴 Cao | Không |
| 2 | Backend API | 8 | 30 phút | 🔴 Cao | Task 1 |
| 3 | Frontend UI | 5 | 20 phút | 🟡 Trung bình | Task 2 |
| 4 | Integration | 2 | 10 phút | 🟢 Thấp | Task 2, 3 |
| | **Tổng** | **18** | **~71 phút** | | |

#### 3.2 Dependency Graph (Thứ tự bắt buộc)

```
[Task 1: DB] ──> [Task 2: API] ──> [Task 3: UI] ──> [Task 4: E2E]
                                 └──> [Task 4: E2E]
```

> ⚠️ Không được làm Task 2 nếu Task 1 chưa xong. Không được làm Task 3 nếu Task 2 chưa có endpoint hoạt động.

**Bạn cần review:**
- Thứ tự ưu tiên có hợp lý không?
- Ước tính thời gian có thực tế không?
- Có task nào bỏ sót không?

---

### BƯỚC 4: TẠO FILE KẾ HOẠCH (AI làm)

Lưu file kế hoạch vào: `.obsidian-vault/07-Superpowers/Plans/YYYY-MM-DD-<topic>-plan.md`

Format file Plan:

```markdown
# Plan: [Tên tính năng]
> Spec: [link tới file Spec]
> Ngày tạo: YYYY-MM-DD
> Tổng ước tính: XX phút

## Task 1: Database Layer
- [ ] 1.1 Tạo migration bảng users (3p) → `src/db/migrations/001_create_users.ts`
- [ ] 1.2 Tạo migration bảng orders (3p) → `src/db/migrations/002_create_orders.ts`
- [ ] 1.3 Tạo seed data (5p) → `src/db/seeds/dev_seed.ts`

## Task 2: Backend API
- [ ] 2.1 🔴 Test POST /api/v1/users (3p) → `tests/api/users.test.ts`
- [ ] 2.2 🟢 Implement POST /api/v1/users (5p) → `src/routes/users.ts`
- [ ] 2.3 🔵 Refactor validation (3p)
...
```

---

### BƯỚC 5: XÁC NHẬN VÀ BẮT ĐẦU (Bạn làm)

Nếu đồng ý: **"OK, bắt đầu Task 1"**
Nếu cần sửa: **"Sửa lại: [điểm cần thay đổi]"**
Nếu muốn ưu tiên khác: **"Làm Task 3 trước"**
Nếu muốn bỏ bớt: **"Bỏ Task 4, để sau"**

---

### BƯỚC 6: CẬP NHẬT TIẾN ĐỘ LIÊN TỤC (AI làm, trong quá trình code)

Sau mỗi subtask hoàn thành, AI phải:
1. Đánh dấu `- [x]` trong file Plan
2. Ghi thời gian thực tế
3. Commit code (nếu đủ điều kiện)

```markdown
## Task 1: Database Layer
- [x] 1.1 Tạo migration bảng users (3p → thực tế: 4p) ✅
- [x] 1.2 Tạo migration bảng orders (3p → thực tế: 3p) ✅
- [/] 1.3 Tạo seed data (5p) ⏳ đang làm
- [ ] ...
```

---

## CHECKLIST TRƯỚC KHI BẮT ĐẦU CODE

```
- [ ] Spec đã được duyệt?
- [ ] Tất cả task đã chia nhỏ ≤ 5 phút?
- [ ] Mỗi task ghi rõ file path?
- [ ] Mỗi task có test / done criteria?
- [ ] Thứ tự dependency đúng?
- [ ] Ước tính thời gian hợp lý?
- [ ] User đã review và đồng ý Plan?
- [ ] File Plan đã lưu vào .obsidian-vault?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Không code nếu chưa có Plan** — Plan là bản đồ, code không có bản đồ = đi lạc.
2. **Task ≤ 5 phút** — Nếu task ước tính > 5 phút, chia nhỏ hơn nữa.
3. **Mỗi task = 1 TDD cycle** — RED → GREEN → REFACTOR → commit → tick checkbox.
4. **Cập nhật Plan liên tục** — File Plan là tài liệu sống, không phải viết xong rồi quên.
5. **Một Epic một lúc** — Không làm quá nhiều tính năng song song, dễ mất kiểm soát.
