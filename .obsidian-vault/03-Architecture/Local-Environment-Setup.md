---
title: Local Environment Setup
author: Solo Developer
created: 2026-05-28
---

# Local Environment Setup

> Hướng dẫn thiết lập môi trường phát triển cục bộ cho Solo Developer và AI Agent.

## 1. Prerequisites
Đảm bảo máy của bạn đã cài đặt các công cụ sau:
- Node.js (phiên bản khuyến nghị: >= v20.x)
- Docker & Docker Compose (cho Database & Redis)
- Git

## 2. Clone Project & Install Dependencies
```bash
git clone [repository-url]
cd [repository-folder]

# Cài đặt thư viện
npm install
```

## 3. Environment Configuration
1. Copy file `.env.example` thành `.env`:
   ```bash
   cp .env.example .env
   ```
2. Điền các giá trị cấu hình tương ứng trong file `.env`.

## 4. Run Infrastructure (Docker)
Nếu dự án có sử dụng database hoặc redis cục bộ:
```bash
docker-compose up -d
```

## 5. Database Setup (Migrations & Seeding)
```bash
# Chạy migration để tạo các bảng
npx prisma migrate dev  # Hoặc lệnh tương đương của ORM bạn dùng

# Chạy seed dữ liệu mẫu (nếu có)
npm run seed
```

## 6. Run Application
```bash
# Chạy ở chế độ Development
npm run dev

# Chạy ở chế độ Production build
npm run build
npm run start
```

## 7. Verification Command
Để kiểm tra xem môi trường đã sẵn sàng chưa:
```bash
npm test
```
