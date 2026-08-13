# Changelog

## 3.1.0-rc.1 — 2026-08-14

### GitHub Pages và cấu hình riêng

- Tách `app-config.js` khỏi core; thêm `app-config.example.js` và `SETUP_CONFIG.html` chạy cục bộ.
- Chuẩn hóa đường dẫn tương đối/hash routing, history back/forward, PWA subpath, `.nojekyll`, trang 404/offline và cache version mới.
- Gói mặc định không chứa Gmail rõ, client secret, token hoặc dữ liệu nghiệp vụ.

### Khóa phiên và Google Identity

- Xóa license/dùng thử cũ; chỉ giữ lớp khóa `admin@` trong RAM, tự khóa 5/10/15/30 phút và khóa khi nền quá hạn.
- Khởi tạo Google Identity Services từ thao tác mở ứng dụng; access token chỉ ở RAM.
- Xác minh Drive `about.get(fields=user)`, ưu tiên permission ID hoặc SHA-256 Gmail; sai Gmail bị chặn nhưng local vẫn mở.

### Local-first và đồng bộ Drive

- Nâng schema 8→9; thêm device ID, sync status và index operation/batch/Drive.
- Ghi bản ghi + outbox trong cùng IndexedDB transaction; trạng thái local/Drive tách biệt.
- Đồng bộ `appDataFolder` dạng manifest + snapshot + lô operation bất biến + attachment UUID/checksum.
- Retry idempotent sau mất phản hồi; backoff/jitter cho rate limit/5xx; không retry vô hạn.
- Kéo trước/đẩy sau, tombstone, xung đột nhìn thấy và lựa chọn local/Drive/hợp nhất từng trường.
- Bỏ qua journal do chính thiết bị đã xác nhận để không tự tạo xung đột.
- Ghi hàng loạt đọc revision hiện hữu trong cùng transaction và rollback toàn lô khi stale.
- Manifest có write ID/base revision, đọc lại/xác nhận và hòa giải khi hai thiết bị ghi gần nhau.
- Attachment nhỏ dùng multipart; tệp lớn dùng resumable upload, tiến trình/hủy và kiểm tra checksum/kích thước.
- Khi appDataFolder/manifest bị xóa, bỏ cờ ghép và yêu cầu người dùng ghép lại thay vì tự ghi đè.

### Giao diện và vận hành

- Mobile có đủ đường truy cập chức năng, ngữ cảnh riêng, touch target 44 px, safe area, input 16 px và modal sticky actions.
- Desktop dùng chiều rộng linh hoạt; bảng cuộn trong vùng chứa; không ẩn nhóm hành động chính trên mobile.
- Thêm màn hình ghép Drive, tổng quan outbox/xung đột, trạng thái thật và trung tâm xử lý xung đột.
- Ngưỡng dung lượng 70/85/95% có cấu hình; hash Blob lớn qua Worker khi hỗ trợ.

### Kiểm thử

- Bổ sung mô phỏng hai thiết bị/REST Drive, lost response, xung đột, quota/rate limit/401, popup, sai Gmail, appData bị xóa và attachment >5 MiB.
- Bổ sung kiểm tra breakpoint 320–1920, zoom mục tiêu, safe area, modal và touch target ở mức cấu trúc CSS/DOM.
- Tổng 103 kiểm tra tự động trong bảy nhóm; xem `TEST_REPORT.md` và giới hạn bằng chứng trong `LOI_CON_LAI.md`.

## 3.0.0-rc.1 — 2026-08-14

### Dữ liệu và độ bền

- Giữ database `TPT_DOI_THCS_DB`, nâng schema 7→8 theo hướng chỉ thêm store/index.
- Thêm App ID, school profile ID và hợp đồng bản ghi; migration giữ nguyên ID, bổ sung metadata thiếu.
- Gộp ghi nghiệp vụ/audit/journal trong transaction; chỉ báo lưu sau `oncomplete`.
- Thêm optimistic revision conflict, lỗi quota rõ ràng, form draft và khóa một tab ghi.
- Thêm journal `started/completed/failed/cancelled/interrupted` cho thao tác lớn.

### Snapshot, backup và restore

- Thêm snapshot nội bộ không nhân Blob, checksum và retention 7 ngày/4 tuần/12 tháng.
- Chuẩn hóa `TPT-BACKUP-3`: nhanh, đầy đủ, gói năm học; manifest, source checksum và metadata tệp.
- Mã hóa AES-GCM xác nhận mật khẩu hai lần; xử lý theo chunk, tiến trình và hủy.
- Thêm thư mục File System Access và sao lưu lúc ứng dụng đang mở khi quyền còn hiệu lực.
- Restore kiểm App ID/profile/schema/checksum/Blob, preview xung đột, staging, merge theo revision hoặc replace transaction.

### Năm học và báo cáo

- Wizard tạo năm có chọn lọc; 2 học kỳ/40 tuần; không sao chép dữ liệu kỳ cũ sai phạm vi.
- Đóng năm có checklist, snapshot, báo cáo tổng kết chốt, gói năm và chế độ chỉ đọc.
- Mở sửa năm cũ theo phiên, bắt buộc lý do và audit.
- Báo cáo có draft/finalized, version, nơi nhận, trạng thái gửi, filter/config/source count/checksum; gói báo cáo chốt.

### PWA, giao diện và kiến trúc

- Xóa 6 khai báo hàm top-level trùng.
- Nội dung dùng toàn chiều rộng; footer chữ thuần đúng yêu cầu; thêm reduced motion và trạng thái UI.
- PWA dùng `./index.html`, cache RC1, fallback 404/offline và cập nhật có kiểm tra draft.
- Thêm BrowserPlatformAdapter, data contract và stub/hợp đồng desktop trung thực.
- Không còn URL ngoài, CDN, analytics hoặc telemetry trong runtime.

### Kiểm thử

- 62 kiểm tra tự động đạt; có migration, fault injection, Blob round-trip, tính điểm/xếp hạng đồng hạng, báo cáo bất biến, năm học, đa tab và tải lớn.
- Chưa thực hiện browser/PWA/GitHub/EXE thật; xem `LOI_CON_LAI.md`.
