# Access-Control-Allow-Credentials

### 1. **Khái niệm**

* `Access-Control-Allow-Credentials` là **response header** mà **server trả về** cho browser.
* Nó quyết định xem **trình duyệt có được phép gửi kèm credentials (cookie, session, auth header, TLS cert)** trong **cross-origin request (CORS)** hay không.
* Mặc định, các cross-origin request không gửi credentials vì lý do bảo mật (ngăn CSRF).

### 2. Các giá trị Directives.

```
Access-Control-Allow-Credentials: true
Access-Control-Allow-Origin: https://trusted.com

```

- `true`

  - Giá trị hợp lệ duy nhất.
  - Cho phép credentials trong cross-origin request.
  - Case-sensitive (phải đúng chữ true, không viết hoa, không viết True).

- `(Không có header)`

  - Nếu không cần credentials → bỏ hẳn header này trong response.
  - Không dùng false.

### 2. **Hoạt động**

* Bình thường: khi gọi **cross-origin API**, browser **không gửi credentials** để tránh rủi ro CSRF.
* Nếu client muốn gửi kèm credentials:

  * `fetch(url, { credentials: "include" })`
  * `xhr.withCredentials = true`
  * `EventSource.withCredentials = true`
* Khi đó browser sẽ **kiểm tra response từ server**:

  * Nếu server có header:

    ```
    Access-Control-Allow-Credentials: true
    ```

    → Browser **cho phép gửi và nhận credentials**.
  * Nếu KHÔNG có header này → Browser báo **network error** (dù server có trả response).

Lưu ý:

* Header này chỉ có **giá trị hợp lệ duy nhất**:

  ```
  Access-Control-Allow-Credentials: true
  ```
* Nếu không cần credentials → **bỏ header đi**, KHÔNG được set `false`.


### 3. **Mục đích**

* Kiểm soát bảo mật: tránh việc **website A (kẻ tấn công)** tự ý gọi API của **website B (bank, mail, …)** và dùng cookie session của bạn.
* Giúp server **chủ động** quyết định khi nào chấp nhận **cross-origin request có credentials**.

👉 Khi `ACAC: true` thì server **bắt buộc** phải trả về `Access-Control-Allow-Origin` với **origin cụ thể**, **không được dùng wildcard `*`**.
Ví dụ:

```
Access-Control-Allow-Origin: https://trusted-client.com
Access-Control-Allow-Credentials: true
```

Nếu server viết sai như:

```
Access-Control-Allow-Origin: *
Access-Control-Allow-Credentials: true
```

→ Browser sẽ chặn vì **quá nguy hiểm** (mọi site đều lấy được cookie/session).


### 4. **Sử dụng**

#### 4.1. Client code (fetch API):

```js
fetch("https://api.bank.com/account", {
  credentials: "include"
});
```

#### 4.2. Server response (đúng):

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://banking-app.com
Access-Control-Allow-Credentials: true
Content-Type: application/json
```

→ Browser gửi kèm cookie `session_id=xyz` và cho phép client `banking-app.com` đọc response.


```
// fetch API
fetch("https://api.trusted.com/data", {
  credentials: "include"
});

// XMLHttpRequest
const xhr = new XMLHttpRequest();
xhr.open("GET", "https://api.trusted.com/data");
xhr.withCredentials = true;
xhr.send();

```


