# Hướng dẫn Google Cloud và đồng bộ Google Drive

Ứng dụng dùng Google Identity Services và chỉ yêu cầu scope:

`https://www.googleapis.com/auth/drive.appdata`

Scope này cho phép ứng dụng quản lý dữ liệu riêng trong `appDataFolder`. Dữ liệu không nằm trong repository GitHub và thường không hiện như tệp thông thường trong My Drive.

## 1. Chọn mô hình quản lý OAuth

### Mô hình A — một dự án do người phát hành quản lý

Phù hợp khi cài nhiều giáo viên. Người quản lý OAuth thêm origin GitHub của từng giáo viên. Mỗi Gmail vẫn có `appDataFolder` riêng.

### Mô hình B — mỗi giáo viên tự sở hữu dự án

Giáo viên/trường tạo một Google Cloud project và OAuth Client ID riêng. Cài đặt lâu hơn nhưng giảm phụ thuộc người phát hành. Phải cất Client ID và `app-config.js` để cập nhật sau này.

## 2. Tạo OAuth Client ID

1. Mở Google Cloud Console và tạo/chọn project.
2. Bật **Google Drive API**.
3. Cấu hình **OAuth consent screen**.
4. Khai báo đúng thông tin ứng dụng và email hỗ trợ.
5. Chỉ thêm scope `drive.appdata`; không xin quyền toàn bộ Drive.
6. Nếu ứng dụng đang ở chế độ Testing, thêm Gmail giáo viên vào **Test users**.
7. Vào **Credentials → Create credentials → OAuth client ID**.
8. Chọn loại **Web application**.
9. Trong **Authorized JavaScript origins**, thêm `https://USERNAME.github.io`.
10. Không thêm repository path vào origin; không thêm dấu `/` cuối nếu giao diện không yêu cầu.
11. Sao chép Client ID dạng `....apps.googleusercontent.com`.

Không tạo hoặc đưa client secret vào web. OAuth Client ID phía trình duyệt là định danh công khai, còn access token chỉ được giữ trong RAM của phiên.

## 3. Sinh cấu hình

1. Mở `SETUP_CONFIG.html` trong gói.
2. Nhập Client ID và đúng Gmail giáo viên.
3. Công cụ băm Gmail thành SHA-256 ngay trong trình duyệt; không gửi Gmail đi nơi khác.
4. Tải `app-config.js` và thay tệp mẫu.
5. Upload tệp này cùng mã ứng dụng lên GitHub.

`EXPECTED_GOOGLE_EMAIL_SHA256` giúp ngăn chọn nhầm Gmail ở lần ghép đầu. Sau khi ứng dụng đọc được Drive `permissionId`, có thể dùng `EXPECTED_DRIVE_PERMISSION_ID` ở một vòng cấu hình được kiểm soát; không đoán giá trị này.

## 4. Thiết bị đầu tiên

1. Mở đúng URL GitHub Pages.
2. Nhập `admin@`.
3. Chọn đúng Gmail và chấp thuận quyền `drive.appdata`.
4. Mở **Sao lưu – đồng bộ**, đối chiếu tài khoản hiển thị.
5. Ứng dụng đánh giá cả dữ liệu local và kho Drive trước khi cho ghép.
6. Nếu Drive trống và thiết bị có dữ liệu đúng: chọn khởi tạo/tải dữ liệu thiết bị lên Drive.
7. Nếu Drive đã có dữ liệu: chọn tải về hoặc hợp nhất; không chọn khởi tạo đè.
8. Trước ghép, ứng dụng tải một backup nhanh và tạo snapshot local bảo vệ.
9. Tạo một bản ghi thử, bấm **Đồng bộ ngay** và chờ xác nhận.

## 5. Điện thoại hoặc máy tính thứ hai

1. Mở cùng URL, không tạo URL/repository khác.
2. Nhập `admin@` và chọn cùng Gmail.
3. Chọn tải dữ liệu Drive về thiết bị mới hoặc hợp nhất nếu thiết bị đã có dữ liệu.
4. Không dùng **khởi tạo Drive** khi Drive đã có dữ liệu.
5. Kiểm tra bản ghi thử từ thiết bị đầu.
6. Tạo/sửa một bản ghi trên thiết bị thứ hai và kiểm tra chiều ngược lại.

