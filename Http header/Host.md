# Host

# I.Tổng quan.
## 1. Khái niệm.
- Host : là 1 header http sử dụng để nhận biết đích của request đó point.
- cú pháp : Host: ```<host>:<port>```
+ ```<host>``` : Tên miền của máy chủ (dành cho lưu trữ ảo).
+ ```<port>``` : Không bắt buộc,Số cổng TCP mà máy chủ đang lắng nghe.
## 2. Chức năng.
- Xác định máy chủ: Header "Host" chỉ định tên miền (domain name) hoặc địa chỉ IP của máy chủ web mà yêu cầu HTTP được gửi đến.
- Phân biệt các website trên cùng máy chủ: Một máy chủ web có thể lưu trữ nhiều website khác nhau. Header "Host" giúp máy chủ phân biệt được yêu cầu đó dành cho website nào.
- Hỗ trợ Virtual Hosting: Trong môi trường Virtual Hosting, nhiều website sử dụng chung một địa chỉ IP. Header "Host" là yếu tố then chốt để máy chủ định tuyến yêu cầu đến đúng website.
## 3. Hoạt động.
- Khi trình duyệt hoặc ứng dụng khách gửi yêu cầu HTTP, nó sẽ bao gồm header "Host" trong yêu cầu đó.
- Máy chủ web nhận được yêu cầu, đọc header "Host" và sử dụng thông tin này để xác định website cần xử lý yêu cầu.
- Máy chủ sau đó xử lý yêu cầu và gửi phản hồi lại cho trình duyệt hoặc ứng dụng khách.
## 4. Mục đích.
- Mục đích của tiêu đề HTTP Host là giúp xác định thành phần back-end nào mà máy khách muốn giao tiếp. Nếu các yêu cầu không chứa tiêu đề Host hoặc nếu tiêu đề Host bị định dạng sai theo một cách nào đó, điều này có thể dẫn đến sự cố khi định tuyến các yêu cầu đến ứng dụng dự định.
- 
## 5. Những cách thao túng.
![image](https://github.com/user-attachments/assets/5b32b046-d715-418e-ab47-802567e293d8)

\- phương pháp cấp cao để xác định các trang web dễ bị tấn công tiêu đề HTTP Host và trình bày cách bạn có thể khai thác điều này cho các loại tấn công sau:
+ Đặt lại mật khẩu đầu độc LABS
+ Đầu độc bộ nhớ đệm web LABS
+ Khai thác các lỗ hổng cổ điển ở phía máy chủ
+ Bỏ qua xác thực LABS
+ Tấn công máy chủ ảo bằng cách dùng vũ lực
+ SSRF LABS dựa trên định tuyến
+ Tấn công trạng thái kết nối LABS****
