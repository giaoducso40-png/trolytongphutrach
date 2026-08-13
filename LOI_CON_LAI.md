# Cổng nghiệm thu còn mở

Không có lỗi Blocker/Critical/High **đã tái hiện trong 103 kiểm tra tự động**. Tuy nhiên các mục dưới đây là khoảng trống bằng chứng thực tế, nên phiên bản vẫn mang nhãn `3.1.0-rc.1` và chưa được tuyên bố hoàn thiện chính thức.

| ID | Cổng chưa có bằng chứng | Mức chặn phát hành chính thức | Ảnh hưởng | Cách hoàn tất |
|---|---|---:|---|---|
| RG-01 | Chưa có `GOOGLE_CLIENT_ID`, Gmail và Drive permission ID thật của giáo viên | Blocker | `app-config.js` trong ZIP là mẫu an toàn; Drive mặc định bị vô hiệu hóa | Sinh config bằng `SETUP_CONFIG.html`, kiểm tra hash, origin và đúng Gmail |
| RG-02 | Chưa deploy release candidate lên repository GitHub Pages thật | Blocker | Chưa chứng minh URL/subpath/404/cache/update trên hạ tầng đích | Upload vào staging, bật Pages `main/(root)`, chạy checklist README |
| RG-03 | Chưa chạy OAuth/Drive thật | Blocker | REST Drive hiện được kiểm bằng mô phỏng; chưa chứng minh consent/popup/CORS/quota thật | Chạy hai tài khoản (đúng/sai), thu hồi quyền, appData trống và tệp >5 MiB trên Drive thật |
| RG-04 | Không có browser executable trong môi trường build | Blocker bằng chứng UI/PWA | Viewport 320–1920/zoom hiện mới kiểm CSS/DOM; chưa có screenshot/interaction thật | Chạy Chrome, Edge, Safari và Firefox; lưu ảnh/console/network |
| RG-05 | Chưa có laptop + điện thoại vật lý | Blocker bằng chứng đa thiết bị | Hai thiết bị được mô phỏng bằng hai IndexedDB/browser context | Chạy laptop tạo → điện thoại nhận → sửa ngược → offline/online → conflict |
| RG-06 | Chưa chạy soak ≥8 giờ và đo memory trên phần cứng đích | Medium | Chưa loại trừ leak/timer/listener chậm tích lũy | Dùng DevTools Performance/Memory, thao tác lặp và để ứng dụng mở nhiều giờ |
| RG-07 | Chưa đo dữ liệu thật đã ẩn danh/tổng tệp lớn trên máy giáo viên | Medium | Số đo JSDOM/Node không đại diện tốc độ thiết bị/Wi-Fi | Pilot bằng bản sao đã ẩn danh, ghi startup/search/save/sync/backup/restore/RAM |
| RG-08 | Resumable session URL không được khôi phục sau reload giữa upload | Medium | Local Blob/outbox vẫn còn và lượt sau tải lại từ đầu; không mất dữ liệu nhưng tốn băng thông | Nếu cần tệp rất lớn/Wi-Fi yếu, bổ sung store phiên upload và query trạng thái session |
| RG-09 | Chưa có pixel/keyboard/print QA thủ công mọi modal | Medium | Static checks không phát hiện mọi khác biệt font, bàn phím ảo hoặc driver in | Rà toàn bộ 16 module, modal, A4 và tên Unicode dài trên thiết bị thật |

## Điều kiện đổi từ RC sang bản chính thức

1. Điền config thật và không còn placeholder.
2. RG-02 đến RG-05 đạt, không lỗi console/404/unhandled rejection.
3. Drive thật qua đủ hai chiều, offline, conflict, wrong Gmail, revoked token, quota và attachment checksum.
4. Backup đầy đủ được restore trên hồ sơ thử và đối chiếu số bản ghi/tệp.
5. Không phát hiện Blocker/Critical/High; lỗi Medium/Low mới phải ghi vào bảng này.
6. Tạo lại ZIP/checksum sau mọi thay đổi cuối.

Không dùng câu “100% không lỗi” dựa riêng trên kiểm thử mô phỏng. `TEST_REPORT.md` phân biệt rõ phần đã chạy và phần cần người cài nghiệm thu.
