---
title: Workflow — Performance Optimization
version: v2.0.0
document_code: SOP-DEV-015
---

# ⚡ WORKFLOW: TỐI ƯU HIỆU NĂNG (Performance Optimization)

## Khi nào dùng?
- Khi ứng dụng bị phản hồi chậm, xuất hiện các trang/API có thời gian tải (Response Time) lớn hơn **200ms**.
- Trước khi triển khai một tính năng dự kiến sẽ xử lý lượng dữ liệu lớn hoặc số lượng người dùng đồng thời cao (High Throughput).
- Khi hệ thống tiêu thụ tài nguyên (CPU, RAM, DB Connections) quá cao trên server.
- **Slash command:** `/performance`

---

## ⚔️ MỤC TIÊU

> **Xác định chính xác nguyên nhân gây chậm (Bottleneck), cải thiện tốc độ phản hồi của API/Frontend mà không ảnh hưởng đến tính đúng đắn của logic, đồng thời giảm thiểu tiêu tốn tài nguyên hệ thống.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI (REJECT lập tức)

1. ❌ Tối ưu hóa cảm tính (Premature Optimization) mà không có số liệu đo lường thực tế (Metrics/Logs).
2. ❌ Tối ưu hiệu năng gây lỗi logic hoặc sai lệch dữ liệu (Regression).
3. ❌ Sử dụng truy vấn N+1 (truy vấn trong vòng lặp) khi làm việc với cơ sở dữ liệu.
4. ❌ Không thiết lập Pagination (phân trang) cho các danh sách dữ liệu có khả năng tăng dần theo thời gian.
5. ❌ Tối ưu xong nhưng không chạy benchmark đối chiếu trước/sau để chứng minh kết quả.

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: ĐO LƯỜNG & PHÁT HIỆN BỐT CỔ CHAI (Metrics & Profiling)

> ⚠️ **LUẬT THÉP:** Chỉ tối ưu hóa khi đã có số liệu chứng minh hệ thống đang chậm ở đâu. Không được đoán mò.

#### 1.1 Thu thập số liệu hiệu năng hiện tại (Baseline Metrics):
Chạy Profiling hoặc đọc các thông số giám sát hệ thống:

```
📊 BASELINE PERFORMANCE METRICS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 Endpoint/Chức năng: [Ví dụ: GET /api/v1/orders]
⏱️ Response Time hiện tại (P95/P99): [Ví dụ: 850ms]
📈 Throughput: [Ví dụ: 20 req/s]
💻 Tài nguyên sử dụng: CPU [XX]%, RAM [XX]%
🗃️ DB Queries: [Ví dụ: 101 queries cho 1 request]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 1.2 Khoanh vùng Bottleneck:
- **Database:** Query chậm, thiếu index, hoặc kết nối DB bị nghẽn?
- **Network:** Payload quá lớn, không nén dữ liệu?
- **Backend CPU:** Thuật toán quá phức tạp, xử lý đồng bộ các tác vụ có thể chạy bất đồng bộ?
- **Frontend:** Quá nhiều DOM elements, re-render liên tục, bundle size quá lớn?

---

### BƯỚC 2: TỐI ƯU HÓA TRUY VẤN DATABASE (Database Tuning)

#### 2.1 Loại bỏ hoàn toàn lỗi N+1 Query:
- Luôn sử dụng Eager Loading (`include`, `join`, `populate`) thay vì Lazy Loading khi query dữ liệu liên quan.

```typescript
// ❌ DANGEROUS: N+1 query (1 query lấy orders, sau đó N query lấy user cho từng order)
const orders = await db.order.findMany();
for (const order of orders) {
  order.user = await db.user.findUnique({ where: { id: order.userId } });
}

// ✅ SAFE: Eager Loading (chỉ 1 truy vấn JOIN hoặc 2 truy vấn song song)
const orders = await db.order.findMany({
  include: { user: true }
});
```

#### 2.2 Tạo Index cho các cột thường xuyên tìm kiếm/JOIN:
- Mọi trường nằm trong câu lệnh `WHERE`, `JOIN (ON)`, `ORDER BY` cần được đánh chỉ mục (Index) nếu bảng có nhiều hơn 1000 records.
- Sử dụng `EXPLAIN ANALYZE` trong SQL để kiểm tra xem hệ thống có đang quét toàn bộ bảng (Table Scan) hay dùng Index Scan.

#### 2.3 Phân trang bắt buộc (Pagination):
- Cấm trả về toàn bộ dữ liệu của một bảng qua API mà không giới hạn.
- Sử dụng Offset-based hoặc Cursor-based pagination:
  ```
  ✅ SAFE: GET /api/v1/orders?page=1&limit=20
  ```

---

### BƯỚC 3: TỐI ƯU HÓA BACKEND & CACHING (API Optimization)

#### 3.1 Sử dụng Caching cho dữ liệu ít thay đổi:
- Với các dữ liệu dạng cấu hình, danh mục sản phẩm, thông tin hệ thống: Lưu vào bộ nhớ đệm (Redis, In-memory cache) kèm cơ chế TTL (Time To Live) phù hợp.

#### 3.2 Xử lý song song (Parallelization):
- Khi cần gọi nhiều API độc lập hoặc nhiều queries không phụ thuộc nhau, hãy dùng `Promise.all` thay vì `await` tuần tự.

```typescript
// ❌ SLOW: Chạy tuần tự (mất T1 + T2)
const profile = await getUserProfile(userId);
const orders = await getUserOrders(userId);

