### Access-Control-Allow-Origin Header

### 1. Khái niệm

`Access-Control-Allow-Origin` là **HTTP Response Header** thuộc nhóm **CORS (Cross-Origin Resource Sharing)**. Nó cho biết **tài nguyên trên server có thể được chia sẻ cho những Origin nào** (tức là nguồn gửi request, bao gồm protocol + domain + port).

Nếu không có header này (hoặc cấu hình sai), trình duyệt sẽ **chặn** request cross-origin vì vi phạm **Same-Origin Policy (SOP)**.


### 2. Cú pháp

```http
Access-Control-Allow-Origin: *
Access-Control-Allow-Origin: <origin>
Access-Control-Allow-Origin: null
```

* `*` → Cho phép **mọi origin** truy cập (chỉ dùng khi không có credential).
* `<origin>` → Chỉ cho phép **1 origin cụ thể**, ví dụ:

  ```http
  Access-Control-Allow-Origin: https://example.com
  ```
* `null` → Chỉ cho phép các request đến từ **origin = null** (ví dụ tài liệu sandbox, data URI). ⚠️ Không nên dùng vì có thể bị lợi dụng.


### 3. Nguyên tắc hoạt động

* **Server sẽ đọc giá trị `Origin` trong request** (gửi từ browser).
* Nếu Origin hợp lệ, server phản hồi lại với `Access-Control-Allow-Origin` = chính giá trị đó.
* Nếu không, browser sẽ chặn kết quả dù request server-side có trả dữ liệu.

Ví dụ request cross-origin:

```http
GET /data.json HTTP/1.1
Host: api.example.com
Origin: https://client.com
```

Response hợp lệ:

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://client.com
```

### 4. Trường hợp đặc biệt với **Credential (Cookie, Auth Header)**

* Nếu request có **credentials** (cookie, Authorization header, client certificate…), server **không được phép** dùng `*`.
* Bắt buộc phải trả về một **origin cụ thể**.

Ví dụ đúng:

```http
Access-Control-Allow-Origin: https://client.com
Access-Control-Allow-Credentials: true
```

### 5. Liên quan đến caching

Nếu server trả về nhiều origin khác nhau tuỳ client, cần thêm:

```http
Vary: Origin
```

→ để tránh cache nhầm dữ liệu cho origin khác.

### 6. Ví dụ thực tế

* Cho phép tất cả (public API, không credentials):

```http
Access-Control-Allow-Origin: *
```

* Chỉ cho phép client nội bộ:

```http
Access-Control-Allow-Origin: https://internal.company.com
Vary: Origin
```

