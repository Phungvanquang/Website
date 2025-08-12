# 4. Storage


##### 1. Volumes — Docker-managed persistent storage.
- Volume là vùng lưu trữ dũ liệu do docker quản lý và tách biệt khỏi vòng đời container.Khác với dữ liệu ghi trực tiếp trong container layer, dữ liệu trong volume sẽ persist ngay cả khi container bị xoá hoặc rebuild.
path :
```
/var/lib/docker/volumes/<volume_name>/_data
```

###### 1.1. Ưu Điểm : 
- portable : không phụ thuộc path trên host.
- an toàn : docker quản lí truy cập, tránh ghi đè ngoài ý muốn.
- dễ backup/restore : qua docker volume hoặc volume driver.
- có thể lưu trữ tư xa (NFS, S3, GlusterFS,v.v.).Qua volume driver.
###### 1.2. câu lệnh.
- tạo `docker volume` : 
```
docker volume create mydata
``` 
- liệt kê volume :
```
docker volume ls
```
- xem chi tiết volume :
```
docker volume inspect  shellshock1_CVE_2014_6271
```

output : 

```
[
    {
        "CreatedAt": "2025-07-22T02:08:04Z",
        "Driver": "local",
        "Labels": null,
        "Mountpoint": "/var/lib/docker/volumes/shellshock1_CVE_2014_6271/_data",
        "Name": "shellshock1_CVE_2014_6271",
        "Options": null,
        "Scope": "local"
    }
]

```

- xoá volume :
```
docker volume rm mydata
```
- xoá tất cả volume không dùng :
```
docker volume prune
```

###### 1.3. gắn volume vào container.
- cách 1 : dùng `-v`.
```
docker run -d -v mydata:/var/lib/mysql mysql:8.0
```
- cách 2 : dùng `--mount`.
```
docker run -d --mount source=mydata,target=/var/lib/mysql mysql:8.0
```
###### 1.4. volume driver.

###### 1.5. backup & Restore Volume.
- backup :
```
docker run --rm -v mydata:/data -v $(pwd):/backup busybox tar czf /backup/mydata.tar.gz /data
```
- Restore :
```
docker run --rm -v mydata:/data -v $(pwd):/backup busybox tar xzf /backup/mydata.tar.gz -C /data
```
###### 1.6. Best Practices.
- dùng volume thay vì bind mount cho dữ liệu production.
- đặt tên volume rõ ràng (app_db_data, app_logs) dễ quản lí.
- không hardcode path /var/lib/docker/... --hãy quản lí qua docker CLI.
- regular prune : xoá volume không dùng để tiết kiệm disk.
  
##### 2. Bind Mounts — Direct host path mapping.
- Bind mount là cách gắn 1 đường dẫn cụ thể trên host vào container.

###### 2.1. Ưu Điểm. 
- tức thời : sửa file trên host --> container phản ánh ngay lập tức.
- không cần copy dữ liệu khi container tạo ra.
- dễ dàng truy cập file host mà không phải export/import.
###### 2.2. Nhược Điểm.
- Portability : đường dẫn hardcode trên host,mmays khác không chắc có.
- bảo mật kém : container có thể ghi/xoá file quan trong trên host nếu mount sai.
- phụ thuộc OS host : path, permission, symlink...có thể gây lỗi khi deploy trên OS khác.
- khó backup/restore : qua Docker CLI.
##### 2.3. Cách sử dụng.
cách 1: 
```
docker run -d -v /home/user/app:/app python:3.11=slim
```
- /home/user/app --thư mục trên host.
- /app --> thư mục trong container.
- nếu path host không tồn tại --> docker sẽ tạo thư mục trống (LINUX) hoặc báo lỗi (Windows).

cách 2:
```
docker run -d --mount type=bind, source=/home/user/app,target=/app python:3.11-slim
```
###### 2.4. Quyền truy cập (read-only vs read-write).
- mặc định của bind mount là read-write.
```
docker run -d --mount type=bind,source=/home/user/app,target=/app,readonly python:3.11-slim
```

###### `thực tế  :`

Dev python app với hot reload : 
```
docker run -it --rm -v $(pwd):/app -w /app python:3.11 bash
```
- $(pwd) : thư mục code hiện tại.
- mọi thay đổi code trên host sẽ được reflect ngay trong container.

Mount Config file : 
```
docker run -d -v /etc/nginx/nginx.conf:/etc/nginx/nginx.conf:ro nginx:latest
```
- Mount file cấu hình từ host vào container,chế độ read-only.  
###### 2.5. Lỗi thường gặp. 
- path không tồn tại trên host : container sẽ tạo thư mục rỗng (LINUX) và gây lỗi nếu app mong đợi nội dung có sẵn.
- Permission mismatch : user trong container không có quyền ghi file trên host.
- SElinux/appArmor : có thẻ block biond mount trừ khi bật quyền thích hợp (:z hoặc :Z trên SELinux).
###### 2.6. Best Practices.
- dùng bind mount chủ yếu trong môi trường dev để đồng bộ code nhanh.
- trong production, hạn chế mount trừ khi thật sự cần, nó phá tính isolation.
- luôn dùng path tuyệt đối trên host.
- read-only nếu cần đọc dữ liệu để giảm rủi ro ghi nhầm.
- kiểm tra permission để tránh lỗi quyền truy cập.

###### `so sánh bind mount với volumes:`
| Tiêu chí       | Bind Mounts                     | Volumes                    |
| -------------- | ------------------------------- | -------------------------- |
| Quản lý        | Host quản lý                    | Docker quản lý             |
| Portability    | Thấp                            | Cao                        |
| Bảo mật        | Kém hơn                         | Tốt hơn                    |
| Use case chính | Dev code, config                | Persistent data production |
| Backup qua CLI | Khó                             | Dễ                         |
| Hiệu năng      | Có thể kém hơn nếu FS host chậm | Ổn định hơn                |

 
##### 3. tmpfs Mounts — In-memory ephemeral storage.
##### 4. containerd Image Store — Docker’s internal image & layer storage.
