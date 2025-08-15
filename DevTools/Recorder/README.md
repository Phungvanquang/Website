# Recorder



#### **1. Concept (Khái niệm)**

**Recorder tab** cho phép bạn **ghi lại chuỗi hành động trên trang web** (click, nhập liệu, điều hướng) và **phát lại** hoặc **xuất script** để test tự động.

* **Developer**: Dùng để kiểm thử UI, automation flow, hoặc tạo test script (Puppeteer, Playwright).
* **Pentester**: Dùng để **tự động tái hiện tấn công** (XSS, CSRF, auth bypass), thử brute-force form, hoặc capture step-by-step cho POC.

#### **2. Structure (Cấu trúc)**

Recorder tab gồm:

* **Record button** → bắt đầu ghi lại các thao tác.
* **Step list** → mỗi thao tác (click, type, navigate) được lưu thành 1 step.
* **Replay** → chạy lại toàn bộ flow.
* **Export** → xuất ra script (JavaScript, Puppeteer, JSON).
* **Settings** → chọn device, throttling network.

#### **3. Techniques using Recorder (Pentest Focus)**

##### 3.1. Recon & Automation

* Ghi lại **flow đăng nhập → thao tác quan trọng** để test auth bypass hoặc session fixation.
* Tạo kịch bản truy cập vào trang nhạy cảm với **tài khoản khác** để test phân quyền.

##### 3.2. Payload Injection

* Ghi lại thao tác nhập liệu form → chèn payload XSS, SQLi, LFI.
* Replay nhiều lần với dữ liệu khác để brute-force hoặc fuzzing.

##### 3.3. CSRF & Clickjacking Simulation

* Record flow gửi form → chạy lại script từ domain khác để kiểm tra CSRF.
* Kết hợp với **Network Override** để thử thay đổi request.

##### 3.4. POC & Reporting

* Xuất kịch bản **Puppeteer** → dùng làm bằng chứng tấn công cho báo cáo pentest.
* Chia sẻ file JSON cho đồng đội để chạy lại flow.

#### **4. Practice (Thực hành)**

**Tình huống 1 – CSRF Test**

1. Record thao tác chuyển tiền trong app.
2. Xuất script → chạy từ một trang web attacker để xem server có block không.

**Tình huống 2 – Auth Bypass**

1. Record thao tác vào admin panel với user admin.
2. Logout → đăng nhập user thường.
3. Replay → nếu vào được admin panel → phân quyền sai.

**Tình huống 3 – Stored XSS**

1. Record thao tác nhập bình luận với payload `<script>alert(1)</script>`.
2. Reload page → nếu popup hiện → stored XSS thành công.


#### Pentest Recorder Tab – 15 Điểm.

| #      | Kỹ thuật                  | Mục tiêu                        | Cách thực hiện với Recorder                        | Khai thác tiềm năng                   |
| ------ | ------------------------- | ------------------------------- | -------------------------------------------------- | ------------------------------------- |
| **1**  | Login Flow Capture        | Lưu quy trình đăng nhập         | Record từ trang login → nhập user/pass → submit    | Test brute-force, credential stuffing |
| **2**  | Multi-Role Flow           | So sánh quyền admin & user      | Record flow admin → chạy lại khi login user thường | Auth bypass                           |
| **3**  | Parameter Tampering       | Ghi và chỉnh param URL          | Record → export script → sửa query param           | Price manipulation, IDOR              |
| **4**  | Form Injection            | Payload XSS/SQLi                | Record nhập `<script>alert(1)</script>` vào form   | Stored/Reflected XSS                  |
| **5**  | CSRF Simulation           | Test gửi form từ domain khác    | Record flow → chạy lại script từ attacker page     | CSRF attack                           |
| **6**  | Forced Browsing           | Record truy cập page ẩn         | Chạy lại với role thấp                             | Bypass phân quyền                     |
| **7**  | File Upload Test          | Record upload file              | Replay với file chứa payload                       | RCE, malware upload                   |
| **8**  | Search Injection          | Record thao tác tìm kiếm        | Replay với payload `' OR 1=1 --`                   | SQL Injection                         |
| **9**  | Step Skipping             | Xóa bước trong flow             | Bỏ bước auth → replay                              | Logic flaw                            |
| **10** | Session Fixation          | Record login → thay cookie      | Replay với cookie attacker                         | Session hijacking                     |
| **11** | Rate Limit Test           | Record thao tác gửi OTP         | Replay nhanh nhiều lần                             | OTP brute-force                       |
| **12** | Stored Data Access        | Record truy cập dữ liệu cá nhân | Replay với account khác                            | Data leak                             |
| **13** | Payment Flow Manipulation | Record mua hàng                 | Replay với giá sửa trong request                   | Payment bypass                        |
| **14** | API Recon                 | Record thao tác gọi API         | Export & phân tích endpoint                        | API fuzzing                           |
| **15** | Cache Poison Replay       | Record request bị cache         | Replay với payload độc hại                         | Cache poisoning                       |

