# Access-Control-Request-Headers

### 1. Khái niệm

* `Access-Control-Request-Headers` là **request header đặc biệt**, chỉ xuất hiện trong **preflight request (OPTIONS)**.
* Nó cho server biết: *"Lát nữa khi tôi gửi request thật (GET/POST/DELETE...), tôi sẽ kèm theo những custom headers này. Anh có cho phép không?"*.
* Server sẽ trả lời bằng `Access-Control-Allow-Headers`. Nếu trùng khớp → request thật được phép gửi.

### 2. Cấu trúc (Syntax)

```http
Access-Control-Request-Headers: <header-name>, <header-name>, …
```

* **`<header-name>`**:

  * Danh sách tên header mà client dự định gửi trong request chính.
  * Được viết **lowercase** (theo chuẩn).
  * Phân tách bằng dấu `,` (comma).
  * Không được lặp trùng.

### 3. Hoạt động

#### Luồng Preflight với Header này:

1. Trình duyệt chuẩn bị gửi request có custom header (ví dụ `X-Auth-Token`).
2. Trình duyệt gửi `OPTIONS` kèm `Access-Control-Request-Headers`.
3. Server nhận được và kiểm tra → phản hồi bằng `Access-Control-Allow-Headers`.
4. Nếu có match → trình duyệt mới gửi request chính.

### 4. Sử dụng.

#### Ví dụ 1: Client muốn gửi `Content-Type` và `X-PingOther`

Request (preflight):

```http
OPTIONS /api/data HTTP/1.1
Origin: https://example.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type, x-pingother
```

Response (server cho phép):

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://example.com
Access-Control-Allow-Methods: POST
Access-Control-Allow-Headers: content-type, x-pingother
```

→ Trình duyệt thấy OK → gửi request chính:

```http
POST /api/data
Origin: https://example.com
Content-Type: application/json
X-PingOther: hello
```

#### Ví dụ 2: Server từ chối

Request:

```http
OPTIONS /api/data
Origin: https://evil.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: x-secret
```

Response (server không cho phép):

```http
HTTP/1.1 403 Forbidden
```

→ Trình duyệt **không gửi request chính** nữa.

### 5. Directives (Giá trị có thể có)

* **`<header-name>`**:

  * Danh sách headers mà client muốn dùng (vd: `authorization, content-type, x-custom-header`).
  * Không có wildcard (`*`).
  * Nếu không có header nào đặc biệt → client **không gửi** `Access-Control-Request-Headers`.

### 6. Tóm tắt vai trò

* **Mục đích**: Cho server biết client sẽ dùng headers gì → để server chủ động kiểm soát.
* **Hoạt động**: Đi kèm preflight request, bắt buộc nếu có custom headers.
* **Liên quan**: Server phải trả về `Access-Control-Allow-Headers` để đồng ý.
