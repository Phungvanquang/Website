# Timing-Allow-Origin

### 1. **Khái niệm**

* `Timing-Allow-Origin` là **response header** mà **server trả về** cho browser.
* Nó quyết định xem **trang web nào (origin)** được phép xem chi tiết **Resource Timing metrics** (ví dụ: `connectStart`, `responseEnd`, `transferSize`, …) thông qua **Resource Timing API**.
* Nếu **không có header này** → khi resource là cross-origin, browser sẽ che giấu thông số timing (trả về `0`) để **ngăn rò rỉ thông tin nhạy cảm**.


### 2. **Các giá trị Directives**

```
Timing-Allow-Origin: *
Timing-Allow-Origin: https://trusted.com
Timing-Allow-Origin: https://a.com, https://b.com
```

* `*`

  * Cho phép **mọi origin** được xem dữ liệu timing.
  * An toàn nếu resource là **tài nguyên công khai** (CSS, JS, hình ảnh).
  * Không nên dùng cho tài nguyên nhạy cảm.

* `<origin>`

  * Chỉ định **một origin cụ thể** được phép đọc thông tin timing.
  * Cấu trúc: `scheme://host[:port]`.
  * Có thể liệt kê nhiều origin, cách nhau dấu phẩy.


### 3. **Hoạt động**

* Khi một web app gọi **Resource Timing API**:

```js
performance.getEntriesByType("resource")
```

* Nếu resource **cùng origin** → browser hiển thị đầy đủ metrics.
* Nếu resource **cross-origin**:

  * Browser gửi request → server trả response.
  * Nếu response có header `Timing-Allow-Origin` (với origin khớp hoặc `*`):
    → Browser cung cấp đầy đủ metrics (DNS lookup, TLS handshake, transfer size, …).
  * Nếu response **không có header** hoặc origin không được phép:
    → Browser trả về metrics bị “zero hóa” (ẩn thông tin).

### 4. **Mục đích**

* **Minh bạch hiệu năng**: Cho phép trang client theo dõi hiệu suất tài nguyên lấy từ CDN, API hoặc 3rd-party service.
* **Bảo mật**: Ngăn chặn trang web lạ (không đáng tin cậy) khai thác thông tin timing để **suy đoán cấu hình server** hoặc thực hiện **side-channel attack**.
* **Kiểm soát chủ động**: Chỉ server mới quyết định **origin nào được phép** xem chi tiết timing.


### 5. **Sử dụng**

#### 5.1. Cho phép tất cả (public asset):

```
HTTP/1.1 200 OK
Timing-Allow-Origin: *
Content-Type: application/javascript
```

→ Dùng cho file JS/CSS công khai, CDN muốn ai cũng đo hiệu năng được.

#### 5.2. Cho phép một origin tin cậy:

```
HTTP/1.1 200 OK
Timing-Allow-Origin: https://analytics.client.com
Content-Type: application/json
```

→ Chỉ trang `analytics.client.com` đọc được dữ liệu timing chi tiết.

#### 5.3. Cho phép nhiều origin:

```
HTTP/1.1 200 OK
Timing-Allow-Origin: https://app.clientA.com, https://dashboard.clientB.com
Content-Type: application/json
```

→ Server whitelist nhiều client cụ thể.

#### 5.4. JavaScript minh họa:

```js
// Nếu server trả Timing-Allow-Origin phù hợp
const entries = performance.getEntriesByType("resource");
entries.forEach(entry => {
  console.log(entry.name, entry.responseEnd, entry.transferSize);
});
```

→ Nếu không có `Timing-Allow-Origin`, các giá trị như `transferSize` sẽ = 0.


