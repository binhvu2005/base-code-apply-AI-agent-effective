---
title: Workflow — Test Driven Development (TDD)
version: v2.0.0
document_code: SOP-DEV-006
---

# 🔴🟢🔵 WORKFLOW: LẬP TRÌNH HƯỚNG KIỂM THỬ (TDD)

## Khi nào dùng?
Mỗi khi viết code production — dù là tính năng mới, sửa bug, hay refactor. **Không có ngoại lệ.** `NO FAILING TEST ➔ NO PRODUCTION CODE`.

---

## ⚔️ MỤC TIÊU

> **Viết test trước, code sau. Mỗi dòng code đều có lý do tồn tại — vì nó làm một test chuyển từ đỏ sang xanh.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Viết code production trước khi có test FAIL
2. ❌ Test viết xong mà PASS ngay (test vô dụng hoặc sai)
3. ❌ Viết code "đón đầu" tính năng chưa có test
4. ❌ Refactor mà không chạy lại test suite
5. ❌ Báo cáo "xong" mà không có log test thực tế

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 0: CHUẨN BỊ (AI làm)

Trước khi bắt đầu TDD cycle:
1. Mở file Plan: `.obsidian-vault/07-Superpowers/Plans/YYYY-MM-DD-<topic>-plan.md`
2. Xác định subtask tiếp theo cần làm (task chưa tick `- [ ]`)
3. Đảm bảo baseline test suite đang PASS hoàn toàn

```bash
npm test  # Phải 100% PASS trước khi bắt đầu
```

Nếu baseline FAIL → **DỪNG LẠI**, báo cáo ngay, không tự ý sửa.

---

### BƯỚC 1: 🔴 RED PHASE — VIẾT TEST LỖI (AI làm)

> **Mục tiêu:** Viết MỘT test case mô tả hành vi mong muốn. Test PHẢI FAIL.

#### 1.1 Chọn hành vi cần test

Lấy từ Spec/Plan. Ví dụ:
- "Khi user đăng ký với email đã tồn tại, API trả 400 + message lỗi"
- "Khi giỏ hàng trống, nút thanh toán bị disable"

#### 1.2 Viết test

```
🔴 RED PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 Subtask: [tên subtask từ Plan]
📂 Test file: [đường dẫn file test]
🧪 Test name: [tên test case — mô tả hành vi, không mô tả implementation]

📋 Test code:
  [paste đoạn test vừa viết]

📊 Kết quả chạy test:
  ❌ FAIL (đúng mong đợi)
  Message: [error message từ terminal]

📋 Log terminal: [paste output thực tế]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 1.3 Quy tắc đặt tên test

| Pattern | Ví dụ tốt ✅ | Ví dụ xấu ❌ |
|---|---|---|
| `should [hành vi] when [điều kiện]` | `should return 400 when email exists` | `test register` |
| `[hành động] → [kết quả]` | `register with duplicate email → error` | `testAPI` |
| Tiếng Việt cũng OK | `nên trả 400 khi email trùng` | `test1` |

#### 1.4 Nếu test PASS ngay → CÓ VẤN ĐỀ

```
⚠️ TEST PASS NGAY KHI CHƯA CÓ CODE → KIỂM TRA:
  1. Test có đang test đúng thứ không?
  2. Code đã tồn tại từ trước?
  3. Test bị mock sai, luôn trả true?
  → Sửa test hoặc báo cáo cho User trước khi tiếp tục.
```

---

### BƯỚC 2: 🟢 GREEN PHASE — VIẾT CODE TỐI THIỂU (AI làm)

> **Mục tiêu:** Viết code **đơn giản nhất, ít nhất** để test chuyển PASS. Không hơn.

#### 2.1 Nguyên tắc YAGNI (You Aren't Gonna Need It)

| Được phép ✅ | Cấm ❌ |
|---|---|
| Return hardcode value để pass test | Viết cả hệ thống caching "phòng xa" |
| If/else đơn giản | Design pattern phức tạp khi chưa cần |
| Code xấu nhưng đúng | Code đẹp nhưng dư thừa |

#### 2.2 Báo cáo Green Phase

```
🟢 GREEN PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📂 File sửa/tạo: [đường dẫn]
📍 Thay đổi:
  [Mô tả ngắn gọn code vừa viết]

📊 Kết quả chạy test:
  ✅ PASS
  [X/X tests pass]

