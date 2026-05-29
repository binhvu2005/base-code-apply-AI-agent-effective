---
title: Workflow — Release
version: v2.0.0
document_code: SOP-DEV-013
---

# 🚀 WORKFLOW: PHÁT HÀNH (Release)

## Khi nào dùng?
Khi tính năng/bugfix đã hoàn tất, đã qua audit, và sẵn sàng đưa lên production hoặc đánh version mới.

---

## ⚔️ MỤC TIÊU

> **Phát hành an toàn, có kiểm soát, có thể rollback. Không bao giờ release code chưa test.**

---

## 🚫 ĐIỀU KIỆN THẤT BẠI

1. ❌ Release khi test còn FAIL
2. ❌ Release khi audit có MUST FIX items chưa sửa
3. ❌ Release mà không có changelog
4. ❌ Release mà không có kế hoạch rollback
5. ❌ Release trực tiếp lên production mà không qua staging

---

## 🛠️ CÁC BƯỚC THỰC HIỆN

### BƯỚC 1: PRE-RELEASE CHECKLIST (AI làm)

> **Mục tiêu:** Kiểm tra lần cuối trước khi đánh version.

```
✅ PRE-RELEASE CHECKLIST
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Code:
  - [ ] Tất cả PR đã merge vào nhánh chính?
  - [ ] Không còn PR pending?
  - [ ] Nhánh chính (main) là phiên bản mới nhất?

🧪 Testing:
  - [ ] Full test suite PASS?
  - [ ] E2E tests PASS (nếu có)?
  - [ ] Manual testing trên staging PASS?
  - [ ] Performance testing OK (nếu cần)?

🔒 Security:
  - [ ] Audit PASS — 0 MUST FIX items?
  - [ ] npm audit — 0 critical vulnerabilities?
  - [ ] Secrets không bị hardcode?

📚 Documentation:
  - [ ] CHANGELOG.md đã cập nhật?
  - [ ] README.md đã cập nhật (nếu cần)?
  - [ ] API docs đã cập nhật?
  - [ ] Migration guide (nếu breaking changes)?

⚙️ Configuration:
  - [ ] .env production đã chuẩn bị?
  - [ ] Database migration đã sẵn sàng?
  - [ ] Third-party services đã cấu hình production?

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 2: ĐÁNH VERSION (AI làm, bạn quyết định version)

#### 2.1 Semantic Versioning (SemVer)

Format: `MAJOR.MINOR.PATCH`

| Khi nào | Version | Ví dụ |
|---|---|---|
| **Breaking changes** (API thay đổi, không tương thích ngược) | `MAJOR` ↑ | 1.0.0 → **2.0.0** |
| **Tính năng mới** (tương thích ngược) | `MINOR` ↑ | 1.0.0 → 1.**1.0** |
| **Sửa bug** (tương thích ngược) | `PATCH` ↑ | 1.0.0 → 1.0.**1** |

**Bạn quyết định:** Version tiếp theo là gì?

#### 2.2 Tạo Git Tag

```bash
# Cập nhật version trong package.json
npm version [major|minor|patch] -m "release: v%s"

# Hoặc thủ công
git tag -a v1.2.0 -m "Release v1.2.0: [mô tả ngắn]"
git push origin v1.2.0
```

---

### BƯỚC 3: TẠO RELEASE NOTES (AI làm)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📦 RELEASE NOTES — v[X.Y.Z]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📅 Ngày: YYYY-MM-DD

✨ Tính năng mới (Features):
  • [Tính năng 1] — [mô tả ngắn] (#PR_number)
  • [Tính năng 2] — [mô tả ngắn] (#PR_number)

🐛 Sửa lỗi (Bug Fixes):
  • [Bug 1] — [mô tả ngắn] (#PR_number)
  • [Bug 2] — [mô tả ngắn] (#PR_number)

♻️ Cải thiện (Improvements):
  • [Cải thiện 1] — [mô tả ngắn]

⚠️ Breaking Changes:
  • [Thay đổi] — [hướng dẫn migration]

🔒 Bảo mật (Security):
  • [Fix] — [mô tả]

📊 Thống kê:
  Commits: [X]
  Files changed: [X]
  Contributors: [danh sách]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 4: KẾ HOẠCH ROLLBACK (AI làm — BẮT BUỘC)

> **LUẬT:** Mọi release đều phải có kế hoạch rollback. Không có plan B = không được release.

```
🔙 ROLLBACK PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📌 Khi nào rollback:
  • [Điều kiện 1: ví dụ API response time > 2s]
  • [Điều kiện 2: ví dụ error rate > 5%]
  • [Điều kiện 3: ví dụ critical bug trong 24h đầu]

