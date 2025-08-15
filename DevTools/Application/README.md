# Application



#### **1. Concept (Khái niệm)**

**Application tab** trong DevTools là nơi quản lý và phân tích toàn bộ dữ liệu **lưu trữ trên trình duyệt** và các **thành phần ứng dụng web** như service workers, manifest, cache, cookies.

* **Developer**: Debug lưu trữ dữ liệu (LocalStorage, SessionStorage, IndexedDB, Cache API), quản lý service worker, PWA.
* **Pentester**: Thu thập dữ liệu nhạy cảm, kiểm tra cơ chế lưu trữ, phát hiện token/API key, khai thác session.

#### **2. Structure (Cấu trúc)**

Khi mở **Application tab**, bạn sẽ thấy:

##### **2.1. Storage (lưu trữ)**

* **Local Storage** → Lưu dữ liệu key-value bền vững.
* **Session Storage** → Key-value, hết khi đóng tab.
* **IndexedDB** → CSDL NoSQL trong trình duyệt.
* **Web SQL** (deprecated) → SQLite trong browser.
* **Cookies** → Dữ liệu session, token, prefs.

##### **2.2. Cache**

* **Cache Storage** → Dữ liệu cache từ Service Worker.
* **Application Cache** (cũ, deprecated).

##### **2.3. Background Services**

* **Service Workers** → Script chạy nền.
* **Background Fetch / Sync / Push** → Gửi/nhận dữ liệu khi app không active.

##### **2.4. Frames**

* Danh sách các iframe và storage liên quan.

##### **2.5. Manifest**

* File JSON mô tả PWA (tên app, icon, theme…).
  
#### **3. Techniques using Application**

##### **3.1. Developer-Oriented**

* Kiểm tra **PWA manifest** và service worker.
* Debug **IndexedDB** để test lưu dữ liệu offline.
* Xóa cache để tải bản mới của web app.
* Xem storage khi test form → kiểm tra dữ liệu có lưu không.

##### **3.2. Pentester-Oriented**

* **Token Hunting**:

  * LocalStorage / SessionStorage → tìm JWT, API keys.
  * Cookies → tìm `HttpOnly` bị thiếu, `Secure` bị thiếu.
* **Session Hijacking**: Copy giá trị session ID từ Cookies để login vào account khác.
* **Auth Bypass**:

  * Sửa giá trị role trong LocalStorage (`role=admin`).
  * Nếu web chỉ dựa vào client-side → bypass được quyền.
* **Service Worker Exploitation**:

  * Kiểm tra script có chứa logic nhạy cảm hoặc cached response.
* **Offline Data Leak**:

  * Kiểm tra IndexedDB / Cache Storage để tìm dữ liệu nhạy cảm lưu offline.
#### **4. Practice (Thực hành)**

**Tình huống 1 – Token trong LocalStorage**

1. Mở Application → Local Storage → domain web.
2. Thấy key:

   ```
   authToken: eyJhbGciOiJIUzI1...
   ```
3. Copy token → dùng JWT debugger decode → thấy role = "admin".
4. Sửa token và ký lại (nếu secret yếu) → login admin.
   
**Tình huống 2 – Cookie không bảo mật**

1. Mở Application → Cookies.
2. Thấy `sessionid=abc123` nhưng không có **HttpOnly** và **Secure**.
3. Điều này cho phép JS đánh cắp cookie qua XSS hoặc MITM.
**Tình huống 3 – IndexedDB Leak**

1. Application → IndexedDB → mở bảng `users`.
2. Thấy cột password lưu **plain text**.
3. Có thể trích xuất toàn bộ khi có XSS.

#### Pentest Application Tab – 20.
| #                         | Khu vực kiểm tra         | Mục tiêu                          | Cách kiểm tra trong Application tab  | Khai thác tiềm năng               |
| ------------------------- | ------------------------ | --------------------------------- | ------------------------------------ | --------------------------------- |
| **Local Storage**         |                          |                                   |                                      |                                   |
| 1                         | JWT / API Keys           | Tìm token hoặc key nhạy cảm       | Application → Local Storage → domain | Token reuse, privilege escalation |
| 2                         | Role / Permission        | Giá trị quyền user lưu client     | Sửa giá trị `role` = admin           | Bypass phân quyền                 |
| 3                         | Config Data              | Lưu API endpoint hoặc secret      | Đọc config → gọi API ẩn              | API abuse                         |
| 4                         | Sensitive Info           | Email, pass, số thẻ…              | Tìm text/plain                       | Data leak                         |
| **Session Storage**       |                          |                                   |                                      |                                   |
| 5                         | Session Tokens           | Token trong session storage       | XSS → trích token                    | Session hijacking                 |
| 6                         | Temporary Data           | Dữ liệu tạm thời nhạy cảm         | Truy cập key-value                   | Lộ thông tin                      |
| **Cookies**               |                          |                                   |                                      |                                   |
| 7                         | HttpOnly Flag            | Check cột `HttpOnly`              | Nếu thiếu → XSS lấy cookie           |                                   |
| 8                         | Secure Flag              | Check cột `Secure`                | Nếu thiếu → MITM                     |                                   |
| 9                         | SameSite Policy          | Check cột `SameSite`              | Nếu None → CSRF                      |                                   |
| 10                        | SessionID Exposure       | ID dạng dễ đoán                   | Dự đoán/bruteforce                   |                                   |
| **IndexedDB**             |                          |                                   |                                      |                                   |
| 11                        | Sensitive Tables         | Tên bảng chứa user, orders…       | Dump data                            | Lộ thông tin                      |
| 12                        | Plaintext Password       | Tìm trường `password`             | Account takeover                     |                                   |
| 13                        | Offline Cache Data       | Dữ liệu private cache offline     | Phục hồi data                        |                                   |
| **Cache Storage**         |                          |                                   |                                      |                                   |
| 14                        | Stale Data               | File JS/CSS cũ chứa vuln          | Replay vuln                          |                                   |
| 15                        | Sensitive Response Cache | API response chứa data nhạy cảm   | Offline data leak                    |                                   |
| **Service Workers**       |                          |                                   |                                      |                                   |
| 16                        | SW Script Review         | Check nội dung service worker     | Logic bypass, offline attack         |                                   |
| 17                        | Cached API Endpoints     | SW caching API data               | Data leak                            |                                   |
| **Manifest / PWA**        |                          |                                   |                                      |                                   |
| 18                        | Manifest Info            | Tìm URL admin hoặc API            | Recon                                |                                   |
| 19                        | Insecure PWA Config      | start\_url HTTP hoặc không secure | Downgrade attack                     |                                   |
| **Frames / Cross-Domain** |                          |                                   |                                      |                                   |
| 20                        | Cross-Origin Storage     | Iframe khác domain lưu data       | Steal data qua postMessage           |                                   |


