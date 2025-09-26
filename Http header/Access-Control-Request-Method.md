# Access-Control-Request-Method` header

### 1. Khái niệm

* `Access-Control-Request-Method` là một **HTTP request header** đặc biệt.
* Nó **chỉ xuất hiện trong preflight request** (tức là request **OPTIONS** mà browser gửi trước khi gửi request chính).
* Mục đích: cho server biết **method HTTP nào** (như `GET`, `POST`, `DELETE`, `PUT`, …) sẽ được sử dụng trong request thực sự sau đó.

--> Lý do cần có: vì preflight luôn dùng `OPTIONS`, nhưng request thật thì có thể dùng `POST` hay `DELETE`. Server phải biết trước để kiểm tra xem có cho phép method đó không.

### 2. Cấu trúc (Syntax)

```http
Access-Control-Request-Method: <method>
```

Trong đó:

* `<method>` là một **HTTP method hợp lệ**, ví dụ:

  * `GET`
  * `POST`
  * `PUT`
  * `DELETE`
  * `PATCH`

### 3. Directives (các giá trị)

* Chỉ có **một giá trị duy nhất**: `<method>`.
* Đây là tên method HTTP mà request thật sự sẽ dùng.
* Ví dụ:

  ```http
  Access-Control-Request-Method: POST
  ```

  Nghĩa là: request thực sự sẽ là `POST`, còn cái preflight (OPTIONS) chỉ đang hỏi trước server.

### 4. Sử dung.

#### Tình huống:

Một trang web `https://frontend.com` muốn gửi request `DELETE` đến API `https://api.backend.com/user/123`.

--> Vì `DELETE` không phải method an toàn mặc định (safe method), browser sẽ gửi preflight trước:

#### Preflight request (OPTIONS):

```http
OPTIONS /user/123 HTTP/1.1
Host: api.backend.com
Origin: https://frontend.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: content-type
```

#### Server response:

```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://frontend.com
Access-Control-Allow-Methods: GET, POST, DELETE
Access-Control-Allow-Headers: content-type
```

#### Request chính (thực sự) sau khi server cho phép:

```http
DELETE /user/123 HTTP/1.1
Host: api.backend.com
Origin: https://frontend.com
Content-Type: application/json
```

### 5. Mục đích.
- **Lý do tồn tại**: Khi thực hiện CORS preflight request (OPTIONS), browser cần báo cho server biết HTTP method nào sẽ được dùng trong request thật sự.
- **Mục tiêu chính**:

  1. Cho server cơ hội xác thực & kiểm soát (có cho phép method này không).
  
  2. Tránh việc server phải xử lý một request “nguy hiểm” trực tiếp (ví dụ: DELETE, PUT) khi chưa chắc nó được cho phép.

  3. Giúp cơ chế bảo mật Same-Origin Policy (SOP) không bị phá vỡ, nhưng vẫn cho phép chia sẻ tài nguyên an toàn giữa nhiều domain.

- **Ví dụ thực tế**: Một web app từ frontend.com muốn gọi API DELETE trên api.backend.com. Trước khi xoá dữ liệu thật, browser phải hỏi server backend:

  “Tôi định gửi một request DELETE. Ông có cho phép không?”
