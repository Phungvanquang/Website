# Web Application Firewall.

## 1. Khái niệm.
- WAF (Web Application Firewall) là 1 lớp bảo mật đặt giữa cient (người dùng) và web application (website hoặc API) để lọc, giám sát và chặn các request HTTP/HTTPS nguy hiểm.
- Khác với Firewall truyền thống (Network Firewall) chỉ lọc theo IP, Port, Protocol, thì Waf đi sau vào tầng Application (layer 7 trong mô hình OSI) để phân tích nội dung request như :
  - URL.
  - Header.
  - Body.
  - Query String.

## 2. Phân loại.

### 2.1. Network-based WAF.
- Triển khai như phần cứng (appliance) hoặc VM appliance.
- trực tiếp trong data center, gần web server.
### 2.2. Host-based WAF.
- Cài đặt trực tiếp trên server.
- Tích hợp với framework / ngôn ngữ : PHP, java, Node.js, Nginx, Apache.
### 2.3. Cloud-based WAF (SaaS).
- WAF chạy trên cloud, traffic được redirect qua nhà cung cấp (reverse proxy).
  
## 3. Hoạt động.
**Quy trình :**
1. Client gửi request :
ví dụ : `GET /login?username=admin' OR '1'='1&password=...`
2. Reuquest đi qua WAF.
- WAF phân tích request theo rule/policy.
- Nếu request chứa pattern tấn công (Ví dụ : `' OR 1=1--` trong query string) --> chặn ngay.
- Nếu request hợp lệ --> chuyển tiếp đến web server.
3. Response từ web server --> cũng đi qua WAF trước khi trả về client (để lọc leak dữ liệu nhạy cảm, header không an toàn).

## 4. chức năng.
- Ngăn chặn SQL injection, XSS, RCE... bằng signature/rule.
- Rate limiting & DDos mitigation (chặn flood request).
- Geo-blocking (chặn theo quốc gia).
- Bot management (chặn crawler/automation không hợp lệ).
- Virtual patching (chặn lỗ hổng trước khi dev fix code).
- Data leak Prevention (ẩn thông tin nhạy cảm như số thẻ tín dụng, error stack trace).

## 5. Các hãng cung cấp WAF.
### 5.1. Hardware / Appliance WAF.
- F5 Networks (F5 ASM / Advanced WAF)>
- Impreva SecureSphere.
- Citrix Netscaler.
- Barracuda WAF.
## 5.2. Cloud WAF (SaaS).
- Cloudflare WAF.
- Akamai kona Site Defender.
- AWS WAF.
- Google cloud Armor (WAF).
## 5.3. Open- Source WAF.
- ModSecurity (OWasp CRS).
- NAXSI (nginx Anti XSS & SQL injection).
- OpenResty lua WAF.
  
