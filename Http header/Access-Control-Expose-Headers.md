# Access-Control-Expose-Headers

### 1. Khái niệm cốt lõi

* Đây là một **HTTP Response Header** thuộc **CORS**.
* Nó quy định **trình duyệt** được phép **tiết lộ (expose)** thêm những header nào trong **JavaScript (Fetch API, XHR)** khi gọi **cross-origin request**.

--> Nếu server **không khai báo**, thì **script trong browser chỉ truy cập được một số header mặc định (CORS-safelisted)**. Những header khác **có tồn tại trong response nhưng sẽ bị ẩn**.

### 2. Các header mặc định (safelisted headers) mà luôn truy cập được

Theo chuẩn Fetch, mặc định JS chỉ có thể đọc:

* `Cache-Control`
* `Content-Language`
* `Content-Length`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

Cảnh báo : Bất kỳ header nào khác (ví dụ `Authorization`, `ETag`, `X-Custom-Header`) sẽ **bị chặn** khỏi `response.headers.get()` nếu server không khai báo thêm qua `Access-Control-Expose-Headers`.


### 3. Cú pháp

```http
Access-Control-Expose-Headers: <header-name>[, <header-name>]*
Access-Control-Expose-Headers: *
```

* `<header-name>` → danh sách các header cho phép truy cập.
* `*` → cho phép tất cả header, nhưng **chỉ hợp lệ nếu request không có credentials** (cookie, Authorization header).

Nếu request có credentials → `*` sẽ không hoạt động như wildcard mà bị hiểu là một header literal có tên `"*"` → tức là không hữu ích.

### 4. Cơ chế hoạt động (luồng thực tế)

Giả sử bạn có một API:

```http
HTTP/1.1 200 OK
Content-Type: application/json
X-Request-ID: abc123
ETag: "xyz"
```

--> Ở phía client (JS):

```js
fetch("https://api.example.com/data", { credentials: "include" })
  .then(res => {
    console.log(res.headers.get("Content-Type")); // OK
    console.log(res.headers.get("X-Request-ID")); // ❌ null nếu không expose
  });
```

→ `X-Request-ID` **không thấy được** vì không nằm trong safelist.

Muốn cho phép client đọc:

```http
Access-Control-Expose-Headers: X-Request-ID, ETag
```

Khi đó JS có thể:

```js
console.log(res.headers.get("X-Request-ID")); // ✅ abc123
```

### 5. Ví dụ sử dụng thực tế

#### Expose một header chuẩn:

```http
Access-Control-Expose-Headers: Content-Encoding
```

#### Expose nhiều header, kể cả custom:

```http
Access-Control-Expose-Headers: Content-Encoding, X-RateLimit-Remaining, ETag
```

#### Public API không dùng credential:

```http
Access-Control-Expose-Headers: *
```


### 6. Góc nhìn **security**

* Nếu expose bừa bãi (ví dụ expose cả `Authorization` hay `Set-Cookie`), JS của bất kỳ website nào (nếu CORS cho phép) có thể **đọc được thông tin nhạy cảm**.
* Vì vậy:

  * **Chỉ expose những header cần thiết cho frontend**.
  * Với API có credential → tránh dùng `*`.
  * Kết hợp với `Access-Control-Allow-Origin` chính xác (không để `*` khi có auth).

### 7. Quan hệ với các header CORS khác

* `Access-Control-Allow-Origin` → quyết định **origin nào được phép truy cập tài nguyên**.
* `Access-Control-Allow-Methods` → quyết định **phương thức nào được phép gọi** (GET, POST, DELETE…).
* `Access-Control-Allow-Headers` → quyết định **client được phép gửi header gì trong request**.
* `Access-Control-Expose-Headers` → quyết định **client được phép đọc header nào trong response**.

Nói cách khác:

* `Allow-*` = về **gửi request**.
* `Expose-*` = về **đọc response**.

