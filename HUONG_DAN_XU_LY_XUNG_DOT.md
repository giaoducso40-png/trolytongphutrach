# Hướng dẫn xử lý xung đột đồng bộ

Xung đột xuất hiện khi cùng một bản ghi được sửa trên hai thiết bị từ cùng revision hoặc khi dữ liệu local/Drive không thể xác định bản nào thay thế an toàn. Ứng dụng không dùng “lần ghi cuối thắng” âm thầm.

## 1. Trước khi xử lý

1. Dừng sửa bản ghi đó trên các thiết bị khác.
2. Tạo backup nhanh hoặc đầy đủ.
3. Mở **Sao lưu – đồng bộ → Xử lý xung đột**.
4. Kiểm tra loại bản ghi, ID, revision, thời gian, thiết bị nguồn và các trường khác nhau.

## 2. Ba lựa chọn

### Giữ bản thiết bị (local)

Dùng khi bản đang nhìn trên thiết bị là bản đã được kiểm tra. Ứng dụng hủy operation cũ của bản ghi, tạo revision mới và đưa quyết định vào outbox.

### Giữ bản Drive

Dùng khi bản Drive là nguồn đúng. Ứng dụng áp bản Drive có dấu vết và không tạo một vòng ghi ngược không cần thiết.

### Hợp nhất từng trường

Dùng khi mỗi bản có phần đúng. Chọn giá trị cho từng trường khác nhau, rà lại trường liên kết/năm/tuần/trạng thái rồi lưu. Kết quả là một revision mới.

Không trộn thủ công điểm, trạng thái khóa, báo cáo chốt hoặc khóa liên kết nếu chưa hiểu tác động nghiệp vụ.

## 3. Sau khi xử lý

1. Bấm **Đồng bộ ngay**.
2. Chờ outbox giảm và trạng thái **Đã đồng bộ lúc…**.
3. Mở thiết bị thứ hai, kết nối lại nếu cần và đồng bộ.
4. Đối chiếu bản ghi, các liên kết và báo cáo liên quan.
5. Nếu xung đột tái xuất hiện, dừng chỉnh sửa và kiểm tra có thiết bị/tab cũ nào chưa nhận quyết định.

## 4. Phòng tránh

- Một bản ghi quan trọng chỉ nên được một thiết bị sửa tại một thời điểm.
- Đồng bộ trước khi chuyển thiết bị và sau khi hoàn tất một đợt nhập.
- Không đóng tab khi đang gửi tệp lớn hoặc còn outbox nếu muốn Drive nhận ngay.
- Không dùng nhiều bản URL/repository/config cho cùng một trường.
- Không đổi đồng hồ hệ thống, App ID, namespace hoặc school profile tùy tiện.
- Xử lý hết xung đột trước khi chốt báo cáo/đóng năm.

## 5. Khi không chắc chọn bản nào

Không chọn ngẫu nhiên. Xuất backup, ghi lại ID xung đột, đối chiếu hồ sơ gốc/biên bản và nhờ người phụ trách nghiệp vụ xác nhận. Dữ liệu local/outbox được giữ trong lúc chờ.
