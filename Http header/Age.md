# Age

#### 1. Khái niệm.
- `Age` là 1 header responce trong HTTP/1.1
- Nó cho biết thời gian (tính bằng giây) mà bản sao (cached responce) đã tồn tại kẻ từ khi được lưu trong cache.
- header này thường được thêm bởi các proxy cache hoặc CDN (chứ không phải web server gốc).
  
#### 2. Hoạt động.
- Khi client nhận được response có Age, nó sẽ biết bản này không phải "fresh" 100% từ origin server, mà được trả về từ cache trung gian.
- Age luôn tăng dần theo thời gian (khi cache tiếp tục giữ bản đó).
- Nếu không có cache → server gốc trả response → thường không có header Age
#### 3. Mối quan hệ với các header khác.
- Cache-Control: max-age=600

→ Cho phép bản response được xem là hợp lệ tối đa 600 giây.
- Nếu Age: 120 → thì response này đã tồn tại 120 giây. Client có thể tính còn 480 giây trước khi cache hết hạn.

Ngoài ra, Age thường đi kèm:
- Expires
- Cache-Control (max-age, s-maxage)
#### 4. Tác đông thực tế.

Trong Performance (tăng tốc website): CDN/proxy gửi response từ cache thay vì phải gọi server gốc.

Trong Security/Pentesting:

- Age cho pentester biết response có được cache hay không.
- Nếu thấy Age luôn tăng → chứng tỏ có proxy/CDN cache ở giữa (Cloudflare, Varnish, Squid, Akamai…).
- Có thể khai thác trong các lỗi Cache Poisoning, Cache Deception, Cache Key Confusion.
#### 5. Ví dụ.

Request :

```
GET /index.html HTTP/1.1
Host: example.com

```

Response từ cache :

```
HTTP/1.1 200 OK
Cache-Control: max-age=600
Age: 120
Content-Type: text/html

```

--> Nội dung này đã ở trong cache 2 phút, và còn hợp lệ trong 8 phút nữa trước khi cache hết hạn.
