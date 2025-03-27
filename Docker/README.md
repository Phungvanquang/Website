# Docker 

## I.Tổng Quan.
### 1. khái niệm.
- Docker là một nền tảng giúp đóng gói ứng dụng cùng với môi trường của nó vào một "container" để có thể chạy đồng nhất trên nhiều hệ thống.
### 2. một số câu lênh docker

#### 2.1. xem image.
  docker image ls
#### 2.2. xem container.
  docker ps -a
#### 2.3. chạy từ docker file lên thành image.
  docker build -t <name> .
#### 2.4. chạy từ image lên container.
  docker run -p <port>:<port> -d <image>
#### 2.5. xoá container.
  docker rm <container_id>

#### Tài Liệu Tham Khảo.
- https://www.youtube.com/playlist?list=PLwJr0JSP7i8At14UIC-JR4r73G4hQ1CXO
- https://roadmap.sh/docker
