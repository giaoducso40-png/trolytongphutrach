# Báo cáo kiểm thử — 3.1.0-rc.1

Ngày chạy cổng cuối: **14/08/2026**  
Lệnh: `npm test`  
Kết quả: **103/103 kiểm tra đạt, exit code 0**  
SHA-256 `index.html`: `a6df69245abc9949a40dcef3e548cc2ff1d5c9136dcf94e0184d5bbf842128f4`

## 1. Tổng hợp

| Nhóm | Số kiểm tra | Kết quả | Môi trường |
|---|---:|---|---|
| Static/source/PWA/security | 19 | 19 đạt | Node + phân tích HTML/JS/CSS/source |
| Core/integration | 17 | 17 đạt | JSDOM + fake-indexeddb |
| Migration | 8 | 8 đạt | Dữ liệu schema cũ → schema 9 |
| Feature/fault/backup/year | 18 | 18 đạt | JSDOM + fake-indexeddb |
| Hai thiết bị + Drive | 19 | 19 đạt | Hai browser context + REST Drive giả lập |
| Responsive structure | 15 | 15 đạt | Kiểm tra CSS/DOM tĩnh |
| Load/performance | 7 | 7 đạt | Node + JSDOM + fake-indexeddb |
| **Tổng** | **103** | **103 đạt** | Không có console/runtime error trong fixture |

## 2. Hạng mục đã kiểm

### Mã/PWA/quyền riêng tư

- JavaScript parse; không function/DOM ID trùng; hai nguồn release giống byte.
- Nguồn v1 không đổi: `b64798b6e53e2d5dbafc2bac972f3fb2c4a4a2eeda1e231d52152eefb65b8217`.
- Định danh mặc định ổn định; footer đúng một dòng chữ thuần.
- Không dynamic code execution; runtime chỉ chứa endpoint GIS/Drive được duyệt.
- PWA dùng đường dẫn tương đối/subpath, cập nhật có kiểm tra draft/outbox.
- Service worker chỉ xử lý same-origin, không cache/chặn OAuth hoặc Drive API.
- Access token và trạng thái mở khóa không được persist.

### Core, migration và nghiệp vụ

- Khóa `admin@`, mở ứng dụng, 16 điều hướng, schema 9/61 store.
- Hợp đồng bản ghi, journal, snapshot, ba loại backup, năm học và báo cáo chốt.
- Migration giữ ID task/document, chuẩn hóa metadata, chạy một lần và tạo snapshot bảo vệ.
- Revision conflict, IndexedDB quota, draft, backup hỏng/checksum/Blob/mật khẩu sai.
- Export Blob đầy đủ, replace round-trip, báo cáo chốt bất biến, đóng/mở năm và audit.
- Ghi hàng loạt tăng revision đúng, base/new revision đúng và rollback cả lô khi stale.
- Hai tab: tab thứ hai chỉ đọc.

### Đồng bộ hai thiết bị và fault injection

- Ghép lần đầu tạo manifest/snapshot đúng namespace và scope `drive.appdata`.
- Mất phản hồi sau khi Drive đã lưu: retry idempotent, không tạo file operation trùng.
- Laptop → điện thoại; điện thoại → laptop.
- Hai thiết bị ghi manifest gần nhau được đọc lại/hòa giải.
- Hai thiết bị sửa cùng bản ghi tạo xung đột nhìn thấy; quyết định tạo revision mới.
- Offline giữ outbox; online gửi lại.
- Drive quota giữ local/outbox; rate limit dùng backoff và phục hồi.
- Token 401 xóa token RAM, yêu cầu kết nối lại; popup bị chặn không khóa local.
- Sai Gmail bị chặn trước đồng bộ; appData/manifest bị xóa yêu cầu ghép lại an toàn.
- Tệp nhỏ multipart và tệp >5 MiB resumable được tải hai chiều, đối chiếu checksum/kích thước.
- Mọi request giả lập chỉ đến Drive v3/upload và có Bearer token khi phù hợp.

Mô phỏng tạo 1 manifest, 3 snapshot và 9 lô operation; không có runtime error ở bốn context (laptop, điện thoại, sai Gmail, appData bị xóa).

## 3. Kiểm tra giao diện responsive ở mức cấu trúc

Mục tiêu được đối chiếu: 320×568, 360×800, 375×812, 390×844, 412×915, 768×1024, 1366×768, 1440×900, 1920×1080, zoom 125%/150%.

Đạt các rule: viewport-fit, breakpoint mobile/tablet, desktop fluid, năm học và ngữ cảnh có đường truy cập mobile, safe area, input 16 px, touch target 44 px, hành động không bị CSS ẩn, bảng cuộn trong vùng chứa, modal cuộn/sticky actions, reduced motion và nhãn điều hướng.

Đây **không phải** ảnh chụp hoặc thao tác trình duyệt thật; môi trường build không có browser executable.

## 4. Kết quả tải/hiệu năng đo trong JSDOM

Fixture: 10.000 công việc, thêm 100 lớp, 200 hồ sơ/tệp 512 byte.

| Phép đo | Thời gian |
|---|---:|
| Seed fixture | 1.421,1 ms |
| Render trang công việc 100 dòng/trang | 161,2 ms |
| Render bảng điểm 100 lớp | 106,4 ms |
| Render 200 hồ sơ/tệp | 101,5 ms |
| Mở trung tâm backup trên 10.000+ bản ghi | 317,6 ms |
| Tạo snapshot | 218,4 ms |
| Backup nhanh | 1.557,2 ms |

Các số trên chỉ dùng để phát hiện hồi quy tương đối trong Node/JSDOM; không phải cam kết tốc độ Chrome/điện thoại/Wi-Fi thật. Chưa đo startup/search/import/restore/report/RAM/soak trên phần cứng đích.

## 5. Kết luận cổng chất lượng

- Không phát hiện Blocker/Critical/High trong phạm vi tự động đã chạy.
- Không có test thất bại hoặc console/runtime error trong fixture.
- Không được suy diễn thành “100% không lỗi” hay “đã nghiệm thu sản xuất”.
- Các cổng thực tế còn thiếu: config giáo viên, deploy Pages, OAuth/Drive thật, browser/viewport thật, thiết bị vật lý và soak. Xem `LOI_CON_LAI.md`.
