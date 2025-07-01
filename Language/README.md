# Language


| Nền tảng / Framework                                | File / URL nhận diện                               | Phổ biến               | Ứng dụng thực tế / Ghi chú                                                                  |
| --------------------------------------------------- | -------------------------------------------------- | ---------------------- | ------------------------------------------------------------------------------------------- |
| **Node.js / Express.js**                            | Không có file mở rộng, URL dạng REST (`/api/user`) | Tăng mạnh              | Dùng nhiều ở startup, app trẻ. Có thể dễ dính lỗi logic, JWT, NoSQLi                        |
| **Python (Flask, Django)**                          | `/admin/`, `.py` đôi khi lộ                        | Vừa                    | Trang nghiên cứu, AI/ML, quản lý nội bộ                                                     |
| **Ruby on Rails**                                   | `.erb`, URL kiểu REST (`/posts/1/edit`)            | Thấp                   | Gặp ở startup, hệ thống kiểu "cũ nhưng mạnh". Có thể có RCE từ YAML hoặc Template injection |
| **Golang (Go web)**                                 | URL API kiểu `/api/v1/xyz`, file `.go` rò rỉ       | Vừa–Thấp               | App backend tốc độ cao, dễ thấy trong Fintech, Cloud                                        |
| **ColdFusion**                                      | `.cfm`, `.cfc`                                     | Rất thấp nhưng vẫn còn | Hệ thống cũ, có nhiều RCE 0day nguy hiểm                                                    |
| **Classic ASP (trước .NET)**                        | `.asp`                                             | Thấp–trung             | Hệ thống rất cũ, dễ khai thác RCE/SQLi                                                      |
| **Laravel / CodeIgniter / Symfony (PHP Framework)** | `?page=`, `routes/web.php`, `.env` rò rỉ           | Cao (trong PHP)        | Framework PHP phổ biến. Có lỗi cấu hình `.env`, deserialization                             |
| **CMS: WordPress / Joomla / Drupal**                | `/wp-login.php`, `/index.php?option=com_`          | Rất cao                | Gặp nhiều trong hệ thống thông tin công cộng, website trường                                |
| **React / Angular / Vue (Frontend SPA)**            | `.js`, XHR, REST API riêng                         | Rất cao                | Tấn công chủ yếu qua API, DOM-based XSS, JWT, CORS                                          |
| **SharePoint / Exchange Web App**                   | `.aspx`, `/_layouts/15/`                           | Trung–cao              | Cơ quan nhà nước, doanh nghiệp, dễ có CVE cực nguy hiểm                                     |
| **SAP NetWeaver**                                   | `.do`, URL lạ                                      | Thấp                   | Hệ thống ERP khủng, nếu gặp là rất nhạy cảm                                                 |
| **Oracle APEX / Forms**                             | `.fmx`, `.jsp`, `wwv_flow`                         | Thấp                   | Gặp ở ngân hàng, hệ thống quản lý nội bộ                                                    |
| **JAVA **                                           | `.jsp, .war, .jar, .jspx`                          | Thấp                   |  lớn	Rất nhiều ở các cơ quan nhà nước (bạn nói bạn thấy rồi mà!)                           |
| **.NET **                                           | `.aspx, .cshtml, .dll, .exe`                       | Thấp                   |  lớn	Rất nhiều ở các cơ quan nhà nước (bạn nói bạn thấy rồi mà!)                           |
