# Performance

#### 1. Concept.
- Performance tab là công cụ trong devtools chp pép ghi lại và phân tích toàn bộ quá trình render, script execution, network và memory usage của trang web theo thời gian thực.
  - Developer :
    - xác định nguyên nhận trang chậm.
    - tối ưu thời gian và render.
    - debug animation hoặc sự kiện.
  - Pentester :
    - xác định hành vi bất thường của web (script lạ, network call bất thường).
    - phân tích luồng chạy khi thực hiện hành động nhạy cảm ( nhập form, đăng nhập...).
    - tìm điểm chèn mã hoặc hook script.
#### 2. Structure (cấu trúc).
###### 2.1. Toolbar. 
- Record : bắt đầu hoặc dừng ghi hiệu
- Reload While profiling : tự reload trang khi ghi để bắt đầu từ 0.
- Screenshots : chụp ảnh timeline khi trang chạy.
- Memory : bật/tắt đo bộ nhớ.
- CPU throttling : giả lập CPU chậm để test hiệu năng.
###### 2.2. Flame Chart.
- Hiển thị luồng thực thi JS, paint,layout, idle time....
- màu sắc :
  - Vàng → Script execution (JS).
  - Tím → Rendering (Style, Layout).
  - Xanh lá → Painting (Draw UI).
  - Xanh dương → Idle (nhàn rỗi).
###### 2.3. Summary Panel.
- thống kê tổng thời gian dành chp JS, rendering, painting.
- Hiển thị các FPS (khung hình / giây) cho animation/game.
###### 2.4. Bottom-up.
- call tree : Hiển thị cây hàm được gọi theo thứ tự.
- bottom-up : Gom các hàm giống nhau để xem hàm nào tốn nhiều thời gian nhất.ư
#### 3. Techniques using performance (kỹ thuật).
##### 3.1. Developer-Oriented
- Identify Long Tasks: Tìm đoạn JS > 50ms → optimize.
- Detect Layout Thrashing: Quá nhiều layout recalculation → tối ưu CSS/DOM manipulation.
- Analyze Memory Leaks: Bật Memory record → tìm object không được giải phóng.
- Check Animation FPS: Animation < 60 FPS → cần tối ưu.
###### 3.2. Pentester-Oriented
- Detect Suspicious Scripts: Xem trong Flame Chart có script lạ gọi API hoặc DOM manipulation bất thường.
- Timing Attack Analysis: Đo thời gian phản hồi của hành động để suy luận logic (ví dụ: login brute-force có delay khác nhau khi user tồn tại vs không tồn tại).
- Identify Security-Relevant Code Execution: Ghi lại performance khi click “Submit” → tìm đoạn JS xử lý dữ liệu trước khi gửi → hook hoặc sửa.
- Detect Obfuscated Loops: Vòng lặp tính toán dài bất thường → có thể là anti-debug, anti-bot.
#### 4. Practice (thực hành).
###### Tình huống 1 – Developer:
- Bạn có web app load chậm khi mở form.
- Mở Performance → Record → click mở form → Stop.
- Thấy một hàm calculateData() chiếm 400ms.
- Tối ưu thuật toán hoặc debounce để cải thiện UX.
###### Tình huống 2 – Pentester:
- Bạn muốn xem dữ liệu gửi đi khi submit form có bị xử lý client-side không.
- Mở Performance → Record → submit form.
- Dừng lại → trong Flame Chart tìm function preparePayload().
- Mở function trong Sources tab → thấy logic:
```
payload.role = "user";
if (currentUser.isAdmin) payload.role = "admin";
```
- Bạn hook currentUser.isAdmin = true trước khi submit → bypass role check.
###### Tình huống 3 – Malware Detection:
- Website có script chạy ẩn gây CPU cao.
- Mở Performance → Record vài giây khi không thao tác gì.
- Thấy function mineCrypto() chạy liên tục → phát hiện crypto miner.
