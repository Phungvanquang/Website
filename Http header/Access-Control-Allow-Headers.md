# Access-Control-Allow-Headers

#### 1. Khái niệm

* `Access-Control-Allow-Headers` là một **HTTP response header** thuộc nhóm CORS.
* Nó được server gửi về **trong phản hồi preflight (OPTIONS request)** để cho browser biết:
  → Những HTTP request headers nào được phép sử dụng trong **CORS actual request**.
* Nếu client gửi `Access-Control-Request-Headers` trong preflight → server **bắt buộc** phải phản hồi `Access-Control-Allow-Headers` để liệt kê rõ các header được chấp nhận.


#### 2. Hoạt động

1. Khi browser muốn gửi một request cross-origin có sử dụng các custom headers (VD: `X-Custom-Header`, `Authorization`, …), nó sẽ gửi preflight request:

   ```http
   OPTIONS /api/data
   Origin: https://foo.bar.org
   Access-Control-Request-Method: GET
   Access-Control-Request-Headers: Content-Type, X-Requested-With
   ```

2. Server phản hồi, cho biết nó có chấp nhận các headers đó không:

   ```http
   HTTP/1.1 200 OK
   Access-Control-Allow-Origin: https://foo.bar.org
   Access-Control-Allow-Methods: GET, POST
   Access-Control-Allow-Headers: Content-Type, X-Requested-With
   ```

3. Nếu header nào **không được phép** → browser sẽ chặn request.



#### 3. Mục đích

* Cho phép server kiểm soát **những header nào client được gửi trong CORS request**.
* Ngăn chặn việc client gửi những custom header không mong muốn (tránh tấn công).
* Tạo tính linh hoạt khi API yêu cầu một số headers đặc thù (VD: `Authorization`, `X-API-Key`).



#### 4. Sử dụng

* Trong response từ server:

  ```http
  Access-Control-Allow-Headers: X-Custom-Header
  ```
* Cho phép nhiều header cùng lúc:

  ```http
  Access-Control-Allow-Headers: Content-Type, Authorization, X-Requested-With
  ```
* Nếu muốn cho phép tất cả headers (chỉ an toàn khi **không có credentials**):

  ```http
  Access-Control-Allow-Headers: *
  ```


#### 5. Directives (giá trị hợp lệ)

* **`<header-name>`**

  * Tên một header mà server cho phép client gửi.
  * Có thể liệt kê nhiều header, phân tách bằng dấu phẩy.
  * Ví dụ:

    ```http
    Access-Control-Allow-Headers: Content-Type, X-Requested-With
    ```

* **`*` (wildcard)**

  * Cho phép **mọi header** trong request.
  * Chỉ có hiệu lực khi request **không kèm credentials** (cookie, HTTP auth).
  * Nếu request có credentials → `*` bị coi như literal `"*"` chứ không phải wildcard.

* **Lưu ý đặc biệt với `Authorization`**

  * `Authorization` **không được wildcard bao phủ**.
  * Nếu muốn dùng phải chỉ rõ:

    ```http
    Access-Control-Allow-Headers: Authorization
    ```


