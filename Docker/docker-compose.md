# Docker-Compose
## I. Tổng Quan.
### 1. khái niệm.
- Docker-compose là 1 công cụ giúp định nghĩa và chạy các ứng dụng đã container 1 cách dễ dạng. Được sử udnjg file y aml để cấu hình các dịch vụ ựng dụng của nó.
![image](https://github.com/user-attachments/assets/cf48b689-83e0-4fc5-b2ee-0f42e3bd05f7)
![image](https://github.com/user-attachments/assets/dccaf9fb-a5af-463d-a447-1b1496782c4b)
![image](https://github.com/user-attachments/assets/adc16467-8c14-4b57-bf86-90ae4f89ebaa)

## II. Câu Lệnh.
### 1. Thành phần chính.

1.1. version.
- phiên bản docker compose đang sử dụng.
  ví dụ : version: "3.9"
1.2. services.
- danh sách các dịch vụ cần được triển khai.
ví dụ :
`services:
  web:
    build: .
    ports:
      - "3000:3000"
    depends_on:
      - db

`
1.3. build.
- đường dẫn dockerfile đê xây dựng image.
  ví dụ : build: ./app/
1.4. image.
- tên của image từ docker hub hoặc registry khác.
  ví dụ :   image: mysql:latest
1.5. ports.
- map cổng host với container.
  ví dụ :
  `ports:
      - "3000:3000"`
1.6. depends_on.
- định nghĩa sự phụ thược giữa các dịch vụ.
ví dụ :
` depends_on:
      - db`
1.7. environment.
- Đặt các biến môi trường cho container.
ví dụ :
`    environment:
      MYSQL_ROOT_PASSWORD: password`
### 1. câu lênh liên quan đến command.
- docker-compose up: Khởi động các container
- docker-compose down: Dừng và xóa các container
- docker-compose ps: Hiển thị trạng thái của các container
- docker-compose build: Tạo image từ Dockerfile trong mỗi dịch vụ
- docker-compose restart: Khởi động lại các container
- docker-compose stop: Dừng các container
- docker-compose rm: Xóa các container không sử dụng
- docker-compose logs: Hiển thị các logs của các container
- docker-compose config: Hiển thị các cấu hình của Docker Compose
- docker-compose exec: Thực thi một lệnh trên một container
- docker-compose port: Hiển thị các port của các container
- docker-compose top: Hiển thị các process đang chạy trong các container
