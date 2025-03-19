# Dockerfile 

## 1. khái niệm.
- dockerfile là 1 văn bản chứa các lệnh mà người dùng có thể gọi trên dòng lênh lắp ráp 1 image.sử dụng docker build người dùng có thể tạo 1 image tự động.
## 2. các câu lệnh.
### 2.1. ADD.
- sao chép các tệp hoặc folder từ máy vào image.
ví dụ : ADD app.jar /app/app.jar ( sao chép app.jar vào folder app)
### 2.2. ARG. 
- được sử dụng trong quá trình build trong docker image để xác định các phiển bản của image
cú pháp : ARG <biến>[=<giá trị mặc đinh>]
ví dụ : ARG VERSION=latest
### 2.3. CMD.
- cung cấp mặc đinh cá dịch mặc đinh khi container khởi chạy.đây là câu lceenh chạy chương trình mà không cần shell.
ví dụ : CMD['echo','hello']
### 2.4. COPY. 
- sao chép các tệp từ nguồn và thêm chúng vào hệ thống của image.nhưng tự giải nén và nén file.
ví dụ: COPY . /app
### 2.5. entrypoint.
- định cấu hình 1 container sẽ chạy như 1 file thực thi. Câu lênh này giống như câu lệnh CMD.
ví dụ : ENTRYPOINT['java','-jar']
### 2.6. ENV.
- đặc biến môi trường.
ví dụ : Ví dụ: ENV JAVA_HOME /usr/lib/jvm/java-11-openjdk-amd64 (đặt biến môi trường JAVA_HOME).
### 2.7. EXPOSE.
- thông báo rằng container bạn đang lắng nghe ở cổng nào.
ví dụ : EXPOSE 8080 (thông báo rằng container lắng nghe trên cổng 8080).
### 2.8. FROM. 
- sử dụng dịch vụ khác có image có sẵn.
ví dụ : FROM ubuntu:latest ( sử dụng image ubuntu phiên bản mới nhất làm cơ sở).
### 2.9. HEALTHCHECK.
- hướng dẫn docker kiểm tra tình trạng của container để biết rằng container đó vẫn đang hoạt động.
ví dụ : HEALTHCHECK CMD curl -f http://localhost || exit 1
### 2.10. LABEL.
- dùng để gán metadata cho image , giúp tìm kiếm quản lí container.
ví dụ : 
LABEL version="1.0"
LABEL env="production"
LABEL app="web-server"
### 2.11. ONBUILD.
- là 1 trigger(kích hoạt) trong dockerfile giúp thiết lập hành động sẽ chạy image được khi sử dụng làm base image trong 1 dockerfile khác ( thựcx hiện tính kế thừa của docker file khác).
ví du : 
Giả sử tạo một base image cho ứng dụng Node.js:
dockerfile.
####Dockerfile 1: Tạo base image
FROM node:18
WORKDIR /app
- Tự động copy package.json và chạy npm install khi có Dockerfile mới kế thừa
ONBUILD COPY package.json /app/
ONBUILD RUN npm install

--->  như này docker image khác kết thực file package.json của file này và tự động cài npm install.
### 2.12. RUN.
- thực hiện câu lênh thực thi trong docker image và chỉ chạy 1  lần và lưu lại ở layer của image.
ví dụ : 
FROM ubuntu:20.04
RUN apt-get update && apt-get install -y curl
### 2.13. SHELL.
- Cho phép dạng thay thế của Shell được sử dụng khi thực thi các lệnh RUN, CMD và ENTRYPOINT.
ví dụ : SHELL ["powershell", "-Command"].
### 2.14. STOPSIGNAL.
- Đặt tín hiệu hệ thống sẽ được sử dụng để thoát container.
ví dụ : STOPSIGNAL SIGTERM
### 2.15. USER.
- Đặt tên người dùng hoặc UID và tùy chọn nhóm người dùng hoặc GID sẽ được sử dụng khi chạy Image và cho bất kỳ lệnh RUN, CMD và ENTRYPOINT nào theo sau nó trong Dockerfile.
Ví dụ: USER nginx
### 2.16. VOLUME.
- Tạo một điểm gắn kết với tên được chỉ định và đánh dấu nó là giữ các ổ đĩa được gắn ngoài từ các container gốc.
ví dụ : VOLUME /app/data
### 2.17. WORKDIR.
- Đặt thư mục làm việc cho bất kỳ lệnh RUN, CMD, COPY, ADD nào theo sau nó trong Dockerfile.
Ví dụ: WORKDIR /app (đặt thư mục làm việc là /app).



