# 4. Storage


#### 1. Volumes — Docker-managed persistent storage.
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
  
#### 2. Bind Mounts — Direct host path mapping.
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

 
#### 3. tmpfs Mounts — In-memory ephemeral storage.
- tmpfs mount là loại mount đặc biệt mà dữ liệu được lưu trữ trực tiếp trong RAM của host , không ghi xuống disk.
    - nhanh hơn so với SSD/HDD.
    - dữ liệu biến mất khi container dừng hoặc bị restart.
    - thích hợp cho cache, dữ liệu tạm hoặc dữ liệu nhạy cảm không lưu lại.
######  `3.1. Ưu điểm`:
- hiệu năng cực cao : đọc/ghi RAM nhanh hơn nhiều lần so với disk.
- không để lại dấu vết trên disk : dữ liệu nhạy cảm (token, session key, temp files ).
- tránh ghi mòn SSD : trong các tác vụ I/O tạm thời.
###### `3.2. Nhược điểm`:
- dung lượng hạn chế : giới hạn bởi RAM của host.
- dữ liệu mấy ngay : khi container stop/restart.
- không thích hợp cho dữ liệu cần persistence.
- nếu lạm dụng : dễ gây thiếu RAM, ảnh hưởng toàn hệ thống.
###### `3.3. Câu lệnh.`

cách 1: dùng `--mount`:
```
docker run -d --mount type=tmpfs,destination=/app/cache nginx:latest
```
- `type-tmpfs` : kiểu mount RAM-based.
- destination : nơi trong container mà tmpfs sẽ mount vào.
  
cách 2 : dùng `--tmpfs`.
```
docker run -d --tmpfs /app/cache nginx:latest
```
###### `3.4. giới hạn dung lượng & options.`
mặc định tmpfs  sẽ dùng tối đa dung lượng RAM hệ thống cho mount point, nhưng ta có thể giới hạn : 

```
docker run -d --mount type=tmpfs,destination=/app/cache,tmpfs-size=64m,tmpfs-mode=1777
```
- tmpfs-size=64m :tối đa 64MB.
- tmpfs-mode=1777 : quyền truy cập.

`thưc tế : `
- cache web server :
  
```
docker run -d --mount type=tmpfs,destination=/var/cache/nginx nginx:latest
```

-> giúp nginx đọc/ghi cache cực nhanh mà không ảnh hưởng disk.
- lưu file tạm.
  
```
docker run -it --tmpfs /tmp:rw ubuntu bash
```

-> file trong /tmp sẽ biến mất sau khi container dừng.
- dữ liệu nhạy cảm.
```
docker run -it --tmpfs /secrets alpine sh
```

-> lưu file private key, session tokens.v.v.trong RAM thay vì disk.

##### `3.5. Best Practices.`
- dùng cho cache, session storage, build arifacts tạm.
- luôn giới hạn dung lượng để tránh container chiếm hết RAM host.
- không dùng cho dữ liệu cần lưu lâu dài.                                                                                     
#### 4. containerd Image Store — Docker’s internal image & layer storage.
##### 4.1. Container.
- Container là container runtime cấp thấp mà docker sử dụng để quản lí :
    - Pull/push image.
    - Quản lí snapshot(layer).
    - chạy Container.

Docker hiện đại (20.x trở lên) chạy trên kiến trúc.
```
docker CLI --> Docker Daemon --> Containerd --> Runc
```
- Containerd quản lí image store (nơi chứa image,metadata và layer trên host).

##### 4.2. vị trí lưu trữ.

###### `trên linux: `
```
/var/lib/docker/
```
- Cấu trúc thư mục quan trọng:
    - /var/lib/docker/image/overlay2/ → metadata image
    - /var/lib/docker/overlay2/ → layer filesystem (copy-on-write)
    - /var/lib/docker/volumes/ → volumes
    - /var/lib/docker/containers/ → thông tin container (log, config)
###### `nếu Docker dùng container trực tiếp : `
```
/var/lib/containerd/
```
- /var/lib/containerd/io.containerd.snapshotter.v1.overlayfs/ : layer data
- /var/lib/containerd/content/ : Blobs (tar layers)
- /var/lib/containerd/metadata.db : SQLite DB chứa metadata

##### 4.3. Cách hoạt động của image store

1.Khi pull image:
- Docker CLI gọi docker pull nginx:latest
- Docker daemon yêu cầu containerd tải image từ registry
- Layer được lưu dưới dạng blobs trong content/
- Sau đó, containerd unpack blobs thành snapshot trong overlayfs

2. Khi run container:
- containerd dùng snapshot overlay2 làm rootfs
- Copy-on-write (COW) → chỉ ghi layer mới khi có thay đổi

3. Khi xóa image/layer:
- Nếu không container nào tham chiếu → containerd giải phóng snapshot & blob
- Có thể xem bằng:
```
docker system df
docker image prune
```

