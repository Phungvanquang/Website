# Elements



#### **1. Concept (Khái niệm)**

* **Elements tab** cho phép xem và chỉnh sửa **DOM + CSS** của trang web theo thời gian thực.
* Với **developer**, đây là công cụ debug UI.
* Với **pentester**, nó là **cửa sổ nhìn trực tiếp vào cấu trúc HTML**, giúp:

  * Tìm các input ẩn (hidden fields)
  * Phát hiện comment để lộ thông tin
  * Sửa client-side validation
  * Inject payload ngay trên giao diện

#### **2. Structure (Cấu trúc)**

Elements tab gồm:

* **HTML DOM Tree** → Cấu trúc HTML hiện tại, bao gồm cả DOM đã được JavaScript thay đổi.
* **CSS Styles Pane** → Quy tắc CSS áp dụng cho element.
* **Event Listeners** → Xem event handler gắn vào phần tử.
* **DOM Breakpoints** → Dừng script khi một phần tử bị thay đổi.
* **Properties** → Thuộc tính JavaScript của phần tử.

#### **3. Techniques using Elements tab (Pentest Focus)**

##### 3.1. Recon & Information Disclosure

* Tìm **comment trong HTML** để lộ API key, đường dẫn admin, thông tin dev.
* Xem **hidden fields** trong form để lấy giá trị mặc định hoặc token.
* Kiểm tra **inline script** để tìm JS injection point.

##### 3.2. Manipulating Client-side Logic

* Sửa **`disabled` attribute** của nút → bật chức năng bị khóa.
* Đổi **giá trị hidden input** để bypass giá hoặc role user.
* Xóa **maxlength** để nhập payload dài hơn giới hạn.

##### 3.3. Payload Injection & XSS

* Sửa trực tiếp HTML để thêm `<script>alert(1)</script>` xem CSP có chặn không.
* Inject payload vào thuộc tính `onerror`, `onclick` để test DOM XSS.

##### 3.4. Security Feature Testing

* Tắt các CSS overlay chống copy nội dung để test leak dữ liệu.
* Xóa hoặc thay đổi `Contenteditable` để chèn text giả mạo.

#### **4. Practice (Thực hành)**

**Tình huống 1 – Hidden Admin Field**

1. Mở Elements tab → tìm `<input type="hidden" name="role" value="user">`.
2. Sửa thành `"admin"` → submit form → nếu vào được admin → phân quyền sai.

**Tình huống 2 – Bypass Disabled Button**

1. Nút “Thanh toán” bị `disabled`.
2. Xóa `disabled` trong HTML → click → nếu xử lý thành công → logic phía server yếu.

**Tình huống 3 – Inline Script Injection**

1. Chỉnh HTML của một div để chèn `<img src=x onerror=alert(1)>`.
2. Reload → nếu alert xuất hiện → DOM XSS.
#### entest Elements Tab – 20 Điểm.

| #                    | Nhóm         | Kỹ thuật                                   | Mục tiêu                                   | Khai thác tiềm năng             |
| -------------------- | ------------ | ------------------------------------------ | ------------------------------------------ | ------------------------------- |
| **Recon**            |              |                                            |                                            |                                 |
| 1                    | Recon        | Tìm **HTML comments**                      | Lộ API key, đường dẫn admin, thông tin dev | Recon & exploitation            |
| 2                    | Recon        | Tìm **hidden fields** trong form           | Lộ token, giá trị mặc định                 | Token hijack, price tampering   |
| 3                    | Recon        | Xem inline script trong HTML               | Phát hiện JS injection point               | XSS                             |
| 4                    | Recon        | Tìm thuộc tính `data-*`                    | Chứa dữ liệu nhạy cảm (user ID, role)      | IDOR                            |
| 5                    | Recon        | Xem event listeners                        | Tìm JS handler dễ khai thác                | DOM XSS                         |
| **Manipulation**     |              |                                            |                                            |                                 |
| 6                    | Manipulation | Xóa thuộc tính `disabled`                  | Mở khóa nút chức năng bị chặn              | Logic bypass                    |
| 7                    | Manipulation | Thay đổi giá trị hidden input              | Bypass role, thay đổi giá                  | Auth bypass, price manipulation |
| 8                    | Manipulation | Xóa `readonly` từ input                    | Cho phép sửa dữ liệu cấm sửa               | Data tampering                  |
| 9                    | Manipulation | Xóa `maxlength`                            | Nhập payload dài hơn giới hạn              | Buffer overflow, SQLi           |
| 10                   | Manipulation | Sửa DOM để hiển thị element bị ẩn          | Truy cập form/menu bí mật                  | Feature exposure                |
| **Injection**        |              |                                            |                                            |                                 |
| 11                   | Injection    | Chèn `<script>` vào HTML                   | Test CSP & XSS                             | Stored/Reflected XSS            |
| 12                   | Injection    | Thêm thuộc tính `onerror` / `onclick`      | DOM XSS test                               | DOM-based XSS                   |
| 13                   | Injection    | Sửa link thành `javascript:`               | Script execution                           | Clickjacking + XSS              |
| 14                   | Injection    | Chèn `<iframe>` trỏ tới domain khác        | Phishing, clickjacking                     | UI redress attack               |
| 15                   | Injection    | Chèn payload vào `style`                   | CSS exfiltration                           | Stealing sensitive data         |
| **Security Testing** |              |                                            |                                            |                                 |
| 16                   | Security     | Xóa CSS overlay chống copy                 | Copy nội dung bảo vệ                       | Data leak                       |
| 17                   | Security     | Thêm `contenteditable` vào element         | Chèn nội dung giả                          | Defacement simulation           |
| 18                   | Security     | Kiểm tra thuộc tính `integrity` của script | SRI missing                                | Script tampering                |
| 19                   | Security     | Xóa `rel="noopener"` khỏi link             | Tabnabbing                                 | Phishing tab hijack             |
| 20                   | Security     | Xem form `autocomplete="on"`               | Thu thập dữ liệu nhạy cảm                  | Credential harvesting           |