📋 Log terminal: [paste output thực tế]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 3: 🔵 REFACTOR PHASE — TỐI ƯU HÓA (AI làm)

> **Mục tiêu:** Cải thiện cấu trúc code mà KHÔNG thay đổi hành vi. Test vẫn PASS sau refactor.

#### 3.1 Checklist Refactor

- [ ] Có code trùng lặp (DRY)? → Gom lại
- [ ] Tên biến/hàm có rõ nghĩa không? → Đổi tên
- [ ] Hàm quá dài (> 20 dòng)? → Tách hàm
- [ ] Magic number/string? → Tạo hằng số
- [ ] Comment giải thích code phức tạp? → Thêm nếu cần

#### 3.2 Báo cáo Refactor Phase

```
🔵 REFACTOR PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔧 Những gì đã refactor:
  1. [Mô tả thay đổi 1]
  2. [Mô tả thay đổi 2]

📊 Kết quả chạy test sau refactor:
  ✅ PASS — [X/X tests pass] — Không regression
  
📋 Log terminal: [paste output thực tế]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 4: COMMIT & CẬP NHẬT PLAN (AI làm)

Sau mỗi TDD cycle hoàn tất (RED → GREEN → REFACTOR):

1. **Commit** code với message chuẩn:
   ```
   test(users): add test for duplicate email registration
   feat(users): implement duplicate email check
   refactor(users): extract validation logic
   ```

2. **Cập nhật Plan:** Đánh dấu `- [x]` cho subtask vừa xong

3. **Báo cáo tiến độ:**
   ```
   📊 TIẾN ĐỘ: [X/Y] subtasks hoàn thành
   ⏱️ Thời gian thực tế: Xp (ước tính: Yp)
   ⏭️ Subtask tiếp theo: [tên subtask]
   ```

---

### BƯỚC 5: LẶP LẠI (AI làm)

Quay lại **Bước 1 (RED)** với subtask tiếp theo cho đến khi:
- Tất cả subtask trong Task hiện tại đã `[x]`
- Toàn bộ test suite vẫn PASS
- Code đã clean sau refactor

```
🔄 TDD CYCLE LOOP
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Cycle 1: [subtask 1] — RED ✅ → GREEN ✅ → REFACTOR ✅ → COMMIT ✅
Cycle 2: [subtask 2] — RED ✅ → GREEN ✅ → REFACTOR ✅ → COMMIT ✅
Cycle 3: [subtask 3] — RED ⏳ (đang làm)
...
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CÁC LOẠI TEST CẦN BIẾT

| Loại test | Mục đích | Khi nào viết | Ví dụ |
|---|---|---|---|
| **Unit Test** | Test 1 hàm/module cô lập | Mỗi TDD cycle | `validateEmail()` trả đúng/sai |
| **Integration Test** | Test nhiều module phối hợp | Khi hoàn thành 1 Task | API endpoint + DB |
| **E2E Test** | Test toàn bộ luồng người dùng | Khi hoàn thành Epic | Đăng ký → Login → Xem profile |

---

## CHECKLIST TỔNG THỂ

```
- [ ] Baseline test suite PASS trước khi bắt đầu?
- [ ] Mỗi TDD cycle có đủ 3 phase (RED → GREEN → REFACTOR)?
- [ ] RED phase: test thực sự FAIL?
- [ ] GREEN phase: code tối thiểu, không dư thừa?
- [ ] REFACTOR phase: test vẫn PASS sau khi tối ưu?
- [ ] Commit sau mỗi cycle?
- [ ] Plan được cập nhật liên tục?
- [ ] Toàn bộ test suite vẫn PASS ở cuối?
```

---

## RULES CỦA WORKFLOW NÀY

1. **RED trước GREEN** — Không viết code khi chưa có test FAIL. Không ngoại lệ.
2. **Code tối thiểu** — Chỉ viết đủ để pass test. Không "tiện tay" thêm logic.
3. **Refactor có bảo vệ** — Chỉ refactor khi đã có test xanh bảo vệ. Test phải vẫn PASS sau refactor.
4. **Evidence trước báo cáo** — Paste log terminal thực tế. Không "chắc là pass rồi".
5. **1 cycle = 1 hành vi** — Mỗi TDD cycle chỉ giải quyết 1 hành vi duy nhất, không gộp.
