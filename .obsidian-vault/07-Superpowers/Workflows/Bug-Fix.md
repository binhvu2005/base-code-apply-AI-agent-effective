---
title: Workflow — Bug Fix
version: v2.0.0
document_code: SOP-DEV-011
---

# 🐛 WORKFLOW: SỬA LỖI (Bug Fix)

## Khi nào dùng?
Mỗi khi phát hiện bug, lỗi logic, lỗi UI/UX, hoặc hệ thống hoạt động sai so với mong đợi. Dù bug nhỏ hay lớn đều phải đi qua quy trình này.

---

## ⚔️ MỤC TIÊU DUY NHẤT

> **Sửa đúng lỗi. Không phát sinh lỗi mới. Không sửa lan.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI (Vi phạm bất kỳ điều nào = REJECT)

1. ❌ Không xác định được nguyên nhân hoặc giả thuyết trước khi sửa
2. ❌ Không khoanh vùng khu vực ảnh hưởng
3. ❌ Sửa lan sang code không liên quan
4. ❌ Không test lại sau khi sửa
5. ❌ Tự ý refactor diện rộng khi chưa được yêu cầu

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: MÔ TẢ LỖI (Bạn làm hoặc AI ghi nhận)

Mô tả bug theo format chuẩn:

```
🐛 BUG REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 Tiêu đề: [Mô tả ngắn gọn lỗi]
📍 Vị trí: [URL / Màn hình / Chức năng bị lỗi]
📱 Môi trường: [Browser / OS / Device / Phiên bản app]

🔄 Các bước tái tạo (Steps to Reproduce):
  1. [Bước 1]
  2. [Bước 2]
  3. [Bước 3]

❌ Kết quả thực tế (Actual): [Điều gì xảy ra]
✅ Kết quả mong đợi (Expected): [Điều gì lẽ ra phải xảy ra]

📸 Screenshot / Video: [Đính kèm nếu có]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Ví dụ tốt:**
> 📌 Tiêu đề: Nút "Thanh toán" không phản hồi khi giỏ hàng trống
> 📍 Vị trí: Trang /cart
> 🔄 Bước: 1) Xóa hết sản phẩm trong giỏ, 2) Click "Thanh toán"
> ❌ Actual: Không có phản hồi, nút bấm không làm gì
> ✅ Expected: Hiện thông báo "Giỏ hàng trống, vui lòng thêm sản phẩm"

**Ví dụ xấu:**
> "Nút thanh toán bị lỗi" ← Không có context, không tái tạo được

---

### BƯỚC 2: ĐỌC LOG TRƯỚC KHI ĐỌC CODE (AI làm — BẮT BUỘC)

> ⚠️ **LUẬT THÉP:** Nếu bug liên quan đến bot/server/backend, AI **BẮT BUỘC** phải đọc log terminal hoặc file log trước khi đọc code. Không được kết luận nguyên nhân khi chưa xem log thực tế.

#### 2.1 Nguồn Log cần kiểm tra

| Loại bug | Log cần đọc | Lệnh / Vị trí |
|---|---|---|
| Server/API crash | Terminal log | Đọc output terminal đang chạy server |
| API trả sai data | Network log | Browser DevTools → Network tab |
| Frontend render sai | Console log | Browser DevTools → Console tab |
| Database lỗi | DB log | File log của DB hoặc ORM output |
| Build lỗi | Build log | Output của `npm run build` |
| Test lỗi | Test log | Output của `npm test` |

#### 2.2 Ghi nhận Log

```
📋 LOG EVIDENCE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📍 Nguồn: [Terminal / DevTools / File log]
⏰ Thời điểm: [Timestamp từ log]
📝 Nội dung:
  [Copy nguyên văn đoạn log liên quan]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 3: KHOANH VÙNG & XÁC ĐỊNH NGUYÊN NHÂN (AI làm, bạn review)

AI phải hoàn thành bản phân tích sau TRƯỚC KHI viết bất kỳ dòng code sửa nào:

```
🔍 PHÂN TÍCH BUG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📝 Mô tả lỗi:
  [Tóm tắt lại lỗi bằng ngôn ngữ kỹ thuật]

🎯 Nguyên nhân / Giả thuyết:
  [Nguyên nhân gốc rễ (root cause) hoặc giả thuyết nếu chưa chắc chắn]
  [Nếu là giả thuyết, ghi rõ: "GIẢ THUYẾT — cần xác minh bằng..."]

📍 Khu vực ảnh hưởng (Scope):
  File(s): [Danh sách file liên quan — đường dẫn tuyệt đối]
  Function(s): [Tên hàm cụ thể]
  Line(s): [Dòng code cụ thể nếu biết]

🔧 Hướng sửa:
  [Mô tả cách sửa — CHỈ trong phạm vi khoanh vùng]

⚠️ Rủi ro:
  [Sửa chỗ này có thể ảnh hưởng đến đâu?]
  [Có side effect nào cần lưu ý không?]

🚫 KHÔNG SỬA (Out of scope):
  [Liệt kê các phần KHÔNG động vào trong lần sửa này]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Bạn cần review:**
- Nguyên nhân có thuyết phục không?
- Khoanh vùng có đúng không? Có bỏ sót chỗ nào không?
- Hướng sửa có hợp lý không?

Nếu đồng ý: **"OK, sửa đi"**
Nếu không: **"Kiểm tra lại: [chỗ cần xem thêm]"**

---

### BƯỚC 4: VIẾT TEST LỖI TRƯỚC (AI làm — TDD Red Phase)

> **LUẬT:** `NO FAILING TEST ➔ NO FIX CODE`

- Viết test case mô tả **đúng hành vi mong muốn** (hành vi mà bug đang vi phạm).
- Chạy test → **BẮT BUỘC FAIL** (chứng minh bug thực sự tồn tại trong code).
- Nếu test pass ngay → Bug không nằm ở chỗ bạn nghĩ, quay lại Bước 3.

```
🔴 RED PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 Test file: [đường dẫn file test]
🧪 Test case: [tên test case]
📊 Kết quả: FAIL ✅ (đúng mong đợi)
📋 Log: [paste output terminal]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 5: SỬA CODE TỐI THIỂU (AI làm — TDD Green Phase)

