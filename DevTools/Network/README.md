# Network

#### 1. Concept ( khái niệm ).
- Network tab là nơi Devtools ghi lại toàn bộ request và response mà trình duyệt gửi và nhận trong quá trình tải và tương tác với trang web.
  - Developer : dùng để debug tốc độ tải trang, lỗi HTTP, kiểm tra dữ liệu API, phân tích cache.
  - Pentester : dùng để thu thập endpoint , kiểm tra dữ liệu nhạy cảm, phân tích request/responce, phát hiện lỗ hổng (IDOR,XSS,CSRF,Auth bypass....). 
#### 2. Structure ( cấu trúc ).
##### 2.1. Toolbar. 
- Filter : lọc theo loại request (XHR,JS,CSS,...).
- Search (Ctrl+F): tìm chuỗi trong URL hoặc responce.
- Preserve log : giữ log khi reload hoặc chuyển trang .
- Disable cache : tắt cache khi debug.
- Throttling :  giả lập tốc độ mạng chậm để test performance.
##### 2.2. Request Table.
- Name : tên file hoặc endpoint.
- Status : mã HTTP (200,404,500...).
- type : loại file (document,script,xhr....).
- Initiator : nguồn gọi request (JS file,html..).
- Size : kích thước dữ liệu.
- Time : thời gian tải.
##### 2.3. Request Detail Panel.
- Headers : xem requuest header, responce headers.
- Payload : dữ liệu gửi lên (form-data,JSON).
- Responce : nội dung server trả về.
- Cookies : Cookie gửi và nhận
- Timing : thời gian từng giai đoạn (DNS lookup, TTFB,Content Download).
#### 3. Techniques Using Network (kỹ thuật dùng Network ).
##### 3.1. API Enumeration.
- Lọc `XHR` để chỉ xem API request.
- Tìm endpoint cẩn hoặc không được link trên giao diện.
- xem responce để phát hiện dữ liệu nhạy cảm.
##### 3.2. Authentication testing.
- kiểm tra Authorization header, cookie session.
- gửi lại request header/cookie sửa đổi để test privilege escalation.
##### 3.3. CSRF Testing.
- Copy request (copy --> copy as cURL) --> chạy lại từ terminal để kiểm tra có cần token CSRF hay không.
##### 3.4. Paramater Tampering.
- chỉnh giá trị paramater trong tab payload --> resend (hoặc dùng công cụ như burp Suite).
ví dụ : đổi `role=user` thành `role=admin`
##### 3.5. File Upload & Connent -Type Testing.
- upload file  --> kiểm tra request --> thử thau `content-type` hoặc tên file để bypass filter.
##### 3.6. WebSocker Inspection.
- chuyển filter sang WS --> xem message gửi / nhận qua websocket.
- tìm  dữ liệu quan tỏng hoặc lỗ hổng realtime.
#### 4. Practice ( thực hành ).
###### 1. Mở Network Tab --> filter XHR --> click request /api/getUser.
###### 2. trong tab Headers --> xem request URL
###### 3. Sửa URL thành /api/getUser?id=1002 bằng cách : 
  - Right click --> "copy as fetch" --> dán vào Console và sửa ID.
  - Hoặc  "COPY as cURL" --> Chạy trong terminal.
###### 4. Nếu Server Trả về dữ liệu user khác --> lỗ hổng IDOR.

Ví dụ : 
- khi form submit,Network tab cho thấy payload :
```
{"username":"test","role":"user"}
```
- Chỉnh thành :
```
{"username":"test","role":"admin"}
```
