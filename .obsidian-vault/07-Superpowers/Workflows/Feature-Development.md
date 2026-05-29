---
title: Workflow — Feature Development
version: v2.0.0
document_code: SOP-DEV-001
---

# ✨ WORKFLOW: PHÁT TRIỂN TÍNH NĂNG MỚI (Feature Development)

## Khi nào dùng?
Khi bạn muốn thêm một tính năng mới vào hệ thống. Đây là workflow rút gọn 5 bước (so với Full Lifecycle 8 bước), tập trung vào Spec → Plan → Code → Tích hợp.

---

## ⚔️ MỤC TIÊU

> **Từ ý tưởng đến code chạy được, có test, có docs, trong một luồng kỷ luật và kiểm soát được.**

---

## 🗺️ SƠ ĐỒ 5 BƯỚC

```
[1. Spec] ➔ [2. Plan] ➔ [3. Môi trường] ➔ [4. TDD] ➔ [5. Tích hợp]
```

---

## 🛠️ CHI TIẾT TỪNG BƯỚC

### 📐 BƯỚC 1: Động não & Đặc tả (Brainstorming & Spec)

Nói với AI theo format:
```
"Tôi muốn thêm tính năng [X].
Người dùng sẽ [hành động].
Kết quả họ nhận được là [kết quả].
Ví dụ cụ thể: [ví dụ thực tế]."
```

**AI sẽ:**
1. Đặt câu hỏi làm rõ (mục tiêu, giới hạn, edge cases)
2. Đề xuất 2-3 phương án với trade-offs
3. Viết Spec đầy đủ (DB + API + Error Handling)
4. Lưu vào: `.obsidian-vault/07-Superpowers/Specs/YYYY-MM-DD-<topic>-design.md`

**Bạn:** Review Spec. Chỉ khi duyệt mới đi tiếp.

> Workflow chi tiết: Xem [specs.md](./.agent/workflows/specs.md)

---

### 📝 BƯỚC 2: Lập kế hoạch (Implementation Plan)

**AI sẽ:**
1. Phân rã Spec thành micro-tasks (2-5 phút mỗi task)
2. Ghi rõ file path, test mẫu, done criteria cho mỗi task
3. Ước tính thời gian và dependency
4. Lưu vào: `.obsidian-vault/07-Superpowers/Plans/YYYY-MM-DD-<topic>-plan.md`

**Bạn:** Review Plan. Điều chỉnh thứ tự nếu cần.

> Workflow chi tiết: Xem [planning.md](./.agent/workflows/planning.md)

---

### 🛡️ BƯỚC 3: Tạo môi trường cô lập (Isolated Workspace)

> **Mục tiêu:** Không code trực tiếp trên nhánh chính. Tránh xung đột và phá hỏng hệ thống hiện tại.

**AI sẽ:**

```bash
# Tạo branch mới
git worktree add .worktrees/feature-<tên> -b feature/<tên>
cd .worktrees/feature-<tên>

# Cài đặt dependencies
npm install

# Chạy baseline test — MỌI test phải PASS trước khi bắt đầu
npm test
```

```
🛡️ BASELINE CHECK
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📂 Branch: feature/<tên>
📂 Worktree: .worktrees/feature-<tên>
🧪 Baseline test: [X/X] PASS ✅
🏗️ Baseline build: PASS ✅
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

> ⚠️ Nếu baseline test FAIL → **DỪNG LẠI**, báo cáo ngay cho User/Tech Lead. Không tự ý sửa.

---

### 🔴🟢🔵 BƯỚC 4: Thực thi TDD & Xác thực

**AI sẽ** lặp lại cho mỗi subtask trong Plan:

1. **🔴 RED:** Viết test → chạy → FAIL ✅
2. **🟢 GREEN:** Code tối thiểu → chạy → PASS ✅
3. **🔵 REFACTOR:** Tối ưu → chạy → vẫn PASS ✅
4. **💾 COMMIT:** Commit theo Conventional Commits
5. **📋 UPDATE:** Tick `[x]` trong file Plan

```
🔄 PROGRESS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cycle 1: [subtask 1] — ✅ Done (3p)
Cycle 2: [subtask 2] — ✅ Done (5p)
Cycle 3: [subtask 3] — ⏳ In progress
...
Progress: [X/Y] subtasks — [XX]% complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

> Workflow chi tiết: Xem [tdd.md](./.agent/workflows/tdd.md)

---

### 🏁 BƯỚC 5: Hoàn thành & Tích hợp

Khi toàn bộ subtask đã `[x]`:

#### 5.1 Chạy CI/CD Pipeline

```bash
npm run lint        # Lint clean
npm test            # All tests PASS
npm run build       # Build thành công
```

#### 5.2 Tạo Pull Request

```bash
git push -u origin feature/<tên>
```

Tạo PR trên GitHub kèm:
- Link tới file Spec
- Link tới file Plan
- Tóm tắt thay đổi
- Screenshot (nếu có UI thay đổi)

#### 5.3 Code Review & Audit

Chạy audit theo [audit.md](./.agent/workflows/audit.md).

#### 5.4 Merge & Cleanup

Sau khi PR được approve:

```bash
# Merge vào nhánh chính
git checkout main
git merge feature/<tên>

# Dọn dẹp worktree
git worktree remove .worktrees/feature-<tên>
git worktree prune
git branch -d feature/<tên>
```

#### 5.5 Cập nhật Docs

Chạy cập nhật tài liệu theo [docs.md](./.agent/workflows/docs.md).

---

## CHECKLIST TRƯỚC KHI BẮT ĐẦU CODE

```
- [ ] Mục đích tính năng rõ ràng?
- [ ] Ai sẽ dùng tính năng này?
- [ ] Tính năng ảnh hưởng gì đến hệ thống hiện tại?
- [ ] Spec đã được duyệt?
- [ ] Plan đã chia đủ nhỏ?
- [ ] Branch/worktree đã tạo?
- [ ] Baseline test PASS?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Không skip Bước 1** — Mô tả rõ trước khi AI thiết kế. Garbage in = garbage out.
2. **Luôn review Plan** trước khi cho AI code — Đừng để AI chạy tự do.
3. **Một Epic một lúc** — Không làm quá nhiều tính năng song song.
4. **Ghi lại quyết định** — Tại sao chọn cách này, không phải cách khác.
5. **Baseline phải xanh** — Không bắt đầu code trên nền test đỏ.
