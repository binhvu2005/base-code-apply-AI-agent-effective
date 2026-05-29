---
title: Workflow — Technical Research & Feasibility (Big Tech Standard)
version: v2.0.0
document_code: SOP-DEV-003
---

# 🔍 WORKFLOW: NGHIÊN CỨU KỸ THUẬT & KHẢ THI (Technical Research - RFC Prep)

## Khi nào dùng?
- Trước khi bắt tay vào thiết kế chi tiết (Spec) cho bất kỳ tính năng phức tạp nào.
- Khi cần đánh giá công nghệ mới, tích hợp bên thứ ba, hoặc giải quyết bài toán hiệu năng/mở rộng hệ thống.
- Quy trình này tuân theo tiêu chuẩn kỹ sư phần mềm tại các công ty Big Tech (Google, Meta, Amazon) nhằm đảm bảo nghiên cứu có chiều sâu, thực tế và có căn cứ số liệu.
- **Slash command:** `/research`

---

## ⚔️ MỤC TIÊU

> **Làm rõ bài toán, khảo sát các giải pháp hiện hữu, phân tích cặn kẽ các phương án thiết kế (Design Alternatives) kèm số liệu ước lượng, từ đó chọn ra hướng tiếp cận tối ưu nhất trước khi đi vào đặc tả (Spec).**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI (REJECT lập tức)

1. ❌ Nghiên cứu sơ sài, chỉ liệt kê định nghĩa mà không phân tích chiều sâu.
2. ❌ Không có phần so sánh các phương án kỹ thuật thay thế (Design Trade-offs).
3. ❌ Không thực hiện ước lượng hệ thống sơ bộ (Back-of-the-envelope calculation) khi thiết kế hệ thống lớn.
4. ❌ Đề xuất công nghệ cảm tính mà không dựa trên số liệu thực tế (GitHub stars, maintain status, performance benchmarks).
5. ❌ Bỏ qua các rủi ro vận hành (Operational Risk) và bảo mật/pháp lý.

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: XÁC ĐỊNH VẤN ĐỀ & GIẢ ĐỊNH (Problem Definition & Assumptions)

AI phải làm rõ mục tiêu nghiên cứu và các biên giới kỹ thuật:

- **Problem Statement:** Vấn đề cốt lõi cần giải quyết là gì? Tại sao các giải pháp hiện tại không đáp ứng được?
- **Scope (Phạm vi):** Những tính năng/yêu cầu nào thuộc phạm vi nghiên cứu?
- **Non-Goals (Ngoài phạm vi):** Những vấn đề gì được cố tình bỏ qua để thu hẹp phạm vi nghiên cứu?
- **Assumptions (Giả định):** Những giả định ban đầu về lượng dữ liệu, số người dùng, hoặc hạ tầng phần cứng có sẵn.

---

### BƯỚC 2: KHẢO SÁT CÔNG NGHỆ & GIẢI PHÁP HIỆN HỮU (Tech & Literature Survey)

Tìm kiếm thông tin đa nguồn (Google, GitHub, Reddit, StackOverflow, tài liệu kỹ thuật chính thống):

#### 2.1 Đánh giá Thư viện & Công cụ (Third-party Libraries):
Khi đề xuất sử dụng một thư viện open-source, bắt buộc phải thẩm định:
- **Liveness:** Thời gian commit cuối cùng (Last commit date) là khi nào?
- **Community:** Số lượng GitHub stars, Open issues, và tốc độ giải quyết pull requests.
- **Licensing:** Giấy phép sử dụng (MIT, Apache 2.0 là an toàn; GPL cần lưu ý).

#### 2.2 Quy tắc trích dẫn nguồn Big Tech:
Mọi số liệu benchmark hoặc quyết định thiết kế học hỏi từ các công ty khác phải có trích dẫn rõ ràng:
```
> "[Nội dung trích dẫn chi tiết hoặc kết quả benchmark]"
> — Nguồn: [Tên bài viết/tài liệu kỹ thuật](URL), Tác giả, Ngày đăng
```

---

### BƯỚC 3: ĐÁNH GIÁ PHƯƠNG ÁN THAY THẾ (Design Trade-offs)

> ⚠️ **LUẬT THÉP:** Luôn đưa ra tối thiểu **2 đến 3 phương án kỹ thuật** khác nhau để giải quyết vấn đề. Phân tích chi tiết ưu và nhược điểm của từng phương án.

Lập bảng so sánh chi tiết theo các tiêu chí:

| Tiêu chí | Phương án A | Phương án B | Phương án C |
|---|---|---|---|
| **Mô tả** | [Ví dụ: SQL Database] | [Ví dụ: NoSQL (MongoDB)] | [Ví dụ: In-memory Cache] |
| **Độ trễ (Latency)** | Trung bình | Thấp | Cực thấp |
| **Khả năng mở rộng** | Phức tạp (Sharding) | Dễ dàng (Scale ngang) | Giới hạn bởi RAM |
| **Độ tin cậy (Reliability)**| ACID Transactions | Eventual Consistency | Mất dữ liệu nếu crash |
| **Độ phức tạp Dev** | Thấp (Đã quen thuộc) | Trung bình | Cao |
| **Chi phí vận hành** | Trung bình | Trung bình | Cao |

---

### BƯỚC 4: ƯỚC LƯỢNG HỆ THỐNG SƠ BỘ (Back-of-the-envelope Estimation)

