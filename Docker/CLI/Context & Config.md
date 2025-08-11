# Context & Config.

Quản lý kết nối và cấu hình môi trường : 
- nhiều máy chủ docker (remote host).
- nhiều môi trường (dev, taging, production).
- docker swarm mode ( quản lí config phân tán).

#### 1. Docker context create / ls / use / rm – quản lý context
- Docker Context : tập hợp thông tin kết nối đến 1 Docker endpoint, bao gồm :
  - Docker daemon endpoint(DOCKER_HOST).
  - API version.
  - TLS certificates.
  - Namespace, Swarm settings, kubernetes config (nếu có).
##### Câu Lệnh : 
`docker context create` - tạo context :
dùng khi muốn kết nối tới 1 remote docker host : 
```
docker context create my-remote \
  --docker "host=tcp://192.168.1.50:2376,ca=/path/ca.pem,cert=/path/cert.pem,key=/path/key.pem"
```
- `-docker host= ` : url kết nối (tcp, ssh..).
- `ca, cert, key` : file TLS để bảo mật kết nối.
  
`docker context ls ` - liệt kê context :
```
docker context ls 
```
output : 
```
NAME              DESCRIPTION                               DOCKER ENDPOINT                             ERROR
default           Current DOCKER_HOST based configuration   npipe:////./pipe/docker_engine
desktop-linux *   Docker Desktop                            npipe:////./pipe/dockerDesktopLinuxEngine

```

`docker context use ` - chuyển context :
```
docker context use my-remote

```

`docker context rm ` - xoá context : 
```
docker context rm my-remote
```

#### 2. Docker config (Swarm) – quản lý config trong swarm mode
- docker config : dữ liệu dạng file/text lưu trữ trong swarm manager và phân phối đến container khi chạy.
- khác với secret (bảo mật), config dùng cho filce cấu hình chúng : .conf,.json,.env,.v..v.
- swarm sẽ mount config vào container như file read-only.
##### Câu lệnh : 

`docker config create` -tạo config.
```
docker config create nginx-conf ./nginx.conf
```

`docker config ls ` -liệt kê config.
```
docker config ls 
```
output : 
```
ID                          NAME          CREATED          UPDATED
abcd1234efgh                nginx-conf    2 minutes ago    2 minutes ago
```

`docker config inspect ` - xem chi tiết config.
```
docker config inspect nginx-conf
```

`docker config rm ` -xoá config.
```
docker config rm nginx-conf
```

Gắn config vào service : 

```
docker service create \
  --name my-nginx \
  --config src=nginx-conf,target=/etc/nginx/nginx.conf \
  nginx:latest
```
