# Sec-Fetch-Dest



#### 1. Khái niệm.
- `Sec-Fetch-Dest` là một Fetch Metadata Request Header do trình duyệt tự động thêm vào mỗi request.
- Nó cho biết điểm đến (destination) của request, tức là dữ liệu fetch sẽ được sử dụng ở đâu trong trình duyệt (document, image, script, video, worker, v.v.).

--> Điều này giúp server phân biệt:

- Request để tải HTML document?
- Request để tải ảnh, script, style?
- Hay request để chạy worker, WebSocket, hay video/audio?

#### 2. Các giá trị (Directives)

- **`document`**
  - Request cho HTML/XML document, thường do điều hướng (navigation).
  - Ví dụ: Người dùng click link hoặc nhập URL.  
  - Thường đi kèm:
```
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
```

- **`image`**
  - Request ảnh (HTML <img>, CSS background, SVG image...).
  - Ví dụ:
```
<img src="https://example.com/logo.png">
```

- **`script`**

  - Request JavaScript file.
  - Ví dụ:
```
<script src="/app.js"></script>
```

- **`style`**

  - Request CSS stylesheet.
  - Ví dụ:
```
<link rel="stylesheet" href="/main.css">
```

- **`font`**

  - Request font (CSS @font-face).

- **`audio / video`**

  - Request media file cho <audio> hoặc <video>.

- **`iframe / frame / embed / object`**

  - Request nội dung nhúng trong trang (iframe, object tag, v.v.).

- **`worker / sharedworker / serviceworker`**

  - Request script để tạo web worker, shared worker hoặc service worker.

- **`manifest`**

  - Request manifest file (<link rel="manifest">).

- **`empty`**

  - Request không có điểm đến cụ thể (dùng cho fetch(), XHR, WebSocket, sendBeacon...).

- **`webidentity`**

  - Request liên quan đến xác thực danh tính (FedCM API).

--> Nếu có giá trị không nằm trong danh sách chuẩn → server nên bỏ qua, không tin tưởng.

#### 3. Hoạt động.

- Trình duyệt sẽ dựa vào cách request được tạo ra trong DOM / API để gán giá trị Sec-Fetch-Dest.
- Ví dụ:
  - ```<img> → image.```
  - ```<script> → script.```
  - ```fetch('/api/data')``` → ```empty.```
- Header này luôn đi kèm với Sec-Fetch-Mode và Sec-Fetch-Site để mô tả ngữ cảnh đầy đủ của request.

#### 4. Mục đích.
- Cho phép server xác minh loại request có hợp lệ cho endpoint không:
  - Endpoint ```/profile``` chỉ phục vụ HTML → chỉ chấp nhận ```document.```
  - Endpoint ```/api/data``` chỉ phục vụ JSON → chỉ chấp nhận ```empty.```
  - Endpoint ```/logo.png``` chỉ là ảnh → chỉ chấp nhận ```image.```
- Giúp ngăn chặn tấn công:
  - **CSRF nâng cao** (attacker nhúng endpoint nhạy cảm vào ```<img>``` hoặc ```<script>```).
  - **XSSI** (Cross-Site Script Inclusion).
  - **Resource abuse** (truy cập sai loại tài nguyên).
#### 5. Sử dụng.

- Server-side filter dựa trên ```Sec-Fetch-Dest```:
  - Chỉ cho phép ```document``` đối với route trả về HTML.
  - Từ chối (```403 Forbidden```) nếu request API có ```Sec-Fetch-Dest```: ```image``` hoặc ```script```.
  - Với file media (ảnh, video, audio) → chỉ chấp nhận đúng loại tương ứng.

- Kết hợp với:

  - ```Sec-Fetch-Site``` → biết nguồn gốc (same-origin, cross-site).
  - ```Sec-Fetch-Mode``` → biết cách request được tạo ra (navigate, cors, no-cors).
