# Volume Management.

#### 1. docker volume create – tạo volume.
- docker volume là 1 mount point được quản lí bởi docker ,nằm noaig lifecycle của container --> dữ liệu không mất khi container bị xoá.
- mặc địn lưu tại `/var/lib/docker/volumes/<volume_name>/_data/`.
- có thể tạo volume với driver khác ngoài local (vd: NFS, cloud storage).
ví dụ :
```
# tạo volume mặc định
docker volume create mydata
# tạo volume với driver tuỳ chỉnh
docker volumce create --driver local mylocalvol
# tạo volume với label 
docker volume create --label project=webapp mywebvol
```
#### 2. docker volume ls – liệt kê volume.
- liệt kê tất cả volume trong local docker host.
- các volume 'dangling' (mồ côi) là volume không được gắn vào container nào.
ví dụ :
```
docker volume ls
docker volume ls --filter dangling=true
```
#### 3. docker volume inspect – xem chi tiết volume.
- trả về metadata JSON:driver,mountpoint , lables, trạng thái.
- hữu ích để xác định đường dẫn thực tế của dữ liệu.
ví dụ :
```
docker volume inspect mydata
docker volume inspect -f '{{.Mountpoint}}' mydata
```
#### 4. docker volume rm – xóa volume.
- xoá metadata và dữ liệu bên trong volume.
- không xoá nếu volume đang được container sử dụng.
ví dụ :
```
docker volume rm mydata
docker volume rm$(docker volume ls -q) # xoá hàng loạt.
```
#### 5. docker volume prune – dọn volume không dùng.
- xoá tất cả volume 'dangling' (khong được mount vào container nào).
- yêu cầu xác nhận trước khi xoá.
ví dụ :
```
docker volume prune
```
