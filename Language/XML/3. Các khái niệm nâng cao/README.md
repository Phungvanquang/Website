## Các khái niệm nâng cao

### 1. ***Namespaces***
- Namespaces trong XML được sử dụng để phân biệt các phần tử và thuộc tính có cùng tên nhưng đến từ các nguồn khác nhau từ đó giúp tránh xung đột.
- Namespaces là 1 URI (Uniform Resource Identifier) , thường là URL được gán vào phần tử hoặc thuộc tính trong xml.
- nó có vai trò định danh duy nhất.
- khai báo namespace :```<bookstore xmlns="http://example.com/bookstore">```   ==>namspace:`http://example.com/bookstore`
- khai báo namespace với tiền tố :
  
````<library xmlns:novel="http://example.com/novels" xmlns:textbook="http://example.com/textbooks">````
- mục đích :
  +  tránh xung đột : làm veiejc với nhiều dữ liệu từ XML hoặc tích hợp các hệ thống , namespace giúp đảm bảo rằng không có sự nhầm lẫn giữa các phần tử cùng tên.
  +  tích hợp dễ dàng : namespace cho phép bạn kết hợp dữ liệu từ các nguồn khác nhàu mà không lo lắng.
### 2. ***DTD (Document Type Definition)***
- DTD (Document Type Definition) trong XML là một phương pháp xác định cấu trúc và các quy tắc của một tài liệu XML. Nó giúp đảm bảo rằng tài liệu XML "tuân thủ đúng định dạng" (valid XML), tức là các phần tử, thuộc tính và dữ liệu bên trong tài liệu phải tuân theo các quy định mà DTD đặt ra.

### 3. ***Xpath v af xQuery***




