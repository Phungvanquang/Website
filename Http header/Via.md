# HTTP header `Via`

### 1. **Khái niệm**
* `Via` là một header **request và response** do **proxy** (forward hoặc reverse) thêm vào khi chuyển tiếp message. Mục đích chính: **theo dõi các hop (proxy) trong chuỗi**, **tránh vòng lặp (loop)** và **ghi nhận thông tin giao thức** mà mỗi proxy đã dùng khi forward.

**Lưu ý:** theo spec, `Via` là **forbidden request header** trong trình duyệt (script không được phép tự do thiết lập nó), nhưng các proxy/servers vẫn có thể thêm/ghi/ghi đè.

### 2. **Cú pháp**

```
Via: [<protocol-name>/]<protocol-version> <host>[:<port>]
Via: [<protocol-name>/]<protocol-version> <pseudonym>
```

* `<protocol-name>` (tùy chọn): ví dụ `HTTP`.
* `<protocol-version>`: ví dụ `1.0`, `1.1`, `2`.
* `<host>`: host hoặc IP của proxy (có thể kèm port).
* `<pseudonym>`: bí danh/alias cho proxy (khi không muốn/không có host công khai).

### 3. **Các giá trị / Directives**

* `<protocol-name>`: tên giao thức (không bắt buộc).
* `<protocol-version>`: phiên bản giao thức (bắt buộc trong phần định dạng).
* `<host>`: địa chỉ công khai của proxy (hoặc tên).
* `<pseudonym>`: tên thay thế cho proxy nội bộ (giữ riêng tư).

### 4. **Hoạt động (step-by-step)**

* Client gửi request ban đầu (thường không có `Via`).
* Khi request đi qua **Proxy1**, Proxy1 **thêm** một phần `Via` (ví dụ `1.0 fred`).
* Khi request tiếp tục qua **Proxy2**, Proxy2 **append** thông tin của nó vào danh sách: `Via: 1.0 fred, 1.1 p.example.net`.
* Server nhận request có `Via` thể hiện chuỗi proxy (left-to-right = proxy đầu tiên → proxy sau).
* Khi response quay về, các proxy cũng có thể append `Via` vào response để client biết chuỗi đường đi của response.
* Proxies dùng `Via` để **phát hiện vòng lặp**: nếu một proxy thấy chính nó trong `Via`, proxy có thể drop/không forward tiếp để tránh loop.

5. **Cách parse (phải làm cẩn thận)**

* Có thể có **nhiều header `Via`**; hợp nhất các header (join) theo thứ tự nhận được thành một chuỗi rồi xử lý.
* **Split** theo dấu phẩy để lấy từng entry.
* **Trim** khoảng trắng; mỗi entry có dạng `[protocol/]<version> <host-or-pseudonym>`.
* Validate phần version và host/pseudonym; xử lý trường hợp thiếu host (chỉ pseudonym).
* Lưu ý: host có thể là tên không chuẩn (pseudonym) — không cố gắng giải IP từ đó nếu nó là bí danh nội bộ.

### 6. **Vấn đề bảo mật & quyền riêng tư**

* `Via` **có thể lộ thông tin hạ tầng** (hostname/proxy nội bộ) — gây rủi ro fingerprinting.
* Client hoặc attacker (nếu không bị ngăn) có cố gắng gửi `Via` giả — tuy trình duyệt hạn chế, nhưng custom client có thể. Vì vậy **không dùng `Via` cho quyết định bảo mật (auth, block)** nếu chuỗi proxy không thuộc vùng tin cậy.
* Proxies nên **sanitize**: nếu muốn giữ riêng tư, có thể dùng pseudonym hoặc xóa/đổi `Via` trước khi trả về client.
* `Via` giúp phát hiện loop, nhưng chính header cũng có thể bị lợi dụng để dò cấu trúc mạng.

### 7. **phổ biến**

* Hệ thống có **multiple proxies / CDN / load balancer** để debug, tracing đường đi request/response.
* Dùng để **phòng chống loop** trên proxy chain.
* Dùng cho logging/troubleshooting (biết request đã qua những proxy nào).

### 8. **Ví dụ thực tế**

* Một proxy đơn giản (Heroku router) có thể thêm:

```
Via: 1.1 vegur
```

* Proxy khác dùng bí danh:

```
Via: HTTP/1.1 GWA
```

* Nhiều proxy:

```
Via: 1.0 fred, 1.1 p.example.net
```

Ở đây `fred` là proxy đầu tiên, `p.example.net` là proxy tiếp theo.

### 9. **Khuyến nghị vận hành**

* **Không tin `Via` cho security decisions** trừ khi bạn kiểm soát toàn bộ chuỗi proxy.
* Nếu không muốn lộ thông tin nội bộ, **dùng pseudonym** hoặc **loại bỏ/sửa `Via`** trước khi trả về client.
* Sử dụng `Via` cho **debug / loop detection / diagnostic logging**, không phải cho xác thực user.