Đối với các hệ thống xử lý lượng lớn dữ liệu hoặc lượt truy cập cao, kỹ sư Big Tech luôn thực hiện tính toán sơ bộ để định hình tài nguyên cần thiết:

- **Ước lượng Dung lượng Lưu trữ (Storage Estimation):**
  - Số lượng write requests/ngày × Kích thước trung bình 1 record × Số ngày cần lưu giữ = Dung lượng ổ cứng cần dùng.
- **Ước lượng Băng thông (Bandwidth Estimation):**
  - QPS (Queries per second) trung bình và đỉnh × Kích thước trung bình của payload = Băng thông tối thiểu cần đáp ứng.
- **Ước lượng Bộ nhớ (Memory Estimation):**
  - Số lượng active data cần cache (Quy luật 80/20) × Kích thước record = Lượng RAM tối thiểu cho Cache cluster.

```
📊 BACK-OF-THE-ENVELOPE CALCULATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
• DAU (Daily Active Users): 1,000,000 users.
• Read Requests/user/day: 20 -> Tổng: 20,000,000 QPD.
• Write Requests/user/day: 2 -> Tổng: 2,000,000 QPD.
• Write QPS trung bình: 2,000,000 / 86,400s ≈ 23 QPS.
• Write QPS đỉnh (Peak QPS = 2x): 46 QPS.
• Kích thước 1 write payload: 500 Bytes.
• Lưu trữ trong 1 năm: 2,000,000 × 500 Bytes × 365 ngày ≈ 365 GB.
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 5: ĐÁNH GIÁ RỦI RO, BẢO MẬT & TUÂN THỦ (Risks & Compliance)

Phân tích các rủi ro hệ thống trước khi quyết định thiết kế:
- **Security Risks:** Khả năng bị tấn công DDoS, SQL Injection, rò rỉ token hoặc lộ thông tin nhạy cảm.
- **Compliance & Legal:** Quy định bảo vệ dữ liệu (GDPR đối với EU, PDPL đối với Việt Nam), lưu trữ dữ liệu tại chỗ (Nghị định 53 tại VN).
- **Single Point of Failure (SPOF):** Điểm yếu cốt tử nào có thể làm sập toàn bộ hệ thống nếu nó gặp sự cố?

---

### BƯỚC 6: TỔNG HỢP RFC & BÁO CÁO NGHIÊN CỨU (Research RFC Report)

Lưu báo cáo vào: `.obsidian-vault/07-Superpowers/Research/YYYY-MM-DD-<topic>-research.md`

Format file báo cáo nghiên cứu kỹ thuật chuẩn Big Tech (RFC - Request For Comments):

```markdown
# RFC: [Tên chủ đề nghiên cứu]
> Trạng thái: DRAFT / UNDER REVIEW
> Ngày tạo: YYYY-MM-DD
> Tác giả: AI Agent

## 1. Context & Objectives (Bối cảnh & Mục tiêu)
[Mô tả vấn đề thực tế cần giải quyết và các chỉ số đo lường thành công]

## 2. Technical Architecture Survey (Khảo sát công nghệ)
[Các thư viện, công cụ hiện tại có thể giải quyết bài toán]

## 3. Design Alternatives (Các phương án thiết kế)
### Phương án 1: [Tên phương án]
- Ưu điểm: ...
- Nhược điểm: ...
### Phương án 2: [Tên phương án]
- Ưu điểm: ...
- Nhược điểm: ...

## 4. System Capacity Estimation (Ước lượng năng lực hệ thống)
[Bảng tính toán sơ bộ dung lượng lưu trữ, băng thông, RAM]

## 5. Security & Operational Risks (Rủi ro & Bảo mật)
[Phân tích rủi ro an ninh mạng và tính sẵn sàng của hệ thống]

## 6. Engineering Recommendation (Khuyến nghị kỹ thuật)
[Lựa chọn phương án tối ưu nhất và lý do lựa chọn]
```

---

## CHECKLIST TỔNG THỂ TRƯỚC KHI CHUYỂN SANG SPEC

```
- [ ] Vấn đề cốt lõi đã được định nghĩa rõ ràng?
- [ ] Đã khảo sát và liệt kê các thư viện/công cụ hiện hữu?
- [ ] Có ít nhất 2 phương án thiết kế được phân tích ưu/nhược điểm (trade-offs)?
- [ ] Đã thực hiện ước lượng tài nguyên hệ thống (Back-of-the-envelope)?
- [ ] Đã phân tích các rủi ro về bảo mật và tuân thủ pháp lý?
- [ ] Khuyến nghị kỹ thuật cuối cùng có lập luận logic chặt chẽ?
- [ ] File RFC đã được tạo và lưu trữ đúng định dạng?
```

---

## RULES CỦA WORKFLOW NÀY

1. **No Code in Research** — Không viết một dòng code production nào ở giai đoạn này. Tập trung hoàn toàn vào kiến trúc và lý thuyết khả thi.
2. **Data-Driven Decisions** — Mọi lựa chọn thiết kế phải đi kèm số liệu benchmark hoặc tính toán dung lượng. Tránh dùng từ "hình như", "có vẻ tốt hơn".
3. **Always Question the Default** — Đừng luôn luôn chọn phương án quen thuộc nhất. Hãy thử thách các giả định bằng các công nghệ thay thế hiệu quả hơn.
4. **Assume High Scale** — Luôn thiết kế với tư duy hệ thống sẽ phình to gấp 10 lần trong tương lai để chuẩn bị các phương án mở rộng (Scaling path) rõ ràng.
