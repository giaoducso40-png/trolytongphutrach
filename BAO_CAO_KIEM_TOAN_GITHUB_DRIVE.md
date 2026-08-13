# Báo cáo kiểm toán vòng GitHub Pages + Google Drive

Ngày kiểm toán: 14/08/2026  
Nguồn gần nhất dùng để nâng cấp: `index.html` 3.0.0-rc.1 trong gói bàn giao trước  
SHA-256 nguồn baseline: `4dd0372505925d0bcdbcb852156e4bfbfaca3bd862173b4ccd28350086cbb554`

## 1. Hiện trạng baseline

| Hạng mục | Baseline quan sát |
|---|---|
| Ứng dụng | Trợ lý Tổng phụ trách Đội THCS 3.0.0-rc.1 |
| Cơ sở dữ liệu | `TPT_DOI_THCS_DB`, schema 8, 61 object store |
| Nghiệp vụ | 16 phân hệ, backup/snapshot/migration/năm học/báo cáo đã có |
| PWA | Đường dẫn tương đối, service worker RC1 |
| Đồng bộ cloud | Provider/hợp đồng chưa phải Google Drive appDataFolder vận hành thật |
| Khóa | License/dùng thử phía client còn mâu thuẫn yêu cầu chỉ dùng `admin@` |
| Cấu hình giáo viên | Core và định danh trường/Gmail/OAuth chưa tách đủ để nhân bản một web/một giáo viên |
| Mobile | Có responsive cơ bản nhưng một số hành động/ngữ cảnh bị ẩn hoặc khó tiếp cận |

## 2. Phát hiện và cách xử lý

| ID | Phát hiện | Mức ban đầu | Hiệu chỉnh | Bằng chứng tự động |
|---|---|---:|---|---|
| A-01 | Chưa có sync Drive thật | Critical theo phạm vi mới | GIS token model + Drive REST `appDataFolder` | `test:sync` |
| A-02 | License/dùng thử chồng lớp khóa | High | Gỡ đường chạy license, khóa phiên `admin@` memory-only | static/core/features |
| A-03 | Lưu local và outbox chưa nguyên tử | Critical | Cùng IDB transaction, báo lưu sau complete | core/features/sync |
| A-04 | Retry có thể nhân đôi sau mất phản hồi | High | Batch UUID + tên tệp ổn định + tìm tệp trước tạo lại | sync lost-response |
| A-05 | Hai thiết bị có thể ghi đè cùng bản ghi | Critical | Revision, pull-before-push, conflict store/UI | sync concurrent-edit |
| A-06 | Thiết bị có thể đọc journal do chính mình vừa đẩy như remote | High | Marker + operation/device ID để bỏ qua bản tự xác nhận | regression sync |
| A-07 | `bulkPut` không đọc revision hiện hữu | High | Đọc từng ID trong cùng transaction, rollback cả lô | features bulk rollback |
| A-08 | Manifest có cửa sổ đua | High | Base revision/write ID, đọc lại, xác nhận và retry hòa giải | sync concurrent manifest |
| A-09 | Sai Gmail cần chặn trước khi ghi | Critical | `about.get`, permission ID/email hash, trạng thái chặn bền | sync wrong-account |
| A-10 | Token/khóa có nguy cơ tồn tại qua reload | High | Chỉ RAM; quét storage và source | static/core/sync |
| A-11 | Attachment thiếu hợp đồng Drive đầy đủ | High | UUID/checksum, multipart/resumable, download verify, hủy/tiến trình | sync attachment tests |
| A-12 | appData bị xóa có thể khiến cờ ghép cũ sai | High | Xóa cờ ghép, giữ local, yêu cầu quyết định mới | sync deleted-appdata |
| A-13 | Rate limit 403 chưa được retry | Medium | Nhận diện reason, backoff/jitter, giữ outbox | sync rate-limit |
| A-14 | Mobile ẩn hành động/ngữ cảnh | High | Điều hướng/ngữ cảnh mobile, touch/safe-area/modal | responsive static |
| A-15 | Cấu hình dính core | High | `app-config.js`, example và generator không gửi Gmail | static/setup parse |
| A-16 | PWA cần tránh cache Google/token/config cũ | High | Chỉ same-origin, config network-first, Drive/GIS ngoài SW | static/SW audit |

## 3. Định danh sau nâng cấp

- Phiên bản: `3.1.0-rc.1`.
- Schema: `9`.
- APP ID mặc định tương thích: `vn.giaoducso40.tpt.thcs.standard`.
- School profile mặc định: `thcs-local-profile-001`.
- Database mặc định giữ: `TPT_DOI_THCS_DB`.
- Bản theo giáo viên phải sinh định danh ổn định một lần bằng `SETUP_CONFIG.html`.
- Nguồn v1 đính kèm không bị sửa; SHA-256 vẫn là `b64798b6e53e2d5dbafc2bac972f3fb2c4a4a2eeda1e231d52152eefb65b8217`.

## 4. Những gì báo cáo này không chứng minh

Kiểm toán trong container không có OAuth Client ID/Gmail thật, GitHub repository thật, Chrome/Edge/Safari/Firefox executable hoặc hai thiết bị vật lý. Vì vậy chưa thể đánh dấu đạt cho OAuth/Drive thật, Pages/PWA thật, ảnh viewport/zoom thật và soak nhiều giờ. Các mục này nằm trong `LOI_CON_LAI.md` và là cổng nghiệm thu trước khi đổi RC thành bản chính thức.