> **LUẬT:** Chỉ sửa đúng phạm vi đã khoanh vùng ở Bước 3. Không refactor. Không "tiện tay" sửa thêm.

```
🟢 GREEN PHASE
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 File sửa: [đường dẫn]
📍 Dòng sửa: [line X → line Y]
🔧 Thay đổi:
  TRƯỚC: [code cũ]
  SAU:   [code mới]
📊 Kết quả test: PASS ✅
📋 Log: [paste output terminal]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Quy tắc sửa:**
- ✅ Sửa đúng chỗ gây bug
- ✅ Sửa lượng code ít nhất có thể
- ❌ KHÔNG thêm tính năng mới
- ❌ KHÔNG đổi tên biến/hàm không liên quan
- ❌ KHÔNG format lại code file khác
- ❌ KHÔNG nâng cấp dependency

---

### BƯỚC 6: XÁC MINH KHÔNG REGRESSION (AI làm)

Chạy toàn bộ test suite để đảm bảo fix không làm hỏng gì khác:

```bash
npm test            # Tất cả tests cũ + mới đều PASS
npm run lint        # Không lỗi format
npm run build       # Build vẫn thành công
```

```
🔵 VERIFICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🧪 Test suite: [X/X] PASS ✅
🔍 Lint: PASS ✅
🏗️ Build: PASS ✅
📋 Log: [paste output terminal]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 7: GHI NHẬN PHÁT HIỆN THÊM (AI làm, nếu có)

> **LUẬT:** Nếu trong quá trình sửa bug, AI phát hiện thêm các lỗi logic, UI, UX khác NGOÀI phạm vi, **KHÔNG ĐƯỢC tự ý sửa**. Phải ghi riêng ra đây và đề xuất phương án.

```
💡 PHÁT HIỆN THÊM (Ngoài phạm vi bug hiện tại)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1. 🟡 [Mức độ: Thấp/TB/Cao]
   Mô tả: [Lỗi/vấn đề phát hiện được]
   Vị trí: [File + dòng]
   Đề xuất: [Cách xử lý]
   Ưu tiên: [Sửa ngay / Tạo task riêng / Bỏ qua]

2. 🟡 [Mức độ: Thấp/TB/Cao]
   Mô tả: ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

**Bạn quyết định:** Có muốn sửa thêm các lỗi phát hiện này không?
- "Sửa luôn số 1" → AI tạo bug fix mới cho lỗi đó
- "Tạo task riêng" → AI ghi vào backlog
- "Bỏ qua" → Không làm gì

---

### BƯỚC 8: BÁO CÁO HOÀN TẤT (AI làm)

```
📊 BÁO CÁO SỬA BUG
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 Bug: [Tiêu đề]
🎯 Nguyên nhân: [Root cause]
🔧 Giải pháp: [Tóm tắt cách sửa]
📝 File đã sửa: [Danh sách file]
🧪 Test: [X/X] PASS
🏗️ Build: PASS
💡 Phát hiện thêm: [X vấn đề] (xem chi tiết ở Bước 7)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CHECKLIST TỔNG THỂ

```
- [ ] Bug đã tái tạo được (reproduced)?
- [ ] Log đã đọc (nếu bug server/bot)?
- [ ] Nguyên nhân / giả thuyết đã xác định?
- [ ] Khu vực ảnh hưởng đã khoanh vùng?
- [ ] Rủi ro đã đánh giá?
- [ ] Test FAIL đã viết (Red Phase)?
- [ ] Code sửa tối thiểu, đúng phạm vi?
- [ ] Test PASS (Green Phase)?
- [ ] Toàn bộ test suite vẫn PASS (No Regression)?
- [ ] Build vẫn thành công?
- [ ] Phát hiện thêm đã ghi nhận (nếu có)?
- [ ] Báo cáo hoàn tất?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Đọc log trước đọc code** — Đặc biệt với bug server/bot/backend. Không có log = không có kết luận.
2. **Khoanh vùng trước khi sửa** — Xác định rõ file nào, hàm nào, dòng nào. Không "mò" toàn bộ codebase.
3. **Chỉ sửa trong phạm vi** — Không sửa lan. Không tiện tay refactor. Không "tối ưu thêm".
4. **Evidence before report** — Mọi kết luận phải kèm log terminal thực tế. Không "chắc là đã fix rồi".
5. **Phát hiện thêm = ghi riêng** — Không trộn lẫn fix bug hiện tại với fix bug mới phát hiện. Tách biệt rõ ràng.
