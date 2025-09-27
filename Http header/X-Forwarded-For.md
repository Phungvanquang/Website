# HTTP header `X-Forwarded-For (XFF)

### 1. Khái niệm

* `X-Forwarded-For` là một **request header** do **proxy hoặc load balancer** thêm vào.
* Nó dùng để chỉ ra **địa chỉ IP gốc của client** khi request đi qua một hoặc nhiều proxy.
* Nếu không có header này, server **chỉ thấy IP của proxy cuối cùng** → khó biết được ai là client thật sự.

### 2. Cú pháp

```http
X-Forwarded-For: <client-ip>, <proxy1-ip>, <proxy2-ip>, …, <proxyN-ip>
```

* **IP bên trái nhất (leftmost)**: IP của **client gốc**.
* Các IP phía sau: các proxy trung gian.
* **IP bên phải nhất (rightmost)**: proxy cuối cùng trước khi request đến server.

Ví dụ:

```http
X-Forwarded-For: 203.0.113.195, 2001:db8::370:7348, 198.51.100.178
```

* Client gốc: `203.0.113.195`
* Proxy trung gian: `2001:db8::370:7348`
* Proxy cuối: `198.51.100.178`
### 3. các giá trị Directives.
- `<client>` : Địa chỉ IP của máy khách.

- `<proxy>` : Địa chỉ IP của proxy. Nếu một yêu cầu đi qua nhiều proxy, địa chỉ IP của mỗi proxy kế tiếp sẽ được liệt kê. Điều này có nghĩa là địa chỉ IP ngoài cùng bên phải là địa chỉ IP của proxy gần đây nhất và địa chỉ IP ngoài cùng bên trái là địa chỉ của máy khách ban đầu (giả sử máy khách và proxy hoạt động tốt).

### 4. hoạt động (step-by-step).

1. Khi client gửi request qua một hoặc nhiều proxy
- Client (IP_C) gửi request → Proxy1 (P1) nhận.
- Nếu Proxy1 không thấy X-Forwarded-For, nó thêm: X-Forwarded-For: IP_C rồi forward request.
- Nếu Proxy1 thấy header đã có (ví dụ client giả mạo), thường Proxy1 sẽ append IP của phía nó vào cuối danh sách: X-Forwarded-For: <existing>, IP_C_or_original, P1.
- Request qua Proxy2 (P2): P2 thường append địa chỉ IP của P1: X-Forwarded-For: <leftmost original client>, ..., P1.
- Cuối cùng server nhận được một header dạng chuỗi các IP, phân cách bởi dấu phẩy.
- Quy ước: IP bên trái nhất = IP client gốc (nếu các proxy hành xử “đúng”), IP bên phải nhất = proxy gần server nhất.

2. Có thể có nhiều header X-Forwarded-For.
- Một số client/proxy có thể gửi nhiều header X-Forwarded-For. Cách an toàn: ghép tất cả header theo thứ tự nhận được (join bằng dấu phẩy) rồi parse thành list.
### 5. Cách parse (phải làm cẩn thận)

Quy trình chuẩn để parse một request:

1. Lấy tất cả header X-Forwarded-For (có thể nhiều).

2. Join các giá trị với dấu phẩy thành một chuỗi duy nhất (hoặc split từng header rồi nối danh sách).

3. Split theo dấu phẩy để thành danh sách tokens.

4. Trim spaces của mỗi token.

5. Validate mỗi token là IP hợp lệ (IPv4/IPv6). Bỏ token không hợp lệ.

6. (Tùy chọn) Bỏ các IP thuộc private ranges / loopback (10/8, 192.168/16, 172.16/12, 127.0.0.1, fc00::/7) khi bạn đang tìm IP public gần client.

7. Kết quả: danh sách IP theo thứ tự từ leftmost → rightmost.

### 6. Vấn đề bảo mật

Nếu server **tin tưởng toàn bộ X-Forwarded-For** thì có thể bị tấn công:

* **IP Spoofing**: client có thể gửi trực tiếp:

  ```http
  X-Forwarded-For: 1.2.3.4
  ```

  → server tưởng client đến từ `1.2.3.4`.

* Điều này có thể bypass:

  * Rate limiting (giới hạn request theo IP)
  * IP-based access control (chặn/mở IP)
  * Logging/Audit → log sai IP

Vì vậy:

* Chỉ nên tin tưởng phần **X-Forwarded-For được thêm bởi proxy do mình quản lý**.
* Có 2 cách:

  1. **Trusted proxy count**: cấu hình số lượng proxy tin cậy → lấy IP ở vị trí tương ứng.
  2. **Trusted proxy list**: định nghĩa danh sách IP của proxy tin cậy → bỏ qua chúng, lấy IP thật của client.

### 7. Khi nào nên dùng?

* Khi hệ thống có **reverse proxy, CDN (Cloudflare, Akamai…), load balancer (HAProxy, Nginx, ELB)**.
* Giúp app/server backend log đúng IP client.

### 8. Ví dụ thực tế

**Client trực tiếp (không qua proxy):**

```http
GET / HTTP/1.1
Host: example.com
```

Server thấy đúng IP client (192.168.1.100 chẳng hạn).

**Client qua proxy:**

```http
GET / HTTP/1.1
Host: example.com
X-Forwarded-For: 192.168.1.100, 10.0.0.1
```

* `192.168.1.100` = client
* `10.0.0.1` = proxy gần nhất

