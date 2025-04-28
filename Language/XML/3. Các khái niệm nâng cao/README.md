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
- khai báo DTD : ```<!ELEMENT element-name content-type>```
  + Phần tử rỗng: Không chứa nội dung
  + Phần tử con: Gồm các phần tử lồng nhau.
  + Dữ liệu văn bản: #PCDATA (Parsed Character Data).
-  khai báo ENTITY : ```<!ENTITY entity-name "replacement-text">```
-  khai báo ATTLIST :  ```<!ATTLIST element-name attribute-name attribute-type default-value>```
  + Kiểu dữ liệu của thuộc tính (CDATA, ID, IDREF,...).
  + Giá trị mặc định cho thuộc tính.
### 3. ***Xpath và xQuery***

3.1. Xpath.
- Xpath là ngôn ngữ truy vấn được sử dụng để chọn các nút trong tài liệu XML.Xpath được sử dụng chỉ yếu để truy vấn dữ liệu trong XML và thường sử dụng kết hợp XSLT.
- hoạt động : XPath sử dụng một hệ thống "đường dẫn" để chỉ định vị trí của các phần tử hoặc thuộc tính trong tài liệu XML. Các đường dẫn XPath được viết tương tự như đường dẫn trong hệ thống tệp của máy tính.

Ví dụ : 

XML
```<bookstore>
  <book>
    <title lang="en">Programming XML</title>
    <author>John Doe</author>
    <price>29.95</price>
  </book>
  <book>
    <title lang="fr">Programmation XML</title>
    <author>Jean Dupont</author>
    <price>25.50</price>
  </book>
</bookstore>
````
Xpath : 
- `/bookstore/book/title`
- `/bookstore/book/title[@lang='en']`
- `/bookstore/book/title[@lang='en']`
- `/bookstore/book/author`

3.2. XQuery.
- XQuery là một ngôn ngữ truy vấn XML phức tạp hơn XPath và cho phép bạn truy vấn dữ liệu XML, kết hợp và thao tác với dữ liệu trong XML một cách linh hoạt hơn. XQuery có thể được coi là phiên bản mạnh mẽ hơn của XPath, hỗ trợ khả năng lọc, kết hợp và thao tác với dữ liệu XML.
- Cách hoạt động của XQuery:
  + XQuery không chỉ thực hiện các truy vấn mà còn cho phép bạn xử lý và thao tác với các kết quả (ví dụ: tạo các phần tử XML mới).
  + XQuery có thể sử dụng các biểu thức XPath trong cú pháp của nó, nhưng thêm vào đó, nó còn hỗ trợ các thao tác điều kiện, vòng lặp, và xử lý dữ liệu như trong các ngôn ngữ lập trình truyền thống.
 
ví dụ: 
````
let $books := doc("bookstore.xml")/bookstore/book
for $book in $books
return <book>
         <title>{ $book/title }</title>
         <author>{ $book/author }</author>
         <price>{ $book/price }</price>
       </book>
````

giải thích :
- let $books := doc("bookstore.xml")/bookstore/book: Lấy tất cả các sách từ tài liệu XML.
- for $book in $books: Lặp qua từng phần tử <book>.
- return <book>...</book>: Trả về kết quả với cấu trúc XML mới, chỉ bao gồm các thẻ <title>, <author>, và <price>.