## 6. Ý nghĩa trạng thái

| Trạng thái | Ý nghĩa/việc cần làm |
|---|---|
| Đã lưu trên thiết bị | IndexedDB đã commit; Drive chưa chắc đã nhận. |
| Ngoại tuyến – thay đổi đang chờ | Có thể tiếp tục làm việc; đừng xóa dữ liệu site. |
| Chờ đồng bộ: N thay đổi | Outbox còn dữ liệu. Có mạng và token thì bấm đồng bộ. |
| Đang đồng bộ/Đang tải tệp N% | Chờ hoặc chọn hủy; hủy không xóa local/outbox. |
| Đã đồng bộ lúc… | Lượt đồng bộ đã được đọc/ghi/xác nhận. |
| Có xung đột | Mở màn hình xung đột, không ghi đè tùy tiện. |
| Cần kết nối lại | Token hết hạn/không còn trong RAM; bấm kết nối lại. |
| Sai Gmail/đồng bộ bị chặn | Đổi sang tài khoản được cấu hình. |
| Drive hết dung lượng/giới hạn tần suất | Local/outbox vẫn giữ; giải phóng dung lượng hoặc thử lại sau. |

## 7. Cấu trúc dữ liệu Drive

Ứng dụng dùng các tệp phẳng có namespace:

- `__manifest.json`: định danh ứng dụng/trường/schema, snapshot mới nhất, revision và thiết bị đã biết.
- `__snapshot__...json`: ảnh dữ liệu đã băm checksum.
- `__ops__...json`: lô thao tác bất biến, có UUID để retry không tạo trùng.
- `__attachment__...bin`: tệp đính kèm theo UUID/checksum; tệp lớn dùng resumable upload.

Ứng dụng kéo nhật ký từ xa trước, phát hiện xung đột, rồi mới đẩy outbox. Xóa nghiệp vụ truyền bằng tombstone; không xóa vật lý âm thầm.

## 8. Tình huống đặc biệt

- **Popup bị chặn:** cho phép popup cho URL Pages rồi bấm kết nối lại. Phần local vẫn hoạt động.
- **Thu hồi quyền:** mở ứng dụng và cấp lại quyền bằng đúng Gmail. Không xóa local trước khi backup.
- **Kho appDataFolder bị trống/xóa:** ứng dụng bỏ cờ ghép và yêu cầu ghép lại; không tự ghi đè từ local.
- **Đổi username GitHub:** thêm origin mới trong Google Cloud trước khi chuyển.
- **Đổi repository:** kiểm tra URL, cấu hình, manifest và service worker.
- **Đổi OAuth Client ID/project:** có thể làm ứng dụng không thấy vùng appData cũ. Phải đồng bộ, xuất backup đầy đủ và thử trên bản sao trước.
- **Gỡ quyền/xóa dữ liệu ứng dụng Google:** appDataFolder có thể bị xóa. Backup JSON/ZIP bên ngoài là bắt buộc.

## 9. Checklist nghiệm thu Drive thật

- [ ] Origin Google Cloud đúng URL GitHub.
- [ ] Gmail đúng kết nối; Gmail khác bị chặn trước khi tạo tệp.
- [ ] Scope chỉ là `drive.appdata`.
- [ ] Token không xuất hiện trong localStorage/sessionStorage/IndexedDB/URL/log.
- [ ] Laptop → điện thoại và điện thoại → laptop đều nhận dữ liệu.
- [ ] Offline tạo dữ liệu rồi online gửi hết outbox.
- [ ] Hai thiết bị sửa cùng bản ghi tạo xung đột nhìn thấy.
- [ ] Tệp nhỏ và tệp lớn đối chiếu checksum.
- [ ] Thu hồi quyền, hết quota và appData trống không làm mất local.

Đây là checklist phải chạy trên tài khoản thật sau khi có Client ID/Gmail. Báo cáo tự động kèm gói chỉ mô phỏng REST Drive, không thay thế nghiệm thu này.
