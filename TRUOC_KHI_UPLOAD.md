# DỪNG LẠI — phải tạo cấu hình giáo viên trước khi upload

`app-config.js` đi kèm ZIP đang là cấu hình mẫu an toàn và **chưa bật Google Drive** vì chưa có OAuth Client ID/Gmail thật của giáo viên.

Trước khi đưa lên GitHub:

1. Mở `SETUP_CONFIG.html`.
2. Nhập đúng trường, repository, OAuth Client ID và Gmail duy nhất.
3. Tải `app-config.js` mới.
4. Thay `app-config.js` mẫu trong thư mục này.
5. Mở tệp mới và xác nhận `GOOGLE_CLIENT_ID` không trống, email chỉ còn SHA-256 64 ký tự.
6. Không điền client secret, token, mật khẩu Gmail hoặc dữ liệu thật.
7. Sau khi thay config, phải tính lại `CHECKSUMS.txt`/`RELEASE_MANIFEST.json` nếu dùng chúng để đối chiếu phát hành.

Không thể xác nhận “một web đúng một giáo viên” chỉ với cấu hình mẫu. Xem `README_CAI_DAT_GITHUB.md` và `HUONG_DAN_GOOGLE_CLOUD_DRIVE.md`.
