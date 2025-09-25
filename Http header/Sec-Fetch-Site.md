# Sec-Fetch-Site

#### 1. Khái niệm.
- `Sec-Fetch-Site` là một Fetch Metadata Request Header do trình duyệt tự động thêm vào request HTTP, nhằm cho server biết mối quan hệ giữa nguồn khởi tạo request (initiator) và nguồn tài nguyên được yêu cầu (requested resource).

--> Nói đơn giản: nó cho server biết request này xuất phát từ cùng nguồn, cùng site, khác site, hay là do người dùng chủ động nhập URL.
#### 2. Các giá trị (Directives).

- **`same-origin`**

  - Request đến từ cùng origin (scheme + host + port).
  - Ví dụ: https://mysite.com/page gọi https://mysite.com/api.

- **`same-site`**

  - Request đến từ cùng site (cùng domain gốc, khác subdomain cũng tính là cùng site nếu có cùng scheme).
  - Ví dụ: https://app.mysite.com gọi https://api.mysite.com → đây là same-site.

- **`cross-site`**

  - Request đến từ một site khác hoàn toàn.
  - Ví dụ: https://evil.com gọi https://mysite.com.
  - Đây là trường hợp nguy hiểm nhất vì liên quan tới CSRF, XSS, data exfiltration.

- **`none`**

  - Request không đến từ trang web nào cả, mà từ người dùng trực tiếp.
  - Ví dụ: nhập URL vào thanh địa chỉ, mở bookmark, hay kéo thả file vào trình duyệt.
    
#### 3. hoạt động.

1. Trình duyệt tự động thêm Sec-Fetch-Site vào header của request.
2. Server có thể kiểm tra giá trị:
- Nếu same-origin hoặc same-site → thường cho phép.
- Nếu cross-site → có thể từ chối (403) hoặc yêu cầu cơ chế bảo vệ (CORS, CSRF token).
- Nếu none → coi là safe vì do người dùng trực tiếp.
3. Nhờ đó server giảm thiểu rủi ro từ các request cross-site forgery.

#### 4. Mục đích.
**Bảo mật:**

- Giúp chống CSRF (Cross-Site Request Forgery), vì server biết request đến từ đâu.
- Hạn chế XSSI (Cross-Site Script Inclusion).
- Giảm nguy cơ Clickjacking bằng cách từ chối request từ site lạ.

**Tăng độ tin cậy:**

- Chuẩn hóa thay vì phân tích thủ công Referer.
- Trình duyệt đảm bảo luôn thêm header đúng theo chuẩn → dễ xử lý hơn.

**Tích hợp với CORS & CSRF defense:**

- Giúp chính sách CORS chặt chẽ hơn.
- Bổ sung lớp phòng vệ cùng với CSRF token.

#### 5. Sử dụng.

**Same-origin**
```
GET /data.json HTTP/1.1
Host: mysite.com
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty

```

**❌ Cross-site (nguy hiểm hơn)**

```
GET /data.json HTTP/1.1
Host: mysite.com
Sec-Fetch-Site: cross-site
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty

```
