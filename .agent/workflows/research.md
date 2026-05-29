---
title: Workflow — Research & Analysis
version: v2.0.0
document_code: SOP-DEV-003
---

# Workflow: Nghiên cứu Chuyên sâu (Research & Analysis)

> **Slash command:** `/research`
> Giai đoạn nghiên cứu toàn diện: thu thập dữ liệu từ nhiều nguồn chính thống, phân tích pháp lý, so sánh quốc tế, và khảo sát thị trường trước khi thiết kế.

---

## 🔍 MỤC TIÊU

- Thu thập thông tin **có nguồn gốc, có trích dẫn** từ các nguồn chính thống.
- Phân tích bối cảnh pháp lý, thị trường, và kỹ thuật một cách **chi tiết, có căn cứ**.
- So sánh với các giải pháp đã tồn tại ở Việt Nam và quốc tế.
- **KHÔNG VIẾT CODE** ở giai đoạn này. Chỉ tập trung nghiên cứu và tổng hợp.

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: Làm rõ Yêu cầu (Requirements Clarification)

- Xem xét lại bài toán hoặc ý tưởng mà User đã cung cấp.
- Đặt câu hỏi để làm rõ:
  - Mục tiêu cuối cùng là gì? (MVP hay sản phẩm hoàn chỉnh?)
  - Đối tượng người dùng là ai? (B2B, B2C, nội bộ?)
  - Có ràng buộc về ngân sách, thời gian, công nghệ không?
  - Các tiêu chí thành công (Success Criteria) cụ thể là gì?

---

### BƯỚC 2: Tìm kiếm Thông tin Đa nguồn (Multi-Source Research)

Sử dụng công cụ tìm kiếm (Tavily, Web Search) để thu thập thông tin từ **nhiều nguồn chính thống**. Mỗi thông tin tìm được **BẮT BUỘC** phải ghi rõ nguồn trích dẫn.

#### 2.1 Nguồn tìm kiếm bắt buộc

| Nguồn | Mục đích | Ví dụ tìm kiếm |
|---|---|---|
| **Google Search** | Tổng quan, bài báo, nghiên cứu | `"online education platform" site:techcrunch.com` |
| **GitHub** | Mã nguồn mở, thư viện, kiến trúc tham khảo | `repo:awesome-* education platform` |
| **X (Twitter)** | Xu hướng, ý kiến chuyên gia, thảo luận cộng đồng | `"edtech" lang:vi` |
| **Facebook** | Cộng đồng Việt Nam, nhóm chuyên ngành | Tìm trong các nhóm Dev/Startup VN |
| **TikTok** | Xu hướng người dùng trẻ, UX/UI trends | Hashtag liên quan đến sản phẩm |
| **YouTube** | Demo sản phẩm, tutorials, reviews | `"product name" review demo` |
| **Stack Overflow / Reddit** | Vấn đề kỹ thuật, kinh nghiệm thực tế | `[tag] "keyword"` |
| **Báo chí chính thống** | Tin tức, phân tích thị trường | VnExpress, TechInAsia, TechCrunch |
| **Tài liệu pháp luật** | Luật, nghị định, thông tư | `thuvienphapluat.vn`, `vanban.chinhphu.vn` |

#### 2.2 Quy tắc Trích dẫn Nguồn

Mỗi thông tin quan trọng phải được ghi theo format:

```
> "[Trích dẫn nguyên văn 1-2 câu quan trọng nhất]"
> — Nguồn: [Tên bài viết](URL), Tác giả, Ngày đăng
```

