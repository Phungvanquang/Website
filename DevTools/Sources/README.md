# Sources

#### 1. Khái niệm.
- Tab Source trong Devtools :
  - xem toàn bộ mã nguồn javascript, css, html mà trình duyệt đã tải từ server.
  - chạy debug JS ( đặt breakpoint, step-by-step).
  - chỉnh sửa code trực tiếp.
  - lưu lại chỉnh sửa để test lâu dài.
  - xem các file JS ẩn / không được link công khai.
  - tìm API endpoint, token hoặc key hardcode.
  - phân tích login client-side.
  - Debug để hiểu cách app xử lí dữ liệu.
#### 2. Cấu trúc. 

Source tab thường có 3 phần chính : 

###### 1. File Navigator (bên trái).
- chứa tất cả file mà trang đã load, bao gồm :
  - top/ (domain chính).
  - CDN hoặc external JS.
  - Extensions (nếu liên quan).
###### 2. Editor (giữa).
- Nơi hiển thị và chỉnh sửa mã nguồn.
- có thể đặt breakpoint vào dòng code để debug.
###### 3. Debugging Tools (bên phải).
- Call back :xem luồng gọi hàm.
- Scope Variable : biến hiện tại trong function.a
- Watch Expressions: theo dõi biến tùy chọn.
- Breakpoints: quản lý điểm dừng.
- XHR/Fetch Breakpoints: dừng khi gửi request.
- DOM Breakpoints: dừng khi element thay đổi.
- Event Listener Breakpoints: dừng khi sự kiện được kích hoạt.


#### 3. Các Kỹ thuật dùng Source.

###### 3.1. Tìm API và Endpoint

Mở file .js → tìm chuỗi như "api", "token", "auth", "key".

Dùng Ctrl+Shift+F (Search all files) để tìm toàn bộ domain endpoint.

Rất hữu ích cho API reconnaissance.

###### 3.2. Bỏ qua Validation Client-side

Đặt breakpoint ở hàm validate form.

Chạy form → khi breakpoint kích hoạt → sửa giá trị biến rồi tiếp tục.

Có thể bypass captcha hoặc field validation.

###### 3.3. Hook và Theo dõi Request

Đặt XHR/Fetch Breakpoint để chặn mọi request gửi đi.

Ví dụ: chặn /updateUser → chỉnh payload → tiếp tục chạy.

###### 3.4. Phân tích mã bị minify

Mã JS thường được minify → dùng nút {} (Pretty Print) để format lại.

Từ đây pentester có thể đọc logic dễ hơn.

###### 3.5. Ghi đè (Local Overrides)

Dùng tính năng Overrides để chỉnh file JS/CSS và lưu vào máy → mỗi lần reload trang sẽ dùng bản chỉnh sửa.

Ví dụ: sửa JS để bỏ kiểm tra quyền admin trên client.

#### 4. Thực hành.

Ví dụ: Một web app kiểm tra quyền admin ở client trước khi gửi request:

if (userRole === 'admin') {
    fetch('/admin/data')
}

```
➡ Pentester mở Sources tab → tìm file JS → sửa if (userRole === 'admin') thành if (true) → lưu Overrides → reload → truy cập dữ liệu admin.
```
