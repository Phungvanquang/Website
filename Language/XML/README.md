# XML (Extensible Markup Language)
![image](https://github.com/user-attachments/assets/1aec1ed3-c57c-4e5f-97d1-1a489f389722)


[1. **Giới thiệu chung**]()
- XML là từ viết tắt của từ Extensible Markup Language là ngôn ngữ đánh dấu mở rộng được thiết kế để lưu trữ và truyền tải dữ liệu một cách có cấu trúc. Tác dụng chính của XML là đơn giản hóa việc chia sẻ dữ liệu giữa các nền tảng và các hệ thống được kết nối thông qua mạng Internet.

- Ngôn ngữ XML do Tổ chức World Wide Web Consortium (W3C) đề xuất, được tạo ra để hỗ trợ việc xây dựng các dịch vụ API. XML có khả năng truyền và đọc nhiều loại dữ liệu khác nhau. Kết quả từ API thường được trả về dưới dạng XML, cho phép các hệ thống khác nhau dễ dàng giao tiếp với nhau.
- Lịch sử và sự phát triển của XML.
   + XML, hay Ngôn ngữ đánh dấu mở rộng, ra đời vào giữa những năm 1990 như một tiêu chuẩn phổ quát cho đánh dấu tài liệu có cấu trúc. World Wide Web Consortium (W3C), do Jon Bosak lãnh đạo, đã phát triển XML như một sự đơn giản hóa của Ngôn ngữ đánh dấu tổng quát tiêu chuẩn (SGML). Đặc tả XML 1.0 đã được W3C chấp nhận như một khuyến nghị vào tháng 2 năm 1998, đánh dấu tình trạng chính thức của nó như một tiêu chuẩn web.
   + thiết kế chủ yếu để trình bày tài liệu hơn là để lưu trữ và trao đổi dữ liệu. Hơn nữa, bản chất mở tự do của tiêu chuẩn XML và sự tồn tại của các hệ thống lược đồ khác nhau và giao diện lập trình ứng dụng (API) cho các ngôn ngữ dựa trên XML đã góp phần vào việc áp dụng rộng rãi nó.
- Vai trò và ứng dụng thực tế của XML trong các hệ thống công nghệ.
   + `Khả năng đọc của con người và máy móc`: Dữ liệu XML được lưu trữ ở định dạng văn bản thuần túy, tạo điều kiện thuận lợi cho cả con người và máy móc.
   + `Tự mô tả`: Tài liệu XML là tự mô tả. Các thẻ trong XML cung cấp một ý tưởng chung về loại dữ liệu mà chúng chứa.
   + `Có thể mở rộng`: XML, đúng như tên gọi của nó, cho phép người dùng xác định các thẻ của riêng họ, do đó mang lại sự linh hoạt để đại diện cho một loạt các cấu trúc dữ liệu.
   + `Nền tảng và ngôn ngữ độc lập`: Dữ liệu XML có thể được xử lý bởi bất kỳ hệ thống nào có thể xử lý XML, làm cho nó trở thành một lựa chọn tuyệt vời để trao đổi dữ liệu.
  

[2. **Cấu trúc cơ bản**]()

2.1. **Cú pháp thẻ (tags):**
   - **Thẻ mở**: Mỗi phần tử bắt đầu bằng một thẻ mở, ví dụ: `<name>`.
   - **Thẻ đóng**: Kết thúc phần tử bằng thẻ đóng, ví dụ: `</name>`.
   - **Thẻ tự đóng**: Nếu phần tử không có nội dung, bạn có thể sử dụng thẻ tự đóng, ví dụ: `<image />`.

   **Ví dụ:**
   ```xml
   <greeting>Hello, world!</greeting>
   ```

2.2. **Phần tử (elements):**
   - Một phần tử bao gồm thẻ mở, nội dung, và thẻ đóng.
   - Phần tử có thể chứa văn bản, thuộc tính, hoặc các phần tử con.
   - Các phần tử có thể lồng nhau để tạo cấu trúc dữ liệu phức tạp.

   **Ví dụ:**
   ```xml
   <person>
       <name>Quang</name>
       <age>25</age>
   </person>
   ```

2.3. **Thuộc tính (attributes):**
   - Thuộc tính cung cấp thông tin bổ sung cho một phần tử.
   - Thuộc tính được đặt bên trong thẻ mở dưới dạng cặp `tên="giá trị"`.

   **Ví dụ:**
   ```xml
   <person gender="male">
       <name>Quang</name>
       <age>25</age>
   </person>
   ```

2.4. **Một tệp XML cơ bản:**
   Một tệp XML luôn bắt đầu bằng dòng khai báo phiên bản và mã hóa:
   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <data>
       <item>
           <name>Book</name>
           <price>15.00</price>
       </item>
   </data>
   ```

[3. **Các khái niệm nâng cao**]()
   - *Namespaces*: Sử dụng không gian tên để tránh xung đột.
   - *DTD* (Document Type Definition) và *Schema*: Xác định cấu trúc và quy tắc cho tài liệu XML.
   - *XPath* và *XQuery*: Cách truy vấn dữ liệu trong XML.

[4. **Công cụ và môi trường làm việc**]()
   - Các phần mềm hỗ trợ XML (như Notepad++, VS Code).
   - Các thư viện XML trong các ngôn ngữ lập trình (Java, Python, C#,...).
     
[5. **ứng dụng**]()

| ứng dụng thực tế                     | Mô tả nhanh  | 
|--------------------------------------|--------------|
| Web Service(SOAP)                    | Giao tiếp giữa server-client qua mạng. | 
| Cấu hình phần mềm                    | File config cho app, server, framework (như web.xml, pom.xml, AndroidManifest.xml). |
| Thiết kế giao diện (UI)              | 	Mô tả bố cục giao diện trong Android, WPF (.NET).  | 
| Truyền tải dữ liệu                   | Giữa các ứng dụng khác nền tảng (Java ↔ C# ↔ Python).  | 
| Báo cáo dữ liệu                      | Các file báo cáo chuẩn như XBRL (kế toán), FpML (tài chính).  | 
| RSS Feeds                            | Để web/blog cập nhật nội dung tự động cho người đọc.  | 
| Lưu trữ tài liệu                     | Lưu dữ liệu có cấu trúc trong file dạng text.  | 
| Định dạng đồ họa (SVG)               | 	Các ảnh vector dạng XML để hiển thị trên web.  | 
| Chữ ký số và mã hóa (XMLSig, XMLEnc) | Bảo mật tài liệu XML trong giao dịch điện tử.  | 

[6. **Thực hành**]()
   - Viết tệp XML từ đơn giản đến phức tạp.
   - Tích hợp XML với các công nghệ như API, Web Services.
   - Các bài tập thực hành truy vấn và chỉnh sửa XML.
     
[7. **Tài liệu tham khảo**]()
   - Tìm đọc tài liệu chính thức và các nguồn uy tín trên internet như [W3Schools](https://www.w3schools.com/xml/).
