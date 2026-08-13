# Hướng dẫn sử dụng dành cho giáo viên

## Mở ứng dụng mỗi ngày

1. Mở đúng URL GitHub Pages hoặc biểu tượng PWA đã cài.
2. Nhập `admin@`.
3. Nếu Google hỏi, chọn đúng Gmail đã cấu hình.
4. Nhìn hai trạng thái ở đầu trang: lưu trên thiết bị và đồng bộ Drive.
5. Chọn năm học, học kỳ, tuần và cơ sở đúng trước khi nhập.

Ứng dụng tự khóa sau 10 phút không thao tác theo mặc định. Có thể chọn 5/10/15/30 phút trong Thiết lập. Khi cần rời máy, chọn **Khóa ứng dụng**.

## Nguyên tắc không mất dữ liệu

- Thông báo **Đã lưu trên thiết bị** nghĩa là dữ liệu đã nằm trong IndexedDB.
- Chỉ **Đã đồng bộ lúc…** mới xác nhận lượt Drive hoàn tất.
- Khi offline, cứ tiếp tục làm việc; không xóa dữ liệu trình duyệt.
- Trước khi đóng tab ở cuối buổi, nên chờ outbox về 0 hoặc bấm **Đồng bộ ngay**.
- Luôn giữ backup bên ngoài; Drive không thay thế backup.
- Không mở hai tab để cùng sửa. Tab thứ hai sẽ chuyển sang chỉ đọc.

## Các phân hệ chính

1. **Tổng quan/Hôm nay:** việc đến hạn, hoạt động, cảnh báo và thao tác nhanh.
2. **Kế hoạch/Công việc/Lịch:** lập kế hoạch, checklist, việc lặp và lịch hoạt động.
3. **Thi đua lớp:** chọn đúng tuần/bộ tiêu chí, nhập điểm, kiểm tra đủ dữ liệu rồi khóa bảng.
4. **Hoạt động Đội/Tổ chức Liên đội/Rèn luyện – phong trào:** theo dõi hoạt động, tổ chức, chuyên hiệu và phong trào.
5. **Khen thưởng:** quản lý đề nghị, trạng thái xét duyệt và quyết định.
6. **Hồ sơ – minh chứng:** metadata, liên kết nghiệp vụ và tệp đính kèm.
7. **Thiết bị Đội:** kiểm kê, mượn–trả, tình trạng và nơi lưu.
8. **Báo cáo:** tạo bản nháp, kiểm tra nguồn rồi chốt. Bản đã chốt không tự đổi theo dữ liệu mới.
9. **Trợ lý tổng hợp:** truy vấn/tổng hợp nội bộ; kiểm tra lại dữ kiện trước khi dùng chính thức.
10. **Sao lưu – đồng bộ:** Drive, xung đột, backup, restore và snapshot.
11. **Thiết lập:** thông tin trường, lớp, tiêu chí, ngưỡng dung lượng và vòng đời năm học.

## Dùng trên điện thoại

- Điều hướng dưới và nút ngữ cảnh giúp mở đủ chức năng; không có chức năng chỉ dành cho desktop.
- Bảng rộng cuộn bên trong vùng bảng, không kéo ngang toàn trang.
- Form chuyển thành một cột; nút Lưu/Hủy nằm ở phần cố định của modal.
- Khi bàn phím che nội dung, cuộn bên trong modal; không reload trang giữa lúc nhập.
- Có thể cài PWA vào màn hình chính để mở như ứng dụng.

## Thi đua và báo cáo

1. Chọn đúng năm/tuần/cơ sở và bộ tiêu chí.
2. Nhập, rà giá trị trống và trạng thái hoàn thành.
3. Đối chiếu xếp hạng/đồng hạng trước khi khóa.
4. Khi mở khóa bảng hoặc năm cũ, nhập lý do đủ rõ; thao tác được audit.
5. Bản báo cáo chốt chứa version, checksum, bộ lọc và số bản ghi nguồn. Muốn thay đổi thì tạo phiên bản mới.

## Đóng năm/mở năm mới

1. Xử lý hết xung đột và outbox Drive.
2. Kiểm tra checklist, bảng điểm và báo cáo.
3. Tạo backup đầy đủ.
4. Chạy wizard đóng năm; ứng dụng tạo snapshot và gói năm trước khi chuyển chỉ đọc.
5. Khi tạo năm mới, chỉ sao chép cấu hình/danh mục được chọn; không mang điểm/sự việc cũ.
6. Muốn sửa năm đã đóng phải mở quyền sửa theo phiên và ghi lý do.

## Khi gặp lỗi

- **Sai mật khẩu:** ô bị xóa và tăng thời gian chờ sau nhiều lần sai.
- **Cần kết nối lại:** bấm kết nối Drive và chọn đúng Gmail.
- **Hết dung lượng thiết bị:** xuất backup, xóa tệp không cần theo quy trình; không xóa dữ liệu site tùy tiện.
- **Có xung đột:** mở chi tiết và chọn local/Drive/hợp nhất từng trường.
- **Backup hỏng/sai mật khẩu:** ứng dụng từ chối trước khi thay dữ liệu; chọn đúng tệp hoặc bản backup khác.
- **Bản cập nhật:** đồng bộ + backup trước; không đổi định danh cấu hình.

Dòng phát hành trong ứng dụng: Phát triển bởi: Thầy Hiếu (Giáo dục số 4.0) - Zalo: 0812806887