// ✅ FAST: Chạy song song (mất max(T1, T2))
const [profile, orders] = await Promise.all([
  getUserProfile(userId),
  getUserOrders(userId)
]);
```

#### 3.3 Nén dữ liệu truyền tải (Gzip/Brotli):
- Kích hoạt middleware nén dữ liệu trên server để giảm kích thước payload truyền qua network.

---

### BƯỚC 4: TỐI ƯU HÓA FRONTEND (Frontend Performance)

#### 4.1 Giảm thiểu re-render không cần thiết:
- Sử dụng `useMemo`, `useCallback` (trong React) hoặc các kỹ thuật memoization để tránh tính toán lại.

#### 4.2 Tối ưu hóa tải tài nguyên (Assets):
- Sử dụng lazy loading cho components và hình ảnh ngoài khung nhìn (above-the-fold).
- Nén ảnh, chuyển sang định dạng hiện đại (WebP, AVIF).
- Bundle splitting: Chia nhỏ file JS lớn thành các file nhỏ hơn để trình duyệt tải song song.

#### 4.3 Sử dụng Debounce / Throttle:
- Với các sự kiện kích hoạt liên tục như `onScroll`, `onResize`, `onKeyUp` (trong ô tìm kiếm), bắt buộc phải dùng Debounce/Throttle để tránh gửi quá nhiều request dồn dập về server.

---

### BƯỚC 5: LOAD TESTING & BENCHMARK (Xác minh kết quả)

Sau khi tối ưu, AI chạy benchmark lại để thu được số liệu đối chiếu:

```
🚀 PERFORMANCE IMPROVEMENT REPORT
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📌 Endpoint/Chức năng: [GET /api/v1/orders]

📊 ĐỐI CHIẾU TRƯỚC VÀ SAU:
┌───────────────────┬──────────────┬──────────────┬──────────────┐
│ Chỉ số            │ Trước        │ Sau          │ Cải thiện    │
├───────────────────┼──────────────┼──────────────┼──────────────┤
│ Response Time P95 │ 850ms        │ 120ms        │ 🚀 Giảm 85.8%│
│ Throughput        │ 20 req/s     │ 150 req/s    │ 📈 Tăng 7.5x │
│ Số lượng DB query │ 101          │ 1            │ 📉 Giảm 99%  │
│ Kích thước Payload│ 2.4 MB       │ 320 KB       │ 📉 Giảm 86.6%│
└───────────────────┴──────────────┴──────────────┴──────────────┘

📋 Log/Bằng chứng đo lường thực tế:
  [Paste log chạy autocannon, k6, hoặc output console.time]

🎯 Kết luận: Hiệu năng đạt tiêu chuẩn. Không phát hiện regression.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

## CHECKLIST TỔNG THỂ TỐI ƯU HIỆU NĂNG

```
- [ ] Đã đo lường và ghi nhận số liệu ban đầu (Baseline metrics) trước khi tối ưu?
- [ ] Không còn lỗi truy vấn N+1 (SELECT trong vòng lặp) đối với cơ sở dữ liệu?
- [ ] Đã thêm Index cho tất cả các trường được tìm kiếm (WHERE/JOIN/ORDER BY)?
- [ ] Tất cả các API trả về danh sách đều đã tích hợp Pagination?
- [ ] Đã áp dụng Caching cho các dữ liệu ít thay đổi để giảm tải cho DB?
- [ ] Các tác vụ độc lập được xử lý song song thay vì tuần tự (Promise.all)?
- [ ] Sử dụng debounce/throttle cho các input search hoặc scroll events ở Frontend?
- [ ] assets lớn (hình ảnh, scripts) đã được nén hoặc lazy load?
- [ ] Đã chạy benchmark đối chiếu trước/sau và đính kèm bằng chứng cải thiện?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Measure Twice, Cut Once** — Không tối ưu hóa dựa trên phỏng đoán. Phải có công cụ đo và xuất báo cáo.
2. **Correctness First** — Ứng dụng chạy nhanh nhưng sai kết quả thì hoàn toàn vô giá trị. Tính đúng đắn của logic luôn là ưu tiên hàng đầu.
3. **Keep It Simple** — Nhiều khi giải pháp tối ưu đơn giản nhất lại là hiệu quả nhất (ví dụ: thêm 1 index, hoặc caching đơn giản). Tránh làm phức tạp hóa code không cần thiết.
4. **No Premature Optimization** — Không dành hàng giờ để tối ưu những dòng code chỉ chạy 1 lần/ngày. Tập trung vào 20% code gây ra 80% độ trễ (Quy luật Pareto).
5. **No N+1 in Code Review** — Đây là lỗi sơ đẳng nhưng phá hủy hiệu năng DB nhanh nhất. Phải rà soát thật kỹ lỗi này.
