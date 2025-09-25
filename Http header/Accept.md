# Accept 

#### 1. khái niệm. 
- Accept là  yêu cầu và phản hồi HTTP cho biết loại nội dung nào, được biểu thị dưới dạng kiểu MIME, người gửi có thể hiểu được. Trong các yêu cầu, máy chủ sử dụng đàm phán nội dung để chọn một trong các đề xuất và thông báo cho khách hàng về lựa chọn với tiêu đề phản hồi Content-Type. Trong phản hồi, nó cung cấp thông tin về loại nội dung mà máy chủ có thể hiểu trong thông báo đến tài nguyên được yêu cầu, để kiểu nội dung có thể được sử dụng trong các yêu cầu tiếp theo đến tài nguyên.
-Trình duyệt đặt các giá trị bắt buộc cho tiêu đề này dựa trên ngữ cảnh của yêu cầu. Ví dụ: trình duyệt sử dụng các giá trị khác nhau trong yêu cầu khi tìm nạp biểu định kiểu, hình ảnh, video hoặc tập lệnh CSS. 
#### 2. cú pháp. 
**`Accept: <MIME_type>/<MIME_subtype> | <MIME_type>/* | */* `**
#### 3. hoạt động. 
- client gửi gói request tới server - server nhận được sử dụng header Accept và đưa ra cách sử lí. 
#### 4. mục đích. 
- xác đinh kiểu dữ liệu của body. - xác nhận kiễu dữ liệu thật và fake. - cách sử lí đầu vào của dữ liệu.

#### 5. Ví dụ.

```
GET /index.html HTTP/1.1
Host: example.com
Accept: text/html, application/xhtml+xml, application/xml;q=0.9, */*;q=0.8
```

#### 6. Các loại MIME phổ biến.

**Văn Bản :**

- `text/html` → HTML (trang web)

- `text/plain` → Plain text (chữ thô, không format)

- `text/css` → CSS stylesheet

- `text/csv` → Dữ liệu bảng dạng CSV

- `text/javascript hoặc application/javascript` → JavaScript
  
**Ứng dụng (Application):**
  
- `application/json` → JSON (API phổ biến nhất)

- `application/xml` → XML

- `application/xhtml+xml` → XHTML

- `application/pdf` → PDF

- `application/zip` → File nén ZIP

- `application/octet-stream` → Binary data (fallback khi không rõ định dạng)
  
**Ảnh (image) :**

- `image/jpeg` → JPEG

- `image/png` → PNG

- `image/gif` → GIF

- `image/webp` → WebP

- `image/svg+xml` → SVG
  
**Âm thanh/Video :**

- `audio/mpeg` → MP3

- `audio/ogg` → OGG

- `video/mp4` → MP4

- `video/webm` → WebM

- `video/ogg` → OGG video
  
**Loại khác :**

- `multipart/form-data → upload form` (file + text)

- `application/x-www-form-urlencoded` → form cơ bản

- `*/*` → nhận mọi loại (wildcard)
