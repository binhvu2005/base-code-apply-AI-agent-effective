---
title: Workflow — Refactor
version: v2.0.0
document_code: SOP-DEV-012
---

# ♻️ WORKFLOW: TÁI CẤU TRÚC CODE (Refactor)

## Khi nào dùng?
Khi code đang hoạt động đúng nhưng cần cải thiện cấu trúc, hiệu năng, hoặc khả năng bảo trì. **Refactor KHÔNG thay đổi hành vi.**

---

## ⚔️ MỤC TIÊU

> **Cải thiện chất lượng code mà KHÔNG làm thay đổi bất kỳ hành vi nào. Test trước và sau refactor phải cho kết quả giống nhau 100%.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Refactor mà chưa có test bảo vệ (coverage)
2. ❌ Test FAIL sau refactor (đã thay đổi hành vi)
3. ❌ "Tiện tay" thêm tính năng mới trong lúc refactor
4. ❌ Refactor quá nhiều file cùng lúc (rủi ro cao)
5. ❌ Không giải thích WHY cần refactor

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: XÁC ĐỊNH LÝ DO REFACTOR (Bạn làm hoặc AI đề xuất)

Nói với AI theo format:
```
"Tôi muốn refactor [phần nào].
Lý do: [tại sao cần refactor?]
Mục tiêu: [code sau refactor sẽ tốt hơn ở điểm nào?]"
```

**Lý do hợp lệ:**

| Lý do | Ví dụ |
|---|---|
| **Code trùng lặp (DRY)** | 3 file cùng chứa logic validate email |
| **Hàm quá dài** | `processOrder()` dài 200 dòng |
| **Naming kém** | Biến `d`, `tmp`, `x1` |
| **Tight coupling** | Component A phụ thuộc trực tiếp vào implementation của B |
| **Performance** | N+1 query, missing index |
| **Tech debt** | Workaround cũ cần được sửa đúng |

**Lý do KHÔNG hợp lệ:**
- ❌ "Thấy code xấu quá" (quá chung chung)
- ❌ "Muốn thử framework mới" (không phải refactor)
- ❌ "Tiện tay sửa luôn" (phải có plan)

---

### BƯỚC 2: KIỂM TRA TEST COVERAGE (AI làm — BẮT BUỘC)

> **LUẬT THÉP:** Không refactor code chưa có test bảo vệ. Test là tấm khiên duy nhất đảm bảo bạn không phá hỏng gì.

```bash
npm test -- --coverage
```

```
🛡️ COVERAGE CHECK TRƯỚC REFACTOR
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📂 File cần refactor: [đường dẫn]

📊 Coverage hiện tại:
  Statements: [XX]%
  Branches:   [XX]%
  Functions:  [XX]%
  Lines:      [XX]%

🎯 Đánh giá:
  ✅ Coverage đủ (≥ 80%) → Tiến hành refactor
  ⚠️ Coverage thấp (< 80%) → Viết thêm test TRƯỚC khi refactor
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

Nếu coverage thấp → **DỪNG LẠI**, viết thêm test cho code hiện tại trước. Test phải PASS (vì code hiện tại đang đúng).

---

### BƯỚC 3: LÊN KẾ HOẠCH REFACTOR (AI làm, bạn review)

```
📋 REFACTOR PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🎯 Mục tiêu: [Mô tả code sau refactor sẽ khác như nào]

📍 Phạm vi (Scope):
  File(s): [Danh sách file sẽ thay đổi]
  
🔧 Thay đổi dự kiến:
  1. [Thay đổi 1]: [file] — [mô tả]
  2. [Thay đổi 2]: [file] — [mô tả]
  3. ...

🚫 KHÔNG ĐỘNG VÀO:
  [Liệt kê các file/phần KHÔNG sửa]

⚠️ Rủi ro:
  [Thay đổi này có thể ảnh hưởng đến...]

📊 Ước tính: [X phút]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Bạn review:** Đồng ý? → "OK, refactor đi"

---

### BƯỚC 4: THỰC HIỆN REFACTOR (AI làm)

> **LUẬT:** Refactor từng bước nhỏ. Sau mỗi bước, chạy test. Nếu test FAIL → revert ngay.

```
♻️ REFACTOR EXECUTION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Step 1: [Mô tả thay đổi]
  📂 File: [đường dẫn]
  🔧 Thay đổi: [TRƯỚC → SAU]
  🧪 Test: ✅ PASS — Không regression

Step 2: [Mô tả thay đổi]
  📂 File: [đường dẫn]
  🔧 Thay đổi: [TRƯỚC → SAU]
  🧪 Test: ✅ PASS — Không regression

Step 3: ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 4.1 Nếu test FAIL giữa chừng

```
❌ TEST FAIL SAU REFACTOR STEP [X]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧪 Test failed: [tên test]
🔍 Nguyên nhân: [phân tích]

Hành động:
  [ ] Revert step [X] → quay lại trạng thái xanh
  [ ] Tìm cách refactor khác an toàn hơn
  [ ] Báo User nếu cần thay đổi approach
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 5: XÁC MINH TOÀN DIỆN (AI làm)

```bash
npm test -- --coverage    # Test + coverage sau refactor
npm run lint              # Lint clean
npm run build             # Build thành công
```

```
✅ REFACTOR VERIFICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧪 Test: [X/X] PASS
📊 Coverage TRƯỚC: [XX]%
📊 Coverage SAU:   [XX]% (phải ≥ trước)
🔍 Lint: PASS
🏗️ Build: PASS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

> ⚠️ Coverage sau refactor phải **bằng hoặc cao hơn** coverage trước refactor. Nếu giảm → có code mới chưa được test.

---

### BƯỚC 6: COMMIT & BÁO CÁO (AI làm)

```bash
git add [files]
git commit -m "refactor(scope): mô tả thay đổi"
```

```
📊 REFACTOR REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🎯 Mục tiêu: [đã đạt được gì]
📂 Files changed: [số file]
🧪 Tests: [X/X] PASS — Không regression
📊 Coverage: [XX]% → [XX]%
✏️ Lines: +[X] / -[Y] (net: [Z])

Cải thiện cụ thể:
  ✅ [Trước: X → Sau: Y]
  ✅ [Trước: X → Sau: Y]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CHECKLIST TỔNG THỂ

```
- [ ] Lý do refactor rõ ràng và hợp lệ?
- [ ] Test coverage đủ trước khi bắt đầu?
- [ ] Plan refactor đã được User review?
- [ ] Refactor từng bước nhỏ, test sau mỗi bước?
- [ ] Không thay đổi hành vi (behavior)?
- [ ] Không thêm tính năng mới?
- [ ] Test PASS sau refactor?
- [ ] Coverage ≥ trước refactor?
- [ ] Lint + Build PASS?
- [ ] Commit message đúng format refactor?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Test trước refactor** — Không có test = không được refactor. Viết test trước.
2. **Không đổi hành vi** — Nếu user thấy gì khác sau refactor, bạn đã làm sai.
3. **Từng bước nhỏ** — Refactor big-bang = thảm họa. Mỗi step phải test được.
4. **Revert nếu test fail** — Không cố sửa forward. Quay lại trạng thái xanh trước.
5. **Giải thích WHY** — Commit message phải nói rõ tại sao refactor, không chỉ "refactor code".
