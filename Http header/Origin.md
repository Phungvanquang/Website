# Origin HTTP Header

### 1. Khái niệm

* `Origin` là một HTTP request header **do trình duyệt tự động thêm** khi gửi request cross-origin (hoặc trong một số tình huống same-origin nhưng nhạy cảm).
* Nó chỉ định **nguồn gốc (origin)** của request: bao gồm **scheme (http/https), host, port**.
* Không giống `Referer`, header `Origin` **không chứa path** hay query string → chỉ mang phần **origin**.

Ví dụ:

```
Origin: https://example.com
```



### 2. Hoạt động

* Khi bạn gửi request qua **CORS (Cross-Origin Resource Sharing)** hoặc các hành động tiềm ẩn nguy cơ (POST, PUT, DELETE, WebSocket handshake...), trình duyệt sẽ kèm theo header `Origin`.
* Server sẽ dựa vào `Origin` để:

  * Cho phép hoặc từ chối request.
  * So sánh với `Access-Control-Allow-Origin`.
* Nếu request **same-origin** và không nhạy cảm → trình duyệt có thể không gửi `Origin`.

### 3. Cấu trúc (Syntax)

```
Origin: <scheme> "://" <host> [ ":" <port> ]
```

Ví dụ:

* `Origin: https://example.com`
* `Origin: http://evil.com:8080`


### 4. Trường hợp sử dụng

* **CORS**
  Khi frontend (ở `https://mysite.com`) gọi API ở `https://api.othersite.com`, request sẽ có:

  ```
  Origin: https://mysite.com
  ```

* **WebSocket**
  Khi mở kết nối WS:

  ```
  Origin: https://chat.example.com
  ```

* **POST form cross-site**
  Khi submit form từ site khác:

  ```
  Origin: https://attacker.com
  ```

### 5. So sánh với `Referer`

* **Referer**: chứa full URL (bao gồm path, query, fragment).
* **Origin**: chỉ chứa *origin* (scheme + host + port).
* → `Origin` bảo mật hơn vì tránh leak thông tin nhạy cảm trong URL.

### 6. Ví dụ request với `Origin`

```http
POST /api/data HTTP/1.1
Host: api.example.com
Origin: https://app.client.com
Content-Type: application/json

{ "username": "alice" }
```

Nếu server muốn chấp nhận → phải trả về:

```http
Access-Control-Allow-Origin: https://app.client.com
```

### 7. Mục đích bảo mật

* Giúp server **xác định nguồn gốc của request**, ngăn:

  * **CSRF (Cross-Site Request Forgery)**
  * **Cross-Site WebSocket Hijacking**
* Là nền tảng của **CORS policy**.



Bạn có muốn mình viết chi tiết **cách phân biệt `Origin` vs `Host` vs `Referer`** luôn không? Vì ba cái này hay bị nhầm trong bảo mật web 🔥.
