# X-Content-Type-Options


## 1. Khái niệm

* `X-Content-Type-Options` là một **HTTP Response Header** (máy chủ trả về cho client).
* Nó được dùng để **chặn trình duyệt thực hiện MIME Sniffing**.
* MIME Sniffing = khi trình duyệt cố gắng **đoán** loại file (MIME type) thay vì tuân theo header `Content-Type` do server trả về.

  * Ví dụ: server trả về file `.txt`, nhưng bên trong lại có JavaScript → một số trình duyệt cũ có thể cố đoán và thực thi như JavaScript → nguy cơ XSS.

## 2. Cú pháp

```http
X-Content-Type-Options: nosniff
```

* Chỉ có **một giá trị duy nhất hợp lệ**: `nosniff`.

## 3. Cách hoạt động

Khi trình duyệt nhận được header này:

* Nếu **Content-Type = text/css**, thì file đó chỉ được tải như CSS.
* Nếu **Content-Type = JavaScript MIME type** (ví dụ: `application/javascript`), thì file đó chỉ được thực thi như JS.
* Nếu file có MIME không khớp (ví dụ: server trả về `text/plain` nhưng bên trong là JS), thì trình duyệt sẽ **chặn** thay vì cố gắng đoán.

**Điều này giúp tránh tấn công kiểu **MIME Confusion / MIME Sniffing XSS**.**

## 4. Ứng dụng bảo mật

* Được khuyến nghị trong **OWASP Secure Headers Project**.
* Hầu hết các security scanner (Burp, Nikto, OWASP ZAP, …) sẽ cảnh báo nếu header này **không tồn tại**.
* Thường được bật kèm với `Content-Type` chuẩn và `Content-Security-Policy` để tăng cường bảo vệ.

## 5. Ví dụ thực tế

**Không có header này (dễ bị tấn công):**

```http
HTTP/1.1 200 OK
Content-Type: text/plain
```

Trình duyệt có thể sniff ra file chứa `<script>` và chạy.

**Có header `nosniff` (an toàn hơn):**

```http
HTTP/1.1 200 OK
Content-Type: text/plain
X-Content-Type-Options: nosniff
```

Trình duyệt sẽ chỉ coi file là text, **không thực thi script**.

