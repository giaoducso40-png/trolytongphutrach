# Cài đặt Trợ lý Tổng phụ trách Đội THCS lên GitHub Pages

Phiên bản: **3.1.0-rc.1** · Schema: **9** · Ngày build: **14/08/2026**

Gói này là bản phát hành ứng viên dùng một repository GitHub Pages cho một giáo viên. GitHub chỉ phát mã chương trình; dữ liệu nghiệp vụ được lưu trước hết trên thiết bị và chỉ đồng bộ riêng vào `appDataFolder` của đúng Gmail đã cấp quyền.

> `admin@` chỉ là khóa phiên để tránh thao tác nhầm trên thiết bị. Khóa này không phải cơ chế bảo mật tuyệt đối. Google OAuth mới bảo vệ quyền truy cập dữ liệu Drive.

## 1. Chuẩn bị

Cần có:

1. Tài khoản GitHub của giáo viên.
2. Gmail duy nhất dùng cho web này.
3. OAuth Client ID loại **Web application** đã bật Google Drive API.
4. Tên repository không dấu, ví dụ `tro-ly-tpt-doi-thcs`.
5. Bản sao lưu dữ liệu cũ nếu đang nâng cấp.

Không đưa client secret, token, mật khẩu Gmail, backup, tên học sinh hoặc hồ sơ thật lên GitHub.

## 2. Tạo `app-config.js` riêng

1. Giải nén `UPLOAD_TO_GITHUB.zip` vào một thư mục mới.
2. Mở `SETUP_CONFIG.html` bằng Chrome hoặc Edge.
3. Nhập tên trường, mã trường, tên repository, OAuth Client ID và Gmail giáo viên.
4. Chọn **Xem trước cấu hình**.
5. Kiểm tra không có Gmail rõ trong phần xem trước.
6. Chọn **Tải app-config.js**.
7. Dùng tệp vừa tải để thay tệp `app-config.js` mẫu trong thư mục giải nén.
8. Cất một bản `app-config.js` ở nơi an toàn để dùng cho những lần cập nhật sau.

Không đổi `APP_ID`, `SCHOOL_PROFILE_ID`, `APP_NAMESPACE` hoặc `DB_NAME` giữa các bản cập nhật nếu chưa có kế hoạch migration. Đổi các định danh này làm trình duyệt nhìn dữ liệu như một ứng dụng/trường khác.

## 3. Tạo repository và tải mã

1. Đăng nhập GitHub của giáo viên.
2. Tạo repository mới, tên trùng với tên đã nhập trong `SETUP_CONFIG.html`.
3. Có thể chọn Public. Không thêm dữ liệu thật vào repository công khai.
4. Mở repository, chọn **Add file → Upload files**.
5. Kéo toàn bộ tệp và thư mục **bên trong** thư mục đã giải nén vào GitHub.
6. Không tải nguyên `UPLOAD_TO_GITHUB.zip`.
7. Kiểm tra `index.html`, `app-config.js`, `sw.js`, `manifest.webmanifest` và thư mục `assets` nằm ngay ở gốc repository.
8. Commit các tệp vào nhánh `main`.

## 4. Bật GitHub Pages

1. Vào **Settings → Pages**.
2. Ở **Build and deployment**, chọn **Deploy from a branch**.
3. Chọn nhánh **main** và thư mục **/(root)**.
4. Chọn **Save** và chờ GitHub hiển thị URL.
5. URL thường có dạng `https://USERNAME.github.io/REPOSITORY/`.

Ứng dụng dùng đường dẫn tương đối và hash routing, nên chạy được dưới repository subpath. Nếu đổi username GitHub, phải cập nhật authorized origin trong Google Cloud. Nếu đổi tên repository, phải tạo lại/đối chiếu cấu hình và kiểm tra PWA.

## 5. Kiểm tra trước khi dùng dữ liệu thật

Thực hiện theo đúng thứ tự:

1. Mở URL ở cửa sổ ẩn danh; xác nhận hiện lớp khóa.
2. Nhập `admin@` và nhấn Enter.
3. Chọn đúng Gmail, cấp duy nhất quyền `drive.appdata`.
4. Kiểm tra tên/Gmail hiển thị trong **Sao lưu – đồng bộ**.
5. Ở thiết bị đầu tiên, chọn **Khởi tạo Drive từ dữ liệu thiết bị** chỉ khi Drive của ứng dụng chưa có dữ liệu.
6. Tạo một công việc thử và chờ trạng thái **Đã đồng bộ lúc…**.
7. Reload; xác nhận lớp khóa hiện lại và dữ liệu còn nguyên.
8. Mở cùng URL trên điện thoại, chọn cùng Gmail, chọn tải/ghép dữ liệu có chủ đích.
9. Sửa bản ghi thử trên điện thoại; kiểm tra máy tính nhận lại.
10. Sau một lần tải online, thử mở PWA khi offline; xác nhận vẫn mở lớp khóa và dùng dữ liệu local.
11. Kiểm tra không có request 404 trong Developer Tools nếu người cài có thể thực hiện.
12. Xuất một bản sao lưu đầy đủ trước khi nhập dữ liệu thật.

## 6. Cài PWA

- Android/Chrome: menu trình duyệt → **Cài đặt ứng dụng** hoặc **Thêm vào màn hình chính**.
- Máy tính Chrome/Edge: chọn biểu tượng cài đặt ở thanh địa chỉ.
- iPhone/iPad Safari: **Chia sẻ → Thêm vào MH chính**.

PWA và tab web cùng một URL dùng chung IndexedDB theo origin. Xóa dữ liệu trang, đổi origin hoặc dùng chế độ riêng tư có thể làm mất bản local; luôn giữ backup ngoài.

## 7. Cập nhật phiên bản

1. Chờ đồng bộ hết outbox và xử lý hết xung đột.
2. Xuất backup đầy đủ.
3. Giữ lại `app-config.js` đã cấu hình.
4. Thay các tệp chương trình bằng bản mới nhưng không thay các định danh ổn định.
5. Upload vào đúng repository/nhánh cũ.
6. Chờ Pages cập nhật, mở URL và chọn cập nhật khi ứng dụng báo an toàn.
7. Đối chiếu dữ liệu local, trạng thái Drive và một báo cáo đã chốt.

## 8. Xử lý nhanh

| Hiện tượng | Cách xử lý |
|---|---|
| GitHub Pages 404 | Kiểm tra Pages dùng `main` + `/(root)` và `index.html` ở gốc. |
| Google báo origin không hợp lệ | Authorized JavaScript origin phải là `https://USERNAME.github.io`, không kèm repository. |
| Chưa cấu hình Google Drive | Tạo lại `app-config.js` bằng `SETUP_CONFIG.html`; không sửa tay nếu không chắc. |
| Sai Gmail | Chọn **Đổi tài khoản** và dùng đúng Gmail; ứng dụng không tạo kho Drive khi xác minh sai. |
| Cần kết nối lại | Bấm **Kết nối lại Google Drive**; token chỉ tồn tại trong bộ nhớ. |
| Đang offline | Tiếp tục làm việc; thay đổi nằm trong outbox và sẽ đồng bộ khi có mạng/token. |
| PWA vẫn là bản cũ | Chờ GitHub Pages hoàn tất, đóng các tab cũ, mở lại và dùng nút cập nhật của ứng dụng. |

Xem thêm `HUONG_DAN_GOOGLE_CLOUD_DRIVE.md`, `HUONG_DAN_SAO_LUU_PHUC_HOI.md` và `LOI_CON_LAI.md` trước khi nghiệm thu chính thức.
