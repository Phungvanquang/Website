# Access-Control-Allow-Methods

#### 1. Khái niệm

* `Access-Control-Allow-Methods` là một **HTTP response header** thuộc nhóm CORS.
* Nó cho browser biết: **những HTTP methods nào được phép sử dụng** khi gọi tài nguyên cross-origin.
* Header này chỉ xuất hiện trong **phản hồi của preflight request (OPTIONS)**.

#### 2. Hoạt động

1. Khi browser muốn gọi API cross-origin với một method không phải safelisted (vd: `PUT`, `DELETE`, `PATCH`), nó sẽ gửi preflight:

   ```http
   OPTIONS /api/resource
   Origin: https://foo.bar.org
   Access-Control-Request-Method: DELETE
   ```

2. Server trả về danh sách methods được chấp nhận:

   ```http
   HTTP/1.1 200 OK
   Access-Control-Allow-Origin: https://foo.bar.org
   Access-Control-Allow-Methods: GET, POST, PUT, DELETE
   ```

3. Nếu method request thực tế **không nằm trong danh sách** → browser sẽ chặn request.


#### 3. Mục đích

* Cho phép server kiểm soát rõ **API nào hỗ trợ method nào** khi gọi cross-origin.
* Tăng tính bảo mật: tránh việc client gửi các request “không mong muốn” như `DELETE`, `PUT` khi server chưa sẵn sàng.
* Bắt buộc phải dùng khi API cần hỗ trợ các method ngoài safelisted.


#### 4. Sử dụng

* Server khai báo danh sách method cho phép:

  ```http
  Access-Control-Allow-Methods: GET, POST, PUT, DELETE
  ```
* Cho phép tất cả method:

  ```http
  Access-Control-Allow-Methods: *
  ```

Lưu ý: `*` chỉ hoạt động với request **không có credentials** (cookie, auth header). Nếu có credentials → `*` sẽ được hiểu như literal `"*"` (không có ý nghĩa wildcard).

#### 5. Directives (các giá trị hợp lệ)

* **`<method>`**

  * Tên của HTTP method mà server cho phép.
  * Có thể liệt kê nhiều method, phân cách bằng dấu phẩy.
  * Ví dụ:

    ```http
    Access-Control-Allow-Methods: GET, POST, PUT, DELETE
    ```
  * Các method **safelisted luôn mặc định cho phép** (kể cả khi không ghi trong header):

    * `GET`
    * `HEAD`
    * `POST`
    * `OPTIONS`
    * `PUT`
    * `PATCH`
    * `DELETE`

* **`*` (wildcard)**

  * Cho phép tất cả method.
  * Chỉ có hiệu lực khi request **không kèm credentials**.
  * Nếu có credentials (cookies, HTTP auth) → `*` không hoạt động như wildcard.

