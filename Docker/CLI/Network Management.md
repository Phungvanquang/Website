# Network Management. 

`Docker Network` là cơ chế giúp container giao tiếp với Internet.Khi container chạy , docker tạo ra Network namespace riêng và kết nối nó vào 1 network driver.

#### Các Driver chính:

| Driver      | Mô tả                                                       | Use case                                                |
| ----------- | ----------------------------------------------------------- | ------------------------------------------------------- |
| **bridge**  | Mặc định, chỉ cho container trên cùng host kết nối với nhau | Môi trường dev hoặc single-node                         |
| **host**    | Container dùng network stack của host (chia sẻ IP host)     | Khi cần hiệu năng cao, giảm latency                     |
| **none**    | Container không có network                                  | Bảo mật hoặc xử lý dữ liệu offline                      |
| **overlay** | Mạng ảo nhiều host qua Swarm/K8s                            | Cluster multi-host                                      |
| **macvlan** | Container có IP trực tiếp từ LAN                            | Khi cần container xuất hiện như máy thật trong mạng LAN |


#### 1. Docker network create – tạo network
- Tạo 1 mạng tuỳ chỉnh thay vì dùng bridge mặc định :
  - cách ly container
  -  giao tiếp bằng tên container (docker DNS nội bộ)
  -  tuỳ chỉnh IP/subnet.
ví dụ 1:
```
docker network create mynet
```
ví dụ  2:
```
docker network create \
  --driver bridge \
  --subnet 172.28.0.0.16/16 \
  --gateway 172.28.0.1 \
  --ip-range 172.28.5.0/24 \
mycustomet
```

- `--subnet ` : định nghĩa dải mạng.
- `--gateway` : gateway cho network.
- `--ip-range` : range IP container có thể nhận.
- mỗi 1 app toạ network riêng để tránh xung đột.
- subnet phải tránh trùng mạng LAN của máy thật.

#### 2. Docker network ls – liệt kê network
- hiển thị tất cả network hiện có
ví dụ :
```
docker netowork ls
docker network ls --filter driver=bridge
```
kết quả : 
```
NETWORK ID     NAME      DRIVER    SCOPE
e9b8c3c64d32   bridge    bridge    local
74b857c6df8c   host      host      local
c8b927c9f041   none      null      local
7c9cddf5f3d4   mynet     bridge    local
```
- DRIVER : loại network. 
- SCOPE : Local (chỉ trên host này) hoặc swarm.

#### 3. Docker network inspect – xem thông tin network
- Hiển thị JSON chi tiết : subnet, Driver, Container đang kết nối.
```
docker network inspect mynet
```
trích JSON : 
```
"Containers": {
    "3b1e8": {
        "Name": "web",
        "IPv4Address": "172.28.0.2/16",
        "MacAddress": "02:42:ac:1c:00:02"
    }
}
```
- khi debug network, đây là lệnh bắt buộc phải dùng.
- có thể format output để đọc:
```
docker network inspect -f '{{range .Containers}}{{.Name}} - {{.IPv4Address}}{{end}}' mynet
```
#### 4. Docker network connect / disconnect – gắn/bỏ container khỏi network
- 1 container gắn thêm hoặc tháo bơt network khi đang chạy.
ví dụ :
```
docker network connect mynet web
docker network disconnect mynet web
```
khi muốn container vừa có Internet (bridge) vừa truy cập mạng nội bộ khác (mynet).
#### 5. Docker network rm – xóa network
- chỉ xoá được nếu không có container nào kết nối.
```
docker network rm mynet
```

#### 6. Docker network prune – dọn network không dùng
- xoá tất cả các network không dùng.
```
docker network prune
```
