# Security

#### **1. Concept (Khái niệm)**

**Security tab** trong DevTools dùng để phân tích **kết nối HTTPS, chứng chỉ số (TLS/SSL), và cấu hình bảo mật của website**.

* **Developer**: Đảm bảo website dùng HTTPS đúng chuẩn, chứng chỉ hợp lệ, cấu hình TLS mạnh.
* **Pentester**: Phát hiện lỗi cấu hình bảo mật như chứng chỉ hết hạn, TLS yếu, mixed content (HTTP trong HTTPS), hoặc thiếu HSTS.

#### **2. Structure (Cấu trúc)**

Khi mở **Security tab**, bạn sẽ thấy:

##### **2.1. Main Panel**

* **Summary**:

  * *Secure*: Kết nối HTTPS hợp lệ, TLS mạnh, chứng chỉ hợp lệ.
  * *Not Secure*: Có vấn đề về chứng chỉ, mixed content, hoặc kết nối HTTP.
* **View certificate**: Xem chi tiết chứng chỉ SSL/TLS.

##### **2.2. Origins (Nguồn)**

Danh sách các domain mà trang web đang tải tài nguyên từ:

* Hiển thị màu xanh (secure) hoặc đỏ (insecure).
* Có thể bấm vào để xem chi tiết chứng chỉ từng domain.

##### **2.3. Certificate details**

* Issuer: Tổ chức phát hành chứng chỉ.
* Valid from / to: Thời gian hiệu lực.
* Subject: Tên miền hoặc wildcard (\*.example.com).
* Public key info: Thuật toán (RSA/ECDSA), độ dài key.
#### **3. Techniques using Security**

##### **3.1. Developer-Oriented**

* **Check HTTPS**: Đảm bảo mọi resource đều tải qua HTTPS (tránh mixed content).
* **Verify TLS version**: Ít nhất TLS 1.2, ưu tiên TLS 1.3.
* **HSTS**: Đảm bảo bật HTTP Strict Transport Security.
* **OCSP Stapling**: Bật để tăng tốc và bảo mật kiểm tra chứng chỉ.

##### **3.2. Pentester-Oriented**

* **Mixed Content Discovery**:

  * Nếu web HTTPS nhưng load script/image qua HTTP → có thể MITM.
* **Expired/Invalid Certificate**:

  * Phát hiện site đang dùng cert hết hạn hoặc self-signed.
* **Weak TLS / Cipher**:

  * Xem certificate info → phát hiện key yếu (RSA < 2048bit) hoặc TLS 1.0/1.1.
* **Domain Mismatch**:

  * Cert không khớp tên miền → spoofing hoặc phishing.
* **Subresource Hijacking**:

  * Nếu web load JS từ bên thứ 3 qua HTTP → có thể thay đổi nội dung JS.
#### **4. Practice (Thực hành)**
**Tình huống 1 – Pentest Mixed Content**

1. Mở Security tab → thấy thông báo *“This page is secure but contains insecure resources”*.
2. Mở tab **Network** → filter `http://` → tìm file JS hoặc CSS.
3. Dùng MITM để thay đổi file JS → chèn XSS.
**Tình huống 2 – Kiểm tra TLS yếu**

1. Mở Security tab → bấm vào *View certificate*.
2. Xem key size: nếu RSA 1024 hoặc dùng SHA-1 → báo cáo lỗi cấu hình.
**Tình huống 3 – Chứng chỉ hết hạn**

1. Mở Security tab → xem Valid from/to.
2. Nếu cert đã hết hạn → connection có thể bị MITM mà người dùng không biết (nếu browser bỏ qua warning).


#### Pentest Security Tab – 15 Bước
ư
| #  | Mục tiêu kiểm tra                         | Cách kiểm tra trong Security tab                                            | Ý nghĩa/Potential Exploit                               |
| -- | ----------------------------------------- | --------------------------------------------------------------------------- | ------------------------------------------------------- |
| 1  | **HTTPS enforced**                        | Xem trạng thái “Secure” hoặc “Not Secure”                                   | Nếu site vẫn cho HTTP → MITM, downgrade attack          |
| 2  | **HSTS (HTTP Strict Transport Security)** | Dùng DevTools Console: `document.securityPolicy` hoặc check response header | Nếu thiếu → attacker ép HTTP                            |
| 3  | **Mixed Content**                         | Security tab → xem cảnh báo “insecure resources”                            | Script HTTP trong HTTPS → code injection                |
| 4  | **Certificate Validity**                  | Xem `Valid from / to`                                                       | Cert hết hạn → dễ MITM                                  |
| 5  | **Issuer Trust**                          | Xem Issuer trong cert                                                       | Cert tự ký hoặc CA không tin cậy → MITM                 |
| 6  | **Domain Mismatch**                       | So sánh Subject CN/SAN với domain thực                                      | Spoofing/phishing                                       |
| 7  | **Key Length**                            | Certificate info → Public Key Info                                          | RSA < 2048bit hoặc ECDSA < 256bit → brute force khả thi |
| 8  | **Signature Algorithm**                   | Certificate info → xem SHA-1 hoặc MD5                                       | Collisions, forging cert                                |
| 9  | **TLS Version**                           | Security tab → xem negotiated protocol                                      | TLS 1.0/1.1 → dễ bị BEAST/POODLE                        |
| 10 | **OCSP Stapling**                         | Kiểm tra qua certificate details hoặc Wireshark                             | Thiếu → tăng nguy cơ revoked cert bị dùng lại           |
| 11 | **Third-Party Resources Security**        | Origins list → kiểm tra domain bên thứ 3                                    | Nếu không dùng HTTPS → tấn công supply chain            |
| 12 | **Subresource Integrity (SRI)**           | Check HTML script/link tag của bên thứ 3                                    | Nếu thiếu SRI → attacker thay đổi nội dung file         |
| 13 | **Redirect HTTPS**                        | Tải thử HTTP URL của site → check redirect                                  | Nếu không redirect → user bị lừa vào HTTP               |
| 14 | **Insecure Forms**                        | Form action không HTTPS → Security tab cảnh báo                             | MITM đánh cắp dữ liệu form                              |
| 15 | **CORS Misconfiguration**                 | Check response headers khi load cross-origin resource                       | Nếu Access-Control-Allow-Origin: \* → data leak         |

