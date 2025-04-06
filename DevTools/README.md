# Chrome DevTools

# I. Tổng quan.
## 1. khái niệm.
- Chrome DevTools là một bộ công cụ phát triển web được tích hợp trực tiếp vào trình duyệt Google Chrome. DevTools có thể giúp bạn chẩn đoán vấn đề một cách nhanh chóng, điều này giúp bạn xây dựng trang web tốt hơn, nhanh hơn. Với DevTools, bạn có thể xem và thay đổi bất kỳ trang nào, ngay cả trang chủ Google.
## 2. lí do.
- ``Xem và thay đổi kiểu của trang`` : code một vài CSS và sau đó xem trang của mình hiển thị ra sao, các kiểu đang không được phản ánh. Hướng dẫn này cho bạn thấy cách sử dụng DevTools để xem cách trình duyệt thực sự có áp dụng css cho các phần tử HTML hay không. Nó cũng cho bạn thấy làm thế nào để thay đổi css từ DevTools, áp dụng các thay đổi ngay lập tức mà không cần phải tải lại trang.

- ``Debug JavaScript`` :  Cách thức đầu tiên mà hầu hết các developer học cách debug là rắc lệnh console.log () trong suốt mã của họ, để suy ra nơi mã đang đi sai. Hướng dẫn này cho bạn thấy cách thiết lập các điểm ngắt trong DevTools, cho phép bạn tạm dừng ở giữa việc thực thi một trang và từng bước qua code một dòng tại một thời điểm. Trong khi bạn bị tạm dừng, bạn có thể kiểm tra (và thậm chí thay đổi) các giá trị hiện tại của các biến tại thời điểm đó. Bạn có thể thấy rằng luồng công việc này giúp bạn debug các vấn đề nhanh hơn nhiều so với phương thức console.log ().

- ``Xem tin nhắn và chạy JavaScript trong Console`` : Bảng điều khiển cung cấp nhật ký theo thời gian của các tin nhắn cung cấp cho bạn thêm thông tin về việc liệu trang có đang chạy chính xác hay không. Những thông điệp này đến từ các developer, người đã xây dựng trang, hoặc từ trình duyệt. Bạn cũng có thể chạy JavaScript từ Console để kiểm tra cách trang được tạo hoặc thử nghiệm thay đổi cách trang chạy.

- ``Phân tích hiệu suất thời gian chạy`` : Nếu trang của bạn chậm, bạn có thể sử dụng DevTools để ghi lại mọi thứ xảy ra trên trang và sau đó phân tích kết quả để biết cách tối ưu hóa hiệu suất của trang.
## 3. Phương pháp sử dụng Chrome DevTools:
- debug JavaScript:
    + Sử dụng tab "Sources" để đặt điểm dừng và theo dõi luồng thực thi.
    + Sử dụng bảng điều khiển "Console" để ghi lại các giá trị biến và thông báo.
- Kiểm tra CSS:
    + Sử dụng tab "Elements" để xem và chỉnh sửa CSS trực tiếp.
    + Kiểm tra các kiểu đã tính toán và mô hình hộp.
- Phân tích hiệu suất:
    + Sử dụng tab "Performance" để ghi lại và phân tích thời gian tải trang.
    + Xác định các tài nguyên chậm và tối ưu hóa chúng.
- Kiểm tra mạng:
    + Sử dụng tab "Network" để theo dõi các yêu cầu và phản hồi.
    + Kiểm tra thời gian tải tài nguyên và các mã trạng thái HTTP.
- Kiểm tra tính đáp ứng:
    + Sử dụng chế độ thiết bị để mô phỏng các thiết bị di động khác nhau.
    + Kiểm tra bố cục và khả năng sử dụng trên các màn hình khác nhau.
## 4 bật DevTools.
- Windows/Linux:
    + Ctrl + Shift + I: Mở DevTools.
    + Ctrl + Shift + J: Mở DevTools và tập trung vào bảng điều khiển Console.
    + Ctrl + Shift + C: Mở DevTools và chọn phần tử.
F12: Mở DevTools.
- macOS:
    + Cmd + Option + I: Mở DevTools.
    + Cmd + Option + J: Mở DevTools và tập trung vào bảng điều khiển Console.
    + Cmd + Shift + C: Mở DevTools và chọn phần tử.
- Nhấp chuột phải (Inspect Element):
    + Nhấp chuột phải vào bất kỳ phần tử nào trên trang web.
    + Chọn "Kiểm tra" (Inspect) hoặc "Kiểm tra phần tử" (Inspect Element). Điều này sẽ mở DevTools và chọn phần tử bạn đã nhấp chuột phải.
- Vào dấu 3 chấm bền góc trên màn hình --> More tools --> Developer tools.
![image](https://github.com/user-attachments/assets/b4ea4ba4-6f4c-4ba7-8b24-4da40347cef5)
## 5. Sử dụng các tab:
- [Elements (Phần tử):](https://github.com/Phungvanquang/Website/tree/main/DevTools/Elements)
    + Xem và chỉnh sửa HTML và CSS.
    + Kiểm tra cấu trúc DOM của trang web.
- [Console (Bảng điều khiển):](https://github.com/Phungvanquang/Website/tree/main/DevTools/Console)
    + Xem các thông báo lỗi và cảnh báo JavaScript.
    + Chạy các lệnh JavaScript trực tiếp.
- [Sources (Nguồn):](https://github.com/Phungvanquang/Website/tree/main/DevTools/Sources)
    + Xem và gỡ lỗi mã JavaScript.
    + Đặt điểm dừng và theo dõi luồng thực thi.
- [Network (Mạng):](https://github.com/Phungvanquang/Website/tree/main/DevTools/Network)
    + Kiểm tra các yêu cầu mạng và phản hồi.
    + Phân tích thời gian tải và hiệu suất mạng.
- [Performance (Hiệu suất):](https://github.com/Phungvanquang/Website/tree/main/DevTools/Performance)
    + Ghi lại và phân tích hiệu suất tải trang.
    + Xác định các vấn đề về hiệu suất và tối ưu hóa.
- [Application (Ứng dụng):](https://github.com/Phungvanquang/Website/tree/main/DevTools/Application)
    + Kiểm tra bộ nhớ cục bộ, cookie và bộ nhớ đệm.
    + Gỡ lỗi các ứng dụng web tiến bộ (PWA).
- [Security (Bảo mật):](https://github.com/Phungvanquang/Website/tree/main/DevTools/Security)
    + Kiểm tra tính bảo mật của website như https, chứng chỉ bảo mật.
- [Lighthouse:](https://github.com/Phungvanquang/Website/tree/main/DevTools/Lighthouse)
    + Đánh giá hiệu suất, khả năng truy cập, các phương pháp hay nhất và SEO của trang web.
- [Recorder:](https://github.com/Phungvanquang/Website/tree/main/DevTools/Recorder)
    + Recorder là một công cụ cho phép bạn ghi lại và phát lại các tương tác của người dùng trên trang web. Điều này rất hữu ích cho việc.

### Tài liệu tham khảo.
- https://viblo.asia/p/tim-hieu-ve-chrome-devtools-phan-1-3Q75wpqeKWb
- https://developer.chrome.com/docs/devtools?hl=vi
- https://github.com/ChromeDevTools/devtools-frontend
- https://200lab.io/blog/gioi-thieu-cong-cu-devtool-trong-cac-trinh-duyet
- https://developer.chrome.com/docs/devtools/tips
