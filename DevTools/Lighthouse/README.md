# Lighthouse

#### **1. Concept (Khái niệm)**

**Lighthouse** là công cụ audit tích hợp trong Chrome DevTools, dùng để phân tích **hiệu suất, khả năng truy cập, SEO, Progressive Web App (PWA)** và tuân thủ **best practices bảo mật**.

* **Developer**: Dùng để tối ưu tốc độ tải, nâng cao trải nghiệm người dùng, và cải thiện SEO.
* **Pentester**: Dùng để rà soát nhanh các điểm yếu bảo mật cơ bản và cấu hình kém an toàn, như HTTP insecure requests, thiếu HTTPS, header yếu, hoặc PWA config lỗi.

#### **2. Structure (Cấu trúc)**

Khi mở **Lighthouse tab**, bạn sẽ thấy:

* **Categories**:

  1. **Performance** → Thời gian tải, render, interactivity.
  2. **Accessibility** → UI/UX cho người khuyết tật.
  3. **Best Practices** → Khuyến nghị bảo mật & coding chuẩn.
  4. **SEO** → Tối ưu cho search engine.
  5. **PWA** → Kiểm tra Progressive Web App.

* **Device type**:

  * *Mobile* hoặc *Desktop*.

* **Options**:

  * Chọn category cần test.
  * Chạy chế độ "Simulated throttling" để giả lập kết nối chậm.

#### **3. Techniques using Lighthouse**

##### **3.1. Developer-Oriented**

* **Performance**:

  * Xem Largest Contentful Paint (LCP), First Input Delay (FID), Cumulative Layout Shift (CLS).
  * Tối ưu ảnh, giảm bundle size, lazy load.
* **Accessibility**:

  * Kiểm tra thẻ alt cho ảnh, contrast màu, semantic HTML.
* **SEO**:

  * Check meta tags, robots.txt, sitemap.xml.
* **PWA**:

  * Manifest hợp lệ, offline support, service worker.

##### **3.2. Pentester-Oriented**

* **Best Practices → Security Checks**:

  1. HTTPS enforced.
  2. No mixed content.
  3. Safe JavaScript libraries (phiên bản mới).
  4. Avoid deprecated APIs.
  5. Secure HTTP headers (X-Content-Type-Options, CSP…).
* **PWA Security**:

  * Check `start_url` và `scope` để tránh open redirect hoặc leak.
* **Recon nhanh**:

  * Xem tất cả endpoint gọi khi load trang.
* **Detect outdated libraries** → CVE scan.

#### **4. Practice (Thực hành)**

**Tình huống 1 – Phát hiện JS library lỗi thời**

1. Mở Lighthouse → tick *Best Practices*.
2. Chạy audit → thấy jQuery 1.8.3.
3. Cross-check CVE → biết có XSS RCE vulnerabilities.

**Tình huống 2 – Mixed Content**

1. Chạy Lighthouse với *Best Practices*.
2. Report báo load image qua HTTP.
3. Pentest → thay ảnh bằng file HTML chứa JS độc hại → stored XSS.

**Tình huống 3 – PWA config insecure**

1. Chạy Lighthouse → PWA.
2. Manifest `start_url` là HTTP.
3. Kết quả: có thể MITM khi load offline mode.


#### Pentest Lighthouse – 25.

| #                  | Category                         | Mục tiêu kiểm tra                  | Cách test trong Lighthouse                        | Khai thác tiềm năng |
| ------------------ | -------------------------------- | ---------------------------------- | ------------------------------------------------- | ------------------- |
| **Performance**    |                                  |                                    |                                                   |                     |
| 1                  | LCP quá cao                      | Xem Largest Contentful Paint > 4s  | Tấn công DoS bằng payload lớn                     |                     |
| 2                  | FID cao                          | First Input Delay > 300ms          | Tấn công user experience                          |                     |
| 3                  | CLS cao                          | Cumulative Layout Shift > 0.25     | Clickjacking qua UI shift                         |                     |
| 4                  | Unused JS/CSS                    | Performance → unused code          | Chèn mã độc vào file chưa dùng                    |                     |
| 5                  | Chưa bật text compression        | No gzip/brotli                     | Payload chèn thêm mà user không để ý              |                     |
| **Accessibility**  |                                  |                                    |                                                   |                     |
| 6                  | Form thiếu label                 | Accessibility warning              | Lừa nhập dữ liệu nhạy cảm                         |                     |
| 7                  | Contrast thấp                    | Low color contrast                 | Che giấu link/phishing                            |                     |
| 8                  | Thiếu role ARIA                  | Missing roles                      | Ẩn nội dung phishing khỏi screen reader detection |                     |
| **Best Practices** |                                  |                                    |                                                   |                     |
| 9                  | HTTPS chưa enforce               | Best Practices → insecure requests | MITM, downgrade attack                            |                     |
| 10                 | Mixed content                    | Warning insecure resources         | Chèn script HTTP để XSS                           |                     |
| 11                 | Outdated JS libs                 | Detect jQuery/Angular cũ           | Khai thác CVE                                     |                     |
| 12                 | Deprec API                       | Sử dụng API cũ                     | Bypass sandbox                                    |                     |
| 13                 | Thiếu CSP header                 | No Content-Security-Policy         | XSS injection                                     |                     |
| 14                 | No X-Content-Type-Options        | Header missing                     | MIME sniffing attack                              |                     |
| 15                 | No X-Frame-Options               | Header missing                     | Clickjacking                                      |                     |
| **SEO**            |                                  |                                    |                                                   |                     |
| 16                 | Thiếu meta description           | SEO audit                          | Fake search preview                               |                     |
| 17                 | Robots.txt cấu hình sai          | SEO audit                          | Crawl endpoint nhạy cảm                           |                     |
| 18                 | Sitemap lộ endpoint              | SEO audit                          | Recon, tìm API hidden                             |                     |
| 19                 | URL dài & query lộ data          | SEO audit                          | Lộ param quan trọng                               |                     |
| **PWA**            |                                  |                                    |                                                   |                     |
| 20                 | Manifest thiếu HTTPS start\_url  | PWA audit                          | MITM offline mode                                 |                     |
| 21                 | Scope quá rộng                   | Manifest scope includes `/`        | Mở rộng vùng tấn công                             |                     |
| 22                 | No service worker caching policy | SW audit                           | Cache poisoning                                   |                     |
| 23                 | SW script insecure fetch         | Review SW fetch code               | Data exfiltration                                 |                     |
| 24                 | Offline page chứa sensitive data | PWA audit                          | Offline data leak                                 |                     |
| 25                 | SW không validate origin         | PWA audit                          | Cache attack cross-site                           |                     |
