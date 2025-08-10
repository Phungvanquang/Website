# Kiến thức nền tảng

#### 1. Container và Virtual Machine.
| Đặc điểm           | Container                                    | Virtual Machine (VM)                           |
| ------------------ | -------------------------------------------- | ---------------------------------------------- |
| **Cách hoạt động** | Chia sẻ chung kernel của hệ điều hành host   | Mỗi VM chạy hệ điều hành riêng trên hypervisor |
| **Tài nguyên**     | Nhẹ hơn, khởi động nhanh, chiếm ít RAM/CPU   | Nặng hơn, cần nhiều tài nguyên                 |
| **Cách đóng gói**  | Đóng gói ứng dụng + dependencies trong image | Đóng gói cả hệ điều hành + ứng dụng            |
| **Khởi động**      | Vài giây                                     | Vài phút                                       |
| **Tính di động**   | Dễ di chuyển giữa môi trường khác nhau       | Khó di chuyển hơn                              |
| **Ví dụ**          | Docker, Podman, LXC                          | VMware, VirtualBox, Hyper-V                    |

--> Container = môi trường + App , VM là máy tính giả lập hoàn toàn giống máy thật.
#### 2. Docker Architecture.
- Docker Engine --> Bộ máy chính của Docker :
  - Docker Deamon : chạy ngầm , quản lí containers , images, networks,volumes.
  - Docker CLI : dòng lệnh để giao tiếp với Deamon  ( ví dụ : `Docker run`).
  - Docker API : giao diện lập trình cho deamon
- Docker Registry : nơi lưu trữ images (docker hub là registry mặc định).

#### 3. Image và Container.
- Image : File đóng gói ứng dụng và môi trường chạy.
  - cấu tạo bởi nhiều layers.
  - dùng union file system (UnionFS) để gộp các layers.
- Container : Instance đang chạy của 1 image.
  - có thể tạo nhiều container từ cùng 1 image.
  - khi xoá container, images vẫn còn.
--> image như 1 bản thiết kế được đóng gói còn Container là sản phầm sau khi triển khai bản thiết kế.

#### 4. Docker Workflow.

Quy trình làm việ phổ biến
1. Pull image từ registry.
```
docker pull nginx
```
2. Run Container từ image.
```
docker run -d -p 8080:80  nginx
```
3. Stop Container khi không cần
```
docker stop <container_ID>
```
4. Build image mới từ Dockerfile.
```
docker build -t myapp:1.0
```
5. Push image lên registry.
```
docker push myapp:1.0
```
