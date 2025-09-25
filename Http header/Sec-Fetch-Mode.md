# Sec-Fetch-Mode

#### 1. Khái niệm.
- `Sec-Fetch-Mode` là một Fetch Metadata Request Header do trình duyệt thêm vào mỗi request. Nó cho biết “chế độ (mode) của request”, tức là cách mà request đó được khởi tạo (navigation, cors, no-cors, v.v.).

--> Điều này giúp server phân biệt request
- Có phải request để tải trang (navigation)?
- Hay request để tải resource (ảnh, script)?
- Hay là kết nối đặc biệt (WebSocket)?
- Hay là CORS request?

#### 2. Các giá trị (Directives)
- **`navigate`**
  - Request được tạo ra do người dùng điều hướng (ví dụ click link, nhập URL, load HTML document).
  - Thường đi kèm với:
```
Sec-Fetch-Dest: document
Sec-Fetch-User: ?1
```

- **`cors`**
  - Request tuân theo cơ chế CORS (Cross-Origin Resource Sharing).
  - Ví dụ: Fetch API, XMLHttpRequest cross-origin.
  - Server cần CORS headers (Access-Control-Allow-Origin) để chấp nhận.

- **`no-cors`**
  - Request kiểu “no-cors”, thường dùng khi tải ảnh, video, script từ site khác mà không cần quyền CORS.
  - Ví dụ:
```
<img src="https://evil.com/x.png">
```

- **`same-origin`**
  - Request đến từ cùng origin và không vượt qua boundary của CORS.
  - Ví dụ: ```fetch('/api/data')``` từ cùng domain.

- **`websocket`**
  - Request nhằm thiết lập WebSocket connection (ws:// hoặc wss://).
  - Ví dụ: new WebSocket("wss://mysite.com/socket").
    
--> Nếu trình duyệt gửi giá trị ngoài các cái trên → server nên bỏ qua (không tin tưởng).

#### 3. Hoạt động.

- Trình duyệt tự động thêm Sec-Fetch-Mode vào mỗi request HTTP.
- Nó biểu thị cách mà request được tạo ra (navigation, cors, no-cors, same-origin, websocket).
- Luôn đi kèm với các header khác như Sec-Fetch-Site, Sec-Fetch-Dest để mô tả ngữ cảnh đầy đủ của request.
  
#### 4. Mục đích.
- Giúp server nhận diện loại request:
  - Người dùng điều hướng (navigate).
  - Tải tài nguyên tĩnh (no-cors).
  - Gọi API có CORS (cors).
  - WebSocket handshake (websocket).
- Từ đó server có thể ra quyết định bảo mật: cho phép, từ chối, hoặc áp dụng cơ chế phòng thủ.
- Là một phần của cơ chế Fetch Metadata Request Headers → giảm thiểu tấn công CSRF, XSSI, data exfiltration
#### 5. Sử dụng.
- Server-side kiểm tra giá trị header trước khi xử lý request.

- Ví dụ:

  - Chỉ cho phép navigate hoặc **same-origin** với các endpoint nhạy cảm.
  - Từ chối (**403 Forbidden**) nếu phát hiện request **no-cors cross-site**.
  - Cho phép **websocket** chỉ tại endpoint WebSocket.

- Dùng kết hợp với:
  - ```Sec-Fetch-Site``` → để biết nguồn gốc request.
  - ```Sec-Fetch-Dest``` → để biết tài nguyên đích.
