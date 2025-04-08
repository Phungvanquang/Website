# Content Length


# Content Security Policy
# I.Tổng quan.
## 1. khái niệm.
- Content-Length chỉ định kích thước dữ liệu trong phần thân của tin nhắn HTTP. Trình duyệt tự động thêm tiêu đề Content-Length và Content-Type dựa trên kích thước và loại dữ liệu được gửi đến máy chủ. Để truyền thủ công tiêu đề Content-Length đến máy chủ, bạn cần thêm tiêu đề Content-Length: [length] và Content-Type: [mime type] vào yêu cầu của mình, tiêu đề này mô tả kích thước và loại dữ liệu trong phần thân của yêu cầu POST. Độ dài dữ liệu phải được chỉ định bằng byte. Trong Ví dụ về tiêu đề Content-Length này, chúng tôi tạo một yêu cầu POST đến URL echo ReqBin và gửi dữ liệu đến máy chủ trong phần thân của tin nhắn POST.
## 2. chức năng.
- Cho biết kích thước phần thân: Chức năng chính của Content-Length là chỉ ra kích thước chính xác (tính bằng byte) của phần nội dung (message body) đang được truyền đi trong một yêu cầu hoặc phản hồi HTTP.
- Đảm bảo truyền dữ liệu đầy đủ: Bằng cách biết trước kích thước dự kiến, bên nhận (máy khách hoặc máy chủ) có thể kiểm tra xem tất cả dữ liệu đã được nhận đầy đủ hay chưa. Điều này giúp phát hiện các trường hợp truyền dữ liệu bị lỗi hoặc chưa hoàn tất.
- Hỗ trợ xử lý dữ liệu hiệu quả: Bên nhận có thể phân bổ tài nguyên cần thiết (ví dụ: kích thước bộ đệm) để xử lý dữ liệu đến một cách hiệu quả, vì đã biết trước kích thước chính xác của nó.
- Hỗ trợ kết nối liên tục (persistent connections): Đối với các kết nối HTTP liên tục (nơi nhiều yêu cầu và phản hồi có thể được gửi qua cùng một kết nối TCP), Content-Length giúp phân định ranh giới giữa các thông điệp, cho biết đâu là kết thúc của thông điệp này và đâu là bắt đầu của thông điệp tiếp theo.
- Được sử dụng trong nhiều phương thức HTTP: Mặc dù thường được liên kết với các phản hồi chứa nội dung (ví dụ: trong yêu cầu GET để lấy nội dung hoặc yêu cầu POST với dữ liệu), Content-Length cũng có thể xuất hiện trong các yêu cầu, đặc biệt là đối với các phương thức như POST hoặc PUT khi dữ liệu đang được gửi lên máy chủ. Giá trị Content-Length bằng 0 cho biết phần thân của thông điệp là rỗng.
## 3.hoạt động.
- 1. Bên gửi (Máy chủ hoặc Máy khách) Tính toán:
  + Trước khi gửi thông điệp HTTP có phần thân, bên gửi sẽ tính toán tổng kích thước của dữ liệu trong phần thân (tính bằng byte).
  + Việc tính toán này chỉ bao gồm nội dung thực tế được gửi đi, không bao gồm các header HTTP.
- 2. Thiết lập Header:
  + Bên gửi thêm header Content-Length vào phần header của thông điệp HTTP.
  + Giá trị của header này là kích thước đã tính toán (tính bằng byte).
Ví dụ: Content-Length: 1234 cho biết phần thân của thông điệp chứa 1234 byte.
- 3. Bên nhận (Máy khách hoặc Máy chủ) Xử lý:
  + Khi bên nhận nhận được thông điệp HTTP, nó sẽ đọc header Content-Length.
  + Lúc này, bên nhận biết chính xác số byte mà nó cần phải nhận trong phần thân của thông điệp.
  + Bên nhận tiếp tục đọc dữ liệu từ kết nối cho đến khi nhận đủ số byte đã được chỉ định trong header Content-Length.
  + Điều này đảm bảo rằng toàn bộ phần thân của thông điệp đã được nhận.
- Lưu ý quan trọng:
  + Tính chính xác là then chốt: Giá trị của header Content-Length phải phản ánh chính xác kích thước thực tế của phần thân thông điệp. Sai lệch có thể dẫn đến lỗi và việc truyền dữ liệu không đầy đủ.
  + Không được sử dụng với Transfer-Encoding: chunked: Khi header Transfer-Encoding được đặt thành chunked, header Content-Length thường sẽ bị bỏ qua. Trong mã hóa chunked, phần thân của thông điệp được gửi dưới dạng một chuỗi các "chunk" (khối dữ liệu), mỗi chunk có kích thước riêng, và việc truyền kết thúc được đánh dấu bằng một chunk có kích thước bằng 0.
  + Xử lý tự động: Trong nhiều máy chủ web và thư viện máy khách, header Content-Length thường được xử lý tự động dựa trên dữ liệu đang được gửi đi. Tuy nhiên, trong một số trường hợp, đặc biệt khi làm việc với các luồng dữ liệu (streams) hoặc lập trình socket thủ công, nhà phát triển có thể cần phải thiết lập header này một cách явное (explicitly).

### Tài Liệu tham khảo.
