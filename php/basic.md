# PHP Basic
![image](https://github.com/user-attachments/assets/8febb743-0681-4a5c-a8b5-8631634bc56c)
## 1. comments.
 //
## 2. variables
cú pháp : 
```$x```
phạm ví : 
- local
![image](https://github.com/user-attachments/assets/8a55e15c-10a5-42d2-a447-3a1ede06ca9e)
- global
![image](https://github.com/user-attachments/assets/bbb28f82-d53a-47c9-8079-d685af52ef51)
- static
## 3.  ehco/print.
- hiển thị lên gui.  
``` print "Hello";```
```print("Hello");```
## 4. data types.
- kiểu dữ liệu phổ biến.
+ String
```$x = "Hello world!";```
+ Integer : number between -2,147,483,648 and 2,147,483,647.
```$x = 5985;```
+ Float (floating point numbers - also called double) :
```$x = 10.365;```
+ Boolean : true or false.
```$x = true; ```
+ Array : Mảng lưu trữ nhiều giá trị trong một biến duy nhất.
 ```$cars = array("Volvo","BMW","Toyota");```
+ Object : Object là khuôn mẫu cho các đối tượng và mỗi đối tượng là một thể hiện của lớp.
Nếu bạn tạo một __construct()hàm, PHP sẽ tự động gọi hàm này khi bạn tạo một đối tượng từ một lớp.
![image](https://github.com/user-attachments/assets/25a45fa8-e525-468e-b7b2-ebe60553f84f)
+ NULL : Null là một kiểu dữ liệu đặc biệt chỉ có thể có một giá trị: NULL.
```$x = NULL; ```
+ Resource : Kiểu tài nguyên đặc biệt không phải là kiểu dữ liệu thực tế. Nó là việc lưu trữ tham chiếu đến các hàm và tài nguyên bên ngoài PHP.
Một ví dụ phổ biến về việc sử dụng kiểu dữ liệu tài nguyên là lệnh gọi cơ sở dữ liệu.
## 5. PHP Strings.
- ghép chuỗi :
![image](https://github.com/user-attachments/assets/1c9be7c9-2283-4b73-88d3-7a1748064824)
- strlen() :  độ dài của chuỗi.
![image](https://github.com/user-attachments/assets/fe584465-de3b-495a-b480-8fee8b0e5769)
- str_word_count() : đếm số lượng từ trong chuỗi
![image](https://github.com/user-attachments/assets/357c5369-8073-4a06-9527-89a7baeccf35)
- strpos() : tìm kiếm từ trong chuỗi.
![image](https://github.com/user-attachments/assets/3de101f2-05b5-4c72-b4b5-43c37804ba94)
- strtoupper() : chuyển thành chữ in hoa.
![image](https://github.com/user-attachments/assets/a6229c24-d3b9-4ca7-8ca8-52a2d324a03e)
- strtolower() : chuyển thành chữ thường.
![image](https://github.com/user-attachments/assets/2e3b03ad-78d1-41bd-99a5-03fa0ae082ee)
-  str_replace() : thay thế chuỗi.
![image](https://github.com/user-attachments/assets/8bf516e7-2e6c-43cc-807c-99118b55eefe)
- strrev() : đảo ngược chuỗi.
![image](https://github.com/user-attachments/assets/96ae1d16-fe38-452f-a8f1-28f200d9eff8)
- trim() : xoá khoảng trắng trong chuỗi.
![image](https://github.com/user-attachments/assets/afe95f9e-ba33-40bb-9202-154cedc9ccd8)
- explode() : chuyển chuỗi  thành mảng.
![image](https://github.com/user-attachments/assets/f4e8fdb7-d844-445b-a5f6-dece57c82a8c)-
- Nối Chuỗi :
![image](https://github.com/user-attachments/assets/64ac0d10-9bb9-4c4e-8a6b-660e1a4587f9)
![image](https://github.com/user-attachments/assets/a0d1144a-6924-45aa-83bd-54ffc7774b22)
- substr() : cắt chuỗi.
bắt đầu cắt cở vị trí số 6 và cắt 5 vị trí tiếp theo.
![image](https://github.com/user-attachments/assets/c078d917-ad63-4b6b-9f53-22216e2e0975)
hoặc cắt hết bắt đầu từ vị trí số 6.
![image](https://github.com/user-attachments/assets/9bb3fd19-f259-4b81-a266-ac54718ca244)
 cắt từ vị trí ngược
![image](https://github.com/user-attachments/assets/478714ed-51fb-481f-8961-43241e7bfac6)
## 6. Numbers. 
- types : interger,float,number strings,ìninty.NaN.
## 7. đổi kiểu dữ liệu.
- (string)- Chuyển đổi sang kiểu dữ liệu String
- (int)- Chuyển đổi sang kiểu dữ liệu Integer
- (float)- Chuyển đổi sang kiểu dữ liệu Float
- (bool)- Chuyển đổi sang kiểu dữ liệu Boolean
- (array)- Chuyển đổi sang kiểu dữ liệu Mảng
- (object)- Chuyển đổi sang kiểu dữ liệu Object
- (unset)- Chuyển đổi sang kiểu dữ liệu NULL
## 8. PHP Math.
- pi() : là số 3.14.
- min() và max() : tìm số lớn nhất và nhỏ nhất.
- abs() : là trị tuyệt đối.
- sqrt() : hmaf căn bậc 2.
- round() : làm tròn số sau đấy pẩy thành số nguyên.
- rand() : sinh số ngẫu nhiên trong khoảng.
## 9. PHP constants.
- define,const: gán nội dung vs biến số.
![image](https://github.com/user-attachments/assets/3ef76910-3a8e-43c7-b3ac-a732392be707)
![image](https://github.com/user-attachments/assets/7ede5cde-e4c2-46db-8e28-117cada0bad8)
## 10. PHP magic constant.
![image](https://github.com/user-attachments/assets/af956280-0477-4c09-8cf4-50dcbe831da6)
## 11. Toán Tử.
- Toán tử số học : -,+,*,/,%,**
- Toán tử gán : x = y	,x += y,x -= y,x *= y,x /= y,x %= y
- Toán tử so sánh : ==,===,!=,<>,!==,>,<,>=,<=,<=>
- Toán tử tăng/giảm : ++$x,--$x,$x++,$x--
- Toán tử logic :  and,or,xor,&&,||,!
- Toán tử chuỗi : .,.=
- Toán tử mảng : 
- Toán tử gán có điều kiện : ?:,??
## 12. câu lênh if..else
- ifcâu lệnh - thực thi một số mã nếu một điều kiện là đúng
- if...elsecâu lệnh - thực thi một số mã nếu điều kiện là đúng và một mã khác nếu điều kiện đó là sai
- if...elseif...elsecâu lệnh - thực thi các mã khác nhau cho hơn hai điều kiện
- switch câu lệnh - chọn một trong nhiều khối mã để thực thi.
![image](https://github.com/user-attachments/assets/9d24251d-5272-45d5-a70e-64bad49cefbd)

## 13. loops 
![Uploading image.png…]()