🔧 Cách rollback:

  Bước 1: Revert code
    git revert [commit-hash]
    # hoặc
    git checkout v[previous-version]

  Bước 2: Revert database (nếu có migration)
    npm run migrate:rollback

  Bước 3: Revert configuration
    [Các bước cụ thể]

  Bước 4: Verify
    npm test
    [Kiểm tra hệ thống hoạt động bình thường]

⏱️ Thời gian rollback ước tính: [X phút]
👤 Người chịu trách nhiệm: [tên]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 5: DEPLOY (Bạn quyết định timing)

#### 5.1 Deploy lên Staging trước

```bash
# Deploy staging
npm run deploy:staging
```

```
🧪 STAGING VERIFICATION
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🌐 URL: [staging URL]
✅ Smoke test: PASS
✅ API health check: PASS
✅ Critical flows: PASS
⏱️ Response time: [XX]ms (acceptable < [YY]ms)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

#### 5.2 Deploy lên Production

Chỉ khi staging OK:

```bash
# Deploy production
npm run deploy:production
```

```
🚀 PRODUCTION DEPLOY
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📦 Version: v[X.Y.Z]
📅 Thời điểm: YYYY-MM-DD HH:MM
🌐 URL: [production URL]
✅ Health check: PASS
✅ Smoke test: PASS
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 6: GIÁM SÁT SAU RELEASE (AI hỗ trợ)

> **LUẬT:** Theo dõi hệ thống ít nhất 24h sau khi release.

```
👀 POST-RELEASE MONITORING
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Theo dõi:
  📊 Error rate: [X]% (threshold: < 1%)
  ⏱️ Response time: [XX]ms (threshold: < 200ms)
  👥 Active users: [X] (so với trước release)
  🐛 Bug reports: [X] tickets mới

🎯 Kết luận sau 24h:
  ✅ Stable — Release thành công
  ⚠️ Issues detected — [mô tả] → tạo bug fix task
  ❌ Critical failure — KÍCH HOẠT ROLLBACK PLAN
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

---

### BƯỚC 7: DỌN DẸP SAU RELEASE (AI làm)

```bash
# Xóa branch đã merge
git branch -d feature/<tên>
git push origin --delete feature/<tên>

# Xóa worktree (nếu có)
git worktree remove .worktrees/feature-<tên>
git worktree prune

# Đánh dấu issues/tasks đã hoàn thành
```

---

## CHECKLIST TỔNG THỂ

```
- [ ] Pre-release checklist đã xong?
- [ ] Version number đã quyết định (SemVer)?
- [ ] Git tag đã tạo?
- [ ] Release notes đã viết?
- [ ] Rollback plan đã chuẩn bị?
- [ ] Staging deploy + verify OK?
- [ ] Production deploy + verify OK?
- [ ] Post-release monitoring 24h?
- [ ] Cleanup branches/worktrees?
```

---

## RULES CỦA WORKFLOW NÀY

1. **Staging trước Production** — Không deploy thẳng lên production. Không ngoại lệ.
2. **Có Rollback Plan** — Không có plan B = không được release.
3. **Monitor 24h** — Release xong không phải là xong. Theo dõi cho đến khi chắc chắn stable.
4. **SemVer nghiêm ngặt** — Breaking change = MAJOR. Tính năng mới = MINOR. Bugfix = PATCH.
5. **Evidence before celebrate** — Đợi monitoring xanh rồi mới ăn mừng.
