# Hướng dẫn sao lưu, phục hồi và cập nhật

Google Drive là kênh đồng bộ, không phải bản sao lưu duy nhất. Duy trì ít nhất hai bản backup ngoài ở hai nơi khác nhau.

## 1. Ba loại backup

| Loại | Nội dung | Khi dùng |
|---|---|---|
| Sao lưu nhanh | Dữ liệu nghiệp vụ, không gồm nội dung Blob | Hằng ngày/trước thao tác nhỏ |
| Sao lưu đầy đủ | Dữ liệu và toàn bộ tệp đính kèm | Định kỳ, trước cập nhật/chuyển máy |
| Gói năm học | Dữ liệu/tệp thuộc năm đang chọn | Trước và sau khi đóng năm |

Mỗi gói có manifest, App ID, school profile, schema, số bản ghi và checksum. Có thể mã hóa AES-GCM bằng mật khẩu tối thiểu 8 ký tự. Mất mật khẩu mã hóa thì không thể phục hồi.

## 2. Lịch khuyến nghị

- Mỗi ngày làm việc: backup nhanh.
- Mỗi tuần hoặc sau đợt nhập lớn: backup đầy đủ.
- Trước cập nhật, restore, nhập CSV, đổi OAuth hoặc đóng năm: backup đầy đủ.
- Cuối học kỳ/năm: gói năm + backup đầy đủ, kiểm tra mở được.

Không lưu backup chứa dữ liệu thật trong repository GitHub.

## 3. Tạo backup

1. Mở **Sao lưu – đồng bộ**.
2. Chờ thao tác lưu local hoàn tất; nếu có thể, đồng bộ hết outbox.
3. Chọn loại backup và đích tải xuống/thư mục đã cấp quyền.
4. Nếu mã hóa, nhập và xác nhận mật khẩu.
5. Bắt đầu; theo dõi tiến trình. Có thể hủy, gói chưa hoàn tất không được ghi nhận là thành công.
6. Chờ ứng dụng báo checksum và hoàn tất.
7. Sao chép tệp sang ổ/Drive dự phòng do giáo viên quản lý.
8. Định kỳ thử phục hồi trên hồ sơ thử, không chờ đến khi có sự cố.

## 4. Phục hồi an toàn

1. Không xóa dữ liệu hiện tại.
2. Tạo backup đầy đủ và snapshot trước phục hồi.
3. Chọn đúng tệp backup; nhập mật khẩu nếu có.
4. Ứng dụng kiểm định dạng, App ID, school profile, schema, checksum và tệp thiếu.
5. Xem màn hình preview số bản ghi mới/trùng/xung đột.
6. Chọn:
   - **Hợp nhất:** giữ dữ liệu mới hơn theo revision và yêu cầu xử lý xung đột.
   - **Thay thế sạch:** thay toàn bộ phạm vi trong một transaction; chỉ dùng khi chắc chắn.
7. Không đóng tab giữa quá trình.
8. Sau hoàn tất, đối chiếu số bản ghi, tệp, một bảng điểm và một báo cáo chốt.
9. Kết nối Drive và chọn ghép/tải lên/hợp nhất có chủ đích; không để dữ liệu cũ trên Drive ghi đè âm thầm.
10. Chạy lại backup đầy đủ sau khi xác nhận.

Restore thay thế tạo tombstone/outbox cho dữ liệu bị loại để thiết bị khác nhận đúng; thao tác từ snapshot Drive dùng chế độ nội bộ để tránh tạo lại toàn bộ outbox.

## 5. Chuyển sang thiết bị mới

Ưu tiên:

1. Đồng bộ thiết bị cũ, xuất backup đầy đủ.
2. Mở cùng URL trên thiết bị mới và chọn đúng Gmail.
3. Tải snapshot Drive có xác minh checksum.
4. Đối chiếu dữ liệu/tệp.
5. Nếu Drive không đủ, phục hồi từ backup đầy đủ.
6. Chỉ xóa dữ liệu thiết bị cũ sau thời gian nghiệm thu và còn ít nhất hai backup.

## 6. Cập nhật mã trên GitHub

1. Outbox = 0 và không có xung đột.
2. Tạo backup đầy đủ.
3. Lưu `app-config.js` hiện tại.
4. Upload mã mới nhưng giữ nguyên App ID, school ID, namespace và DB name.
5. Không xóa IndexedDB/dữ liệu site để “làm mới cache”.
6. Khi service worker báo bản mới, chỉ cập nhật khi không có form nháp/outbox.
7. Đối chiếu migration, dữ liệu và Drive sau reload.

## 7. Sự cố và nguyên tắc dừng

- Checksum sai, thiếu Blob, sai mật khẩu hoặc schema mới hơn: dừng; không cố nhập.
- Quota thiết bị: giải phóng dung lượng có kiểm soát sau khi có backup.
- Drive trống/bị xóa: không tự khởi tạo đè; xác định nguồn đúng rồi ghép lại.
- Sai App ID/school profile/namespace: coi là nhầm sản phẩm hoặc trường; không tiếp tục.
- Tệp backup Unicode/tên dài: không đổi đuôi hoặc mở/lưu lại bằng trình soạn thảo.
