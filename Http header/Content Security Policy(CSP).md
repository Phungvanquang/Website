
# Content Security Policy
# I.Tổng quan.
## 1. khái niệm CSP.
- Content Security Policy (CSP) là biện pháp bảo mật thiết yếu để bảo vệ các ứng dụng web khỏi một số loại tấn công nhất định. Bằng cách xác định các quy tắc nghiêm ngặt về tài nguyên mà trình duyệt có thể tải, CSP hạn chế các vectơ tấn công tiềm ẩn.
- Content Security Policy (CSP) biện pháp bảo vệ bổ sung giúp phát hiện và hạn chế một số cuộc tấn công từ phía máy khách, chẳng hạn như Cross Site Scripting (XSS) , clickjacking và chèn nội dung.
## 2.hoạt động CSP.
- CSP (hay Content-Security-Policy) thực chất là một header nằm trên request khi được gửi đến một server nào đó.Để enable CSP, chúng ta chỉ cần cấu hình cho server của chúng ta trả về Content-Security-Policy HTTP headers.Để xem danh sách các thẻ policy đầy đủ bạn có thể xem tại Content-Security-Policy header Một cách khác chúng ta có thể sử dụng thông qua thẻ <meta>. Ví dụ: <meta http-equiv=”Content-Security-Policy” content=”default-src ‘self’; img-src https://*; child-src ‘none’;”>
## 3.Một số chính sách của CSP.

### Các chỉ thị này xác định các nguồn được ủy quyền cho các tài nguyên HTML khác nhau.

- ```default-src```  Nguồn mặc định nếu không có chỉ thị nào khác được xác định.
- ```script-src```  Nguồn hợp lệ cho các tập lệnh JavaScript và WebAssembly.
- ```script-src-elem```  Nguồn hợp lệ cho ```<script>```thẻ. Nếu không có, script-srcsẽ được sử dụng.
- ```frame-src``` Nguồn hợp lệ cho ```<frame>```và ```<iframe>```.
- ```img-src```  Nguồn hợp lệ cho ```<object>```, ```<embed>```và ```<applet>```.
- ```style-src```  Nguồn hợp lệ cho bảng định kiểu.
- ```font-src```  Nguồn phông chữ hợp lệ

### Một số chỉ thị không liên quan đến việc khôi phục tài nguyên nhưng bổ sung thêm các biện pháp kiểm soát bảo mật

- ```sandbox``` : Kích hoạt một hộp cát để cô lập nội dung nhất định (như đối với ```<iframe>```).
- ```require-trusted-types-for``` : Áp dụng 'các loại đáng tin cậy' để hạn chế các cuộc tấn công XSS dựa trên DOM .
- ```trusted-types``` : Xác định danh sách trắng 'Loại đáng tin cậy' để ngăn chặn việc thực thi dữ liệu giả mạo.
- ```upgrade-insecure-requests``` : Tự động chuyển đổi yêu cầu HTTP sang HTTPS. Hữu ích cho việc hiện đại hóa một trang web có nhiều URL cũ.
- ```frame-ancestors``` : Hạn chế các nguồn được phép cho các phần tử ```<frame>```, ```<iframe>```, ```<object>```, ```<embed>``` và ```<applet>```
- ```form-action``` : Kiểm soát các URL được phép gửi biểu mẫu.
- ```base-uri``` : Hạn chế các nguồn hợp lệ cho ```<base>``` thẻ.

## 4.bỏ qua các chính sách CSP.
- Xác thực JavaScript không an toàn trong dòng : ```Content-Security-Policy: default-src 'none'; script-src 'unsafe-inline';```
  + payload :  ```<script>alert(1);</script>```
- Ủy quyền đánh giá không an toàn :  ```Content-Security-Policy: default-src 'none'; script-src 'unsafe-eval' data:; ```
 + payload :  ``` <script src="data:;base64, YWxlcnQoMSk="></script>```
- Sử dụng ký tự đại diện trong script-src :  ```Content-Security-Policy: default-src 'none'; script-src https://vaadata.com *; ```
  + payload :  ``` <script src="https://evil.vaadata.at"></script>  ```
                 ```<script src="data:;base64, YWxlcnQoMSk="></script> ```
- Không có chỉ thị object-src và default-src :  ```Content-Security-Policy: script-src ‘self’; img-src ‘self’; img-src 'self’;```
  + payload : ```<object data="data:text/html;base64,PHNjcmlwdD5hbGVydCgxKTwvc2NyaXB0Pg=="></object>```
- Khai thác điểm cuối JSONP được ủy quyền :
- 
  **```Content-Security-Policy: default-src 'none';```
  ```script-src https://hello.vaadata.com/test.js ```
```https://accounts.google.com/o/oauth2/revoke;```**
- Bỏ qua CSP thông qua việc tải tệp lên : ```Content-Security-Policy: default-src 'self'```
  + payload : ```<script src="/upload/evil-script.js"></script>```

### Tài Liệu tham khảo.
- https://websecblog.com/vulns/google-csp-evaluator/
- https://medium.com/@vikramroot/part-2-everything-you-need-to-know-about-browser-security-policies-csp-cookie-attributes-etc-3ea98f737b3a
  
