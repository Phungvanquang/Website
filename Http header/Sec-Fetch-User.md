# Sec-Fetch-User

#### 1. Khái niệm.
- ```Sec-Fetch-User``` là một Fetch Metadata Request Header do trình duyệt tự động thêm vào navigation request.
- Header này chỉ xuất hiện khi request được khởi tạo bởi hành động trực tiếp của người dùng (user activation) – ví dụ: người dùng click vào link, submit form, gõ URL và Enter.
- Giá trị của nó luôn là ?1.
- Nếu request không do người dùng trực tiếp kích hoạt (ví dụ tải ảnh, script, request nền của JavaScript) → trình duyệt sẽ bỏ qua header này.

#### 2. Các giá trị (Directives).

- ```?1```.
  - Nghĩa là request này được kích hoạt bởi hành động trực tiếp của người dùng.
  - Không có giá trị nào khác ngoài ?1.

#### 3. Hoạt động.
- Trình duyệt chỉ thêm Sec-Fetch-User: ?1 khi:
  - Người dùng tương tác rõ ràng (click, Enter, submit).
  - Request là navigation request (chuyển hướng sang document mới, iframe mới, v.v.).
- Nếu request là:
  - Tự động (redirect, load script, image, API fetch) → header sẽ không xuất hiện.
  - Do JavaScript tạo mà không có hành động người dùng → không có header này.

#### 4. Mục đích.
- Giúp server xác định xem request điều hướng có phải do người dùng trực tiếp khởi tạo không.
- Dùng để tăng cường bảo mật chống CSRF và clickjacking:
  - Nếu request không có Sec-Fetch-User, có thể nghi ngờ rằng request không phải do người dùng chủ động.
  - Server có thể yêu cầu xác thực thêm (ví dụ CSRF token) hoặc từ chối request.
- Là một phần trong Fetch Metadata Request Headers giúp nhận diện ngữ cảnh request toàn diện.
#### 5. Sử dụng.
- Trên server-side, kiểm tra header này để quyết định cho phép hay từ chối request.

Ví dụ:

- Trường hợp hợp lệ (người dùng click link):
  
```
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
```

- Ứng dụng thực tế trong bảo mật:
  - Chỉ xử lý request navigate đến trang nhạy cảm (```như /account/delete```) nếu có Sec-Fetch-User: ```?1.```
  - Nếu thiếu header này → từ chối request (giảm thiểu nguy cơ tấn công từ script ẩn, iframe ẩn).
- Kết hợp với các header khác:
  - ```Sec-Fetch-Site``` → biết nguồn gốc request (same-origin, cross-site).
  - ```Sec-Fetch-Mode``` → biết chế độ request (navigate, cors, no-cors).
  - ```Sec-Fetch-Dest``` → biết đích của request (document, image, script).