**Ví dụ:**
> "Thị trường EdTech Việt Nam dự kiến đạt 3 tỷ USD vào năm 2025."
> — Nguồn: [Vietnam EdTech Report 2024](https://example.com), TechInAsia, 15/03/2024

---

### BƯỚC 3: Phân tích Pháp lý & Quy định (Legal & Regulatory Analysis)

Nghiên cứu khung pháp lý liên quan đến lĩnh vực của dự án.

#### 3.1 Pháp luật Việt Nam
- Luật nào điều chỉnh trực tiếp? (Ví dụ: Luật Giao dịch điện tử, Luật An ninh mạng, Luật Bảo vệ dữ liệu cá nhân...)
- Nghị định nào hướng dẫn thi hành? (Số nghị định, ngày ban hành)
- Thông tư nào quy định chi tiết?
- Có giấy phép/chứng nhận nào cần xin không?

**Format ghi nhận:**
```
📜 Văn bản: [Tên luật/nghị định]
   Số hiệu: [Số/năm/loại văn bản]
   Điều khoản liên quan: Điều X, Khoản Y
   Nội dung tóm tắt: [Tóm tắt quy định]
   Link: [URL nguồn chính thống]
```

#### 3.2 Quy định Quốc tế (nếu liên quan)
- GDPR (Châu Âu) — Bảo vệ dữ liệu
- CCPA (California, Mỹ) — Quyền riêng tư
- APPI (Nhật Bản) — Bảo vệ thông tin cá nhân
- PIPL (Trung Quốc) — Luật bảo vệ thông tin cá nhân
- Các tiêu chuẩn ngành: PCI-DSS (thanh toán), HIPAA (y tế), SOC 2...

---

### BƯỚC 4: Khảo sát Thị trường Quốc tế (International Market Analysis)

So sánh các sản phẩm/dự án tương tự đã có trên thế giới.

#### 4.1 Bảng so sánh quốc tế

Lập bảng theo format:

| Quốc gia | Tên sản phẩm/dự án | URL | Tính năng nổi bật | Công nghệ sử dụng | Quy mô người dùng | Mô hình kinh doanh |
|---|---|---|---|---|---|---|
| 🇺🇸 Mỹ | | | | | | |
| 🇬🇧 Anh | | | | | | |
| 🇯🇵 Nhật Bản | | | | | | |
| 🇨🇳 Trung Quốc | | | | | | |
| 🇰🇷 Hàn Quốc | | | | | | |
| 🇸🇬 Singapore | | | | | | |
| 🇻🇳 Việt Nam | | | | | | |

#### 4.2 Phân tích chi tiết từng sản phẩm nổi bật
Với mỗi sản phẩm đáng chú ý, phân tích:
- **UX/UI:** Giao diện có gì đặc biệt? Luồng người dùng (User Flow) ra sao?
- **Tính năng cốt lõi:** Điểm mạnh nhất của họ là gì?
- **Kiến trúc kỹ thuật:** Stack công nghệ gì? (Frontend, Backend, Database, Infra)
- **Bài học rút ra:** Mình có thể học hỏi gì? Tránh sai lầm gì?

---

### BƯỚC 5: Khảo sát Thị trường Việt Nam (Vietnam Market Analysis)

#### 5.1 Sản phẩm tương tự tại Việt Nam

Lập danh sách các web/app tương tự đang hoạt động tại Việt Nam:

| Tên sản phẩm | URL | Đánh giá (1-5⭐) | Ưu điểm | Nhược điểm | Khoảng trống (Gap) mình có thể khai thác |
|---|---|---|---|---|---|
| | | | | | |

#### 5.2 Phân tích Khoảng trống Thị trường (Market Gap)
- Người dùng Việt Nam đang thiếu gì?
- Các đối thủ hiện tại đang làm chưa tốt ở điểm nào?
- Cơ hội khác biệt hóa (Differentiation) của mình là gì?

#### 5.3 Xu hướng Người dùng Việt Nam
- Thói quen sử dụng (Mobile-first? Desktop?)
- Phương thức thanh toán phổ biến (VNPay, MoMo, ZaloPay, thẻ quốc tế?)
- Ngôn ngữ và văn hóa cần lưu ý

---

### BƯỚC 6: Đánh giá Kỹ thuật (Technical Feasibility)

#### 6.1 Phân tích các giải pháp kỹ thuật
- Liệt kê 2-3 phương án công nghệ khả thi.
- Nêu rõ ưu/nhược điểm (trade-offs) của mỗi phương án về:
  - Hiệu năng (Performance)
  - Khả năng mở rộng (Scalability)
  - Thời gian phát triển (Time to Market)
  - Chi phí vận hành (Operational Cost)
  - Khả năng bảo trì (Maintainability)

#### 6.2 Khảo sát Thư viện & Công cụ
- Thư viện nào phù hợp? (Kiểm tra trên npm, PyPI, GitHub Stars, Last commit date)
- Có API bên thứ 3 nào cần tích hợp không? (Thanh toán, bản đồ, AI...)
- Rủi ro phụ thuộc (Dependency Risk): Thư viện còn được maintain không?

---

### BƯỚC 7: Phân tích Rủi ro (Risk Assessment)

| Loại rủi ro | Mô tả | Mức độ (Cao/TB/Thấp) | Biện pháp giảm thiểu |
|---|---|---|---|
| Kỹ thuật | | | |
| Pháp lý | | | |
| Thị trường | | | |
| Bảo mật | | | |
| Vận hành | | | |

---

### BƯỚC 8: Tổng hợp & Báo cáo Nghiên cứu (Research Report)

Sau khi hoàn tất tất cả các bước trên, tổng hợp thành một báo cáo nghiên cứu hoàn chỉnh với cấu trúc:

```
📄 BÁO CÁO NGHIÊN CỨU: [Tên dự án]
├── 1. Tóm tắt (Executive Summary) — 3-5 câu
├── 2. Bối cảnh & Yêu cầu
├── 3. Khung Pháp lý (Kèm trích dẫn luật/nghị định)
├── 4. Phân tích Thị trường Quốc tế (Bảng so sánh)
├── 5. Phân tích Thị trường Việt Nam (Gap Analysis)
├── 6. Đánh giá Kỹ thuật (2-3 phương án)
├── 7. Phân tích Rủi ro
├── 8. Đề xuất Giải pháp (Khuyến nghị cuối cùng)
└── 9. Tài liệu Tham khảo (Danh sách URL + Trích dẫn)
```

Lưu báo cáo vào: `.obsidian-vault/07-Superpowers/Research/YYYY-MM-DD-<topic>-research.md`

---

## ⚠️ NGUYÊN TẮC BẮT BUỘC

1. **Có nguồn mới được tin:** Mọi con số thống kê, nhận định thị trường, quy định pháp lý đều phải kèm URL nguồn gốc.
2. **Không bịa số liệu:** Nếu không tìm được dữ liệu chính xác, ghi rõ "Chưa tìm thấy dữ liệu chính thống" thay vì ước đoán.
3. **Đa chiều:** Không chỉ nhìn từ góc độ kỹ thuật. Phải bao gồm góc nhìn pháp lý, kinh doanh, và người dùng.
4. **Cập nhật:** Ưu tiên nguồn mới nhất (trong vòng 1-2 năm gần nhất).
