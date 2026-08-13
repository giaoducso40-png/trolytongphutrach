# Trợ lý Tổng phụ trách Đội THCS

Ứng dụng web/PWA local-first dành cho một giáo viên, chạy từ GitHub Pages trên máy tính và điện thoại. Dữ liệu được commit vào IndexedDB trước; khi có mạng và đúng Gmail, ứng dụng đồng bộ riêng qua Google Drive `appDataFolder`.

Phiên bản bàn giao: **3.1.0-rc.1** · Schema **9**.

## Bắt đầu

1. Đọc `TRUOC_KHI_UPLOAD.md`.
2. Tạo `app-config.js` riêng bằng `SETUP_CONFIG.html`.
3. Làm theo `README_CAI_DAT_GITHUB.md`.
4. Thiết lập OAuth theo `HUONG_DAN_GOOGLE_CLOUD_DRIVE.md`.
5. Chỉ nhập dữ liệu thật sau khi hoàn tất checklist nghiệm thu hai thiết bị.

Mật khẩu mở phiên là `admin@`. Mật khẩu này chỉ ngăn thao tác nhầm ở giao diện, không thay thế khóa thiết bị hay Google OAuth.

## Tài liệu

- `HUONG_DAN_SU_DUNG_GIAO_VIEN.md`
- `HUONG_DAN_SAO_LUU_PHUC_HOI.md`
- `HUONG_DAN_XU_LY_XUNG_DOT.md`
- `SO_DO_LUU_TRU_DU_LIEU.md`
- `TEST_REPORT.md`
- `LOI_CON_LAI.md`

## Quyền riêng tư

Repository chỉ chứa mã, asset, cấu hình công khai và tài liệu. Không commit client secret, token, mật khẩu Gmail, dữ liệu học sinh, hồ sơ thật hoặc backup. Đồng bộ Drive không thay thế bản sao lưu ngoài.

Phát triển bởi: Thầy Hiếu (Giáo dục số 4.0) - Zalo: 0812806887
