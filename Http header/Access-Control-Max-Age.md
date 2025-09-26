# Access-Control-Max-Age

### 1. Khái niệm cốt lõi

* Đây là một **HTTP Response Header** trong CORS.
* Nó cho trình duyệt biết: **kết quả của preflight request (OPTIONS)** được phép cache trong bao lâu.
* Kết quả ở đây gồm các thông tin server trả về:

  * `Access-Control-Allow-Methods`
  * `Access-Control-Allow-Headers`
  * (và cả `Access-Control-Allow-Credentials`, `Access-Control-Allow-Origin` nếu có)

--> Nếu không có header này hoặc giá trị quá thấp, browser sẽ phải **gửi lại preflight request nhiều lần**, gây **tăng độ trễ**.

**Giải thích chi tiết :**

* Khi bạn viết code web (JavaScript) và gọi đến API ở domain khác (ví dụ từ `abc.com` gọi sang `api.xyz.com`), trình duyệt **không gửi request thật ngay lập tức**.
* Nó sẽ gửi trước một request kiểm tra (OPTIONS request), gọi là **preflight**.
* Mục đích: hỏi server xem "Tôi có được phép gọi API này với method và headers kia không?".

Ví dụ trình duyệt gửi:

```http
OPTIONS /users HTTP/1.1
Origin: https://abc.com
Access-Control-Request-Method: DELETE
Access-Control-Request-Headers: Content-Type
```

Server trả lời:

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://abc.com
Access-Control-Allow-Methods: GET, POST, DELETE
Access-Control-Allow-Headers: Content-Type
Access-Control-Max-Age: 600
```

- Vậy `Access-Control-Max-Age` là gì?

  * Đây là cái **thời gian cache** (tính bằng giây) của kết quả preflight ở trên.
  * Nói đơn giản:
  
    * Trình duyệt hỏi 1 lần → server trả lời OK.
    * Nếu có `Access-Control-Max-Age: 600`, thì trong vòng **10 phút tiếp theo**, trình duyệt sẽ **không hỏi lại nữa** mà cứ gửi request thẳng luôn.

- Nếu không có `Access-Control-Max-Age`?
  
  * Trình duyệt sẽ **luôn hỏi lại preflight** trước mỗi request → gây chậm trễ.
  * Ví dụ bạn reload web liên tục → mỗi lần đều mất 1 OPTIONS trước rồi mới gửi request chính.

### 2. Cú pháp

```http
Access-Control-Max-Age: <delta-seconds>
```

* `<delta-seconds>` = số giây (integer ≥ 0).
* Ví dụ:

  * `600` = cache 10 phút.
  * `86400` = cache 24 giờ.
    
### 3. Hành vi mặc định theo browser

* **Firefox**: tối đa `86400` giây (24h).
* **Chromium trước v76**: tối đa `600` giây (10 phút).
* **Chromium v76 trở lên**: tối đa `7200` giây (2h).
* Nếu không chỉ định → mặc định là `5 giây`.

--> Nghĩa là nếu bạn đặt `Access-Control-Max-Age: 86400`, thì trên Chrome chỉ cache tối đa 2h, còn Firefox có thể giữ nguyên 24h.


### 4. Tác động hiệu năng

* **Không có Max-Age**: mọi request "khó" (POST, PUT, DELETE với custom headers) → browser phải gửi lại preflight trước khi thực hiện request chính → tăng latency.
* **Có Max-Age cao**: giảm lượng preflight, cải thiện tốc độ đáng kể với các API nhiều request.

Ví dụ:

```http
Access-Control-Max-Age: 600
```

--> Browser sẽ nhớ **trong 10 phút tới**: origin này được phép gửi các method/headers như đã khai báo → bỏ qua preflight.


### 5. Góc nhìn bảo mật

**Dùng `Access-Control-Max-Age` cũng có rủi ro:**

* Nếu **CORS config thay đổi** (ví dụ bạn chặn `DELETE` sau này), nhưng browser đã cache rule cũ trong 1h → client vẫn có thể gửi request cũ mà không bị chặn ngay.
* Vì vậy:

  * API **nhạy cảm** (auth, admin) → nên để giá trị nhỏ (5–60s).
  * API **public, ổn định** → có thể để lớn (600–7200s) để tối ưu hiệu năng.

### 6. Ví dụ

#### Cache 10 phút (API ổn định, ít thay đổi policy)

```http
Access-Control-Max-Age: 600
```

#### Cache 24h (public API, ít rủi ro bảo mật)

```http
Access-Control-Max-Age: 86400
```

#### Không khuyến nghị (tắt cache)

```http
Access-Control-Max-Age: 0
```

→ Browser sẽ luôn gửi preflight cho mỗi request → cực tốn tài nguyên.

### 7. Mối quan hệ với các header khác

* `Access-Control-Allow-Methods` + `Access-Control-Allow-Headers` → định nghĩa quyền cho preflight.
* `Access-Control-Max-Age` → quyết định **trình duyệt nhớ quyền đó bao lâu**.

Nghĩa là:

* `Allow-*` = bạn **cho phép cái gì**.
* `Max-Age` = bạn **cho phép nhớ bao lâu**.


