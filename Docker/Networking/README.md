# 5. Networking

#### 1. giới thiệu.
Docker tạo container : 
- tạo 1 network namespace riêng cho container.
- tạo virtual ethernet pair (veth) : 1 đầu nằm trong container, một đầu gắn vào bridge hoặc network driver.
- gán địa chỉ IP cho interface trong container (thường thông qua docker0 hoặc network custom).
- cấu hình iptables để NAT traffic ra ngoài Internet.
- tích hợp DNS resolver để container có thể gọi tên thay vì IP (thông qua embedded DNS server của docker, mặc định IP 127.0.0.11 trong container).

#### 2. Network Drivers.
##### 2.1. Bridge (default).
- Kết nối các container trên cùng host.
- Đặc điểm :
  - NAT ra ngoài thông qua iptables MASQUERADE.
  - Hỗ trợ DNS container-name-based.
  - có thể tạo nhiều bridge custom để tách môi trường.
- Câu lệnh :
```
docker network create -d bridge my_bridge
```

##### 2.2. Host.
- Dùng network stack của host.
- Đặc điểm :
  - Không NAT : hiệu năng cao.
  - Mất isolation, container có thể port host.
- Câu lệnh :
```
docker run --network host nginx
```

##### 2.3. None.
- Hoàn toàn cô lập network.
- Dùng muốn container Offline  hoặc tự cấu hình networj stack.
  
##### 2.4. Macvlan.
- Contrainer có MAC & IP như 1 máy vật lí trên LAN.
- Dùng service cần broadcast/multicast hoặc phải được cấp IP từ DHCP.
- Host và container không giao tiếp trực tiếp (cần sub-interface).
- Câu lệnh :
```
docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 pub_net
```
##### 2.5. Ipvlan.
- Tương tự Maclan nhưng sử dụng IP-based routing thay vì MAC.
- Đặc điểm :
  - Container chia sẻ cùng MAC với host.
  - Chế độ :
    - I2 :  hoạt động giống maclan, yêu cầu parent interface.
    - I3 : định tuyến giữa container và host qua IP, không cần broadcast.
  - Dùng cho môi trường nhiều IP tĩnh hoặc cần tách định tuyến.
- Câu lệnh :
```
docker network create -d ipvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 ipvlan_net
```

##### 2.6. Overlay.
- kết nối container trên nhiều host trong cluster (Docker Swarm hoặc standalone + KV Store).
- Đặc điểm :
  - Sử dụng VXLAN tunneling để tạo mạng ảo trên nhiều node.
  - Yêu cầu Cluster Management (Swarm mode hoặc Consul/Etcd).
  - Hữu ích cho microservice phân tán nhiều server.
- Câu lệnh :
```
docker network create -d overlay my_overlay
```

#### 3. Networking Scenarios.
##### 3.1. Single-Host Container Communication (Bridge Network).
- Nhiều Container trên cùng host giao tiếp nội bộ mà không expose ra ngoài Internet.
- Driver : Bridge (custom bridge được khuyến nghị).
- Topology :
```
Container A <--> my_bridge <--> Container B
NAT --> eth0 --> Internet 
```
Ví dụ : 
```
docker network create --driver bridge my_bridge
docker run -d --network my_bridge --name web nginx
docker run -it --network my_bridge busybox ping web
```

##### 3.2. High-Performance Network (Host Network).
- Bỏ qua lớp NAT để tối ưu tốc độ, dùng network stack của host.
- Driver :host.
- Topology :
```
Container ⇄ Host eth0 ⇄ Internet
```
Ví dụ : 
```
docker run --rm --network host nginx
```

##### 3.3. Completely Isolated Container (None Network).
- chạy container hoàn toan offline hoặc custom network stack thủ công.
- Driver : none.
- Topology :
```
[Container] (lo only)
```
Ví dụ : 
```
docker run --rm --network none busybox
```
##### 3.4. Direct LAN Access For Container (Maclan).
- Container nhận IP từ cùng subnet với host, xuất hiện như 1 máy vật lí trong LAN.
- Driver : macvlan.
- Topology :
```
Container IP (192.168.1.x) ⇄ Switch ⇄ LAN
Host IP (192.168.1.y) ⇄ Switch ⇄ LAN
```
Ví dụ : 
```
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 pub_net
docker run --rm --network pub_net nginx
```

##### 3.5. IP Routing Without MAC Duplication (IPvlan).
- Giống macvlan nhưng dùng định tuyến IP, tránh vấn đề broadcast.
- Driver: ipvlan.
- Topology:
```
Container IP ⇄ Router ⇄ LAN
```
Ví dụ : 
```
docker network create -d ipvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 ipvlan_net
```
##### 3.6. Multi-Host Overlay Network (Swarm mode / VXLAN).
- Mục đích: Cho phép container trên nhiều host giao tiếp như cùng một mạng LAN ảo.
- Driver: overlay.
- Topology:
```
Node1: Container A ⇄ VXLAN ⇄ Node2: Container B
```
Ví dụ : 
```
docker swarm init
docker network create -d overlay my_overlay
docker service create --name web --network my_overlay nginx
```
#### 4. Networking Tutorials.
###### 4.1. Tạo và dùng custom bridge network.
- Tạo 1 mnajg bridge riêng để container giao tiếp nội bộ qua tên thay vì IP.
- Tránh việc container default bridge phải dùng --link (deprecated).
- Câu lệnh :
```
# 1. tạo custom bridge
docker network create --driver bridge my_bridge
# 2. chạy container tỏng mạng này
docker run -d --name web --network my_bridge nginx
docker run -it --name client --network my_bridge busybox
# 3. ping từ client sang web qua tên container
ping web
```

###### 4.2. Dùng host network cho hiệu năng cao.
- Container chia sẻ network namespace với host, giảm latency.
- câu lệnh :
```
docker run --rm --network host nginx
```

###### 4.3. Container không có network (None).
- Container hoàn toàn offline, chỉ có loopback.
- Câu lệnh :
```
docker run -it --network none busybox

```

###### 4.4. Macvlan - Container như máy thật trên LAN.
- Container nhận IP cùng subnet với host, xuất hiện như một máy độc lập.
- Câu lệnh :
```
# Tạo macvlan
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 pub_net

# Chạy container
docker run -it --network pub_net busybox
```
###### 4.5. IPvlan - Routing-based network.
- Giống macvlan nhưng dùng IP routing, giảm broadcast.
- Câu lệnh :
```
docker network create -d ipvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  -o parent=eth0 ipvlan_net

docker run -it --network ipvlan_net busybox
```

###### 4.6. Overlay network - multi-host container communication.
- Kết nối container trên nhiều host như cùng LAN ảo.
- Câu lệnh :
```
# Trên node1
docker swarm init --advertise-addr <IP_node1>
docker network create -d overlay my_overlay

# Trên node2 join swarm
docker swarm join --token <token> <IP_node1>:2377

# Deploy service
docker service create --name web --network my_overlay nginx
```

#### 5. Networking Management(CLI Commands).
###### 5.1. Liệt kê netowrk.
```
docker network ls
```
- hiển thin danh sách tất cả network hiện có (bao gồm bridge, host, none và custom netowkrs).

Ví dụ :
```
docker network ls --filter driver=bridge
```
- lọc hiển thị network dùng driver bridge.

###### 5.2. Tạo network.
```
docker network create <name>
```
- help :
  - `-d , --driver` : chon driver (bridge, macvlan,overlay,...).
  - `subnet` : CIDR cho IP range.
  - `--gateway` : gateway IP.
  - `--ip-range` : dải IP cho container.
  - `internal` : mạng nội bộ, không ra Internet.
  - `-o,--opt` : Options driver-specific.
Ví dụ :
```
docker network create -d bridge --subnet=172.28.0.0/16 --gateway=172.28.0.1 my_bridge
```
##### 5.3. Inspect network.
```
docker network inspect <name|id>
```
- trả về JSON chi tiết cấu hình network : driver,subnet, containers đang gắn, IPAM config....
Ví dụ :
```
docker network inspect my_bridge | jq '.[0].containers'
```

##### 5.4. Xoá network.
```
docker network rm <name|id>
```
- không thể xoá network nếu vẫn có container đang sử dụng.
- với nhiều network.
Ví dụ :
```
docker network rm net1 net2 net3
```

##### 5.5. Kết nối container vào network.
```
docker network connect <network> <container>
```
Ví dụ : 
```
docker network connect my_bridge my_container
```
- container sẽ có thêm 1 interface mới tương ứng network đó.
- có thể chỉ định IP tĩnh :
```
docker network connect --ip 172.28.0.50 my_bridge my_container
```

##### 5.6. Ngắt kết nối container khởi network.
```
docker network disconnect <network> <container>
```
- force : ngắt ngay cả khi container đang chạy.
##### 5.7. Dọn network không dùng.
```
docker network prune 
```
- xoá tất cả network khôgng được container nào sử dụng ( ngoạ trừ default network: bridge, host, none).
ví dụ :
```
docker network prune --filter  "until=24h"
```
- filter : giới hạn xoá điều kiện.

##### 5.8. Tạo network Macvlan.
```
docker network create -d macvlan --subnet=192.168.1.0/24 --gateway=192.168.1.1 -o parent=eth0 pub_net
```
- `-o parent` : chỉ định interface vật lí để gắn.

##### 5.9. Tạo Overlay network.
```
docker network create -d overlay my_overlay
```
- chỉ hoạt động khi bật docker Swarm.

##### 5.10. Các tuỳ chọn filter & format nâng cao.
```
docker network ls --filter driver=bridge --format "{{.Name}}:{{.Driver}}"
```
- .ID,Name, Driver, .Scope, .Labels
  
#### 6. CA Certificates.
##### 6.1. Khái niệm. 
- CA (Certificates Authority) : tổ chức phát hành chứng chỉ số (Certificates) để xác thực danh tính server/client.
- Docker, CA certificates thường dùng :
  - kết nối docker CLI ↔ Docker Daemon qua TLS an toàn.
  - kết nối đến private registry (HTTPS).
  - kết nối giữa các Node trong Docker Swarm/Overlay network (mã hoá mutual TLS).
##### 6.2. Sử dụng CA Certificates.
- Docker Daemon TLS Secured API :
  - khi Daemon lắng nghi trên TCP của Daemon và client certificate của CLI.
  - CLI kết nối được nếu CA đó.
- Private registry Https :
  - khi docker pull/ docker push image từ registry riêng.
  - nếu regsitry dùng chứng chỉ CA tự kí (Self-signed), Docker cần được cấu hình tin CA đó.
- Docker Swarm :
  - Swarm mode tự động cấp CA và rotate cert cho mode.
  - có thể thay CA mặc định bằng CA nội bộ.
  
##### 6.3. Vị trí CA Certificates.

Trên Linux (Docker Engine):
```
/etc/docker/certs.d/<registry-host>[:port]/ca.crt
```

Trên Windows : 
```
C:\ProgramData\docker\certs.d\<registry-host>\ca.crt
```

Trên MACOS :
- Import CA vào Keychain Access và tin tưởng nó.

##### 6.4. Cài đặt.

Ví dụ : 

1. Copy file CA cert vào thư mục :
```
sudo mkdir -p /etc/docker/certs.d/myregistry.local:5000
sudo cp my-ca.crt /etc/docker/certs.d/myregistry.local:5000/ca.crt
```
2. Restart Docker :
```
sudo systemctl restart docker
```
3. Test :
```
docker login myregistry.local:5000
```

##### 6.5. Kích hoạt.

Tạo CA,Server cert, client cert : 
```
# 1. Tạo private key cho CA
openssl genrsa -out ca-key.pem 4096

# 2. Tạo CA cert
openssl req -new -x509 -days 365 -key ca-key.pem -sha256 -out ca.pem

# 3. Tạo server key
openssl genrsa -out server-key.pem 4096
openssl req -subj "/CN=my-docker-host" -sha256 -new -key server-key.pem -out server.csr

# 4. Ký server cert với CA
openssl x509 -req -days 365 -sha256 -in server.csr \
  -CA ca.pem -CAkey ca-key.pem -CAcreateserial \
  -out server-cert.pem
```

Cấu hình Docker Daemon : 
```
# /etc/docker/daemon.json
{
  "hosts": ["tcp://0.0.0.0:2376", "unix:///var/run/docker.sock"],
  "tlsverify": true,
  "tlscacert": "/etc/docker/ssl/ca.pem",
  "tlscert": "/etc/docker/ssl/server-cert.pem",
  "tlskey": "/etc/docker/ssl/server-key.pem"
}
```

Khởi động lại : 
```
sudo systemctl restart docker
```

CLI kết nối : 
```
docker --tlsverify \
  --tlscacert=ca.pem \
  --tlscert=cert.pem \
  --tlskey=key.pem \
  -H=tcp://my-docker-host:2376 version
```

##### 6.6. Bảo mật.
- bảo vệ private key của CA và server.
- dùng chứng chỉ có thời gian ngắn và rotate định kỳ.
- public registry (docker hub), không cần cài CA vì đã tin tường sẵn trong OS.

#### 7. Packet Filtering & Firewalls.
##### 7.1. Khái niệm.
- Docker tự động cấu hình packet filtering thông qua iptables(Linux) hoặc tương đương trên các hệ thống khác.
- Mục tiêu :
  - Cho phép container giao tiếp với nhau và tới internet.
  - Cô lập container khỏi các mạng không mong muốn.
  - Áp dụng NAT cho network driver bridge.
- Firewall rules trong docker có thể bị xung đột với firewall hệ thống nếu không cấu hình đúng.
##### 7.2. xử lý.
- Khi cài Docker trên Linux :
  - nat : NAT outbound traffic từ container qua host.
  - filter : kiểm soát cho phép/khoá traffic vào/ra container.
  - mangle : đôi khi dùng để đánh dấu gói.
- Chain chính docker dùng :
  - DOCKER : cho pheps traffic container ↔ host.
  - DOCKER-USER (filter table): nơi người quản trị có thể đặt rule tuỳ chình, được áp dụng trước rule docker mặc định. 
  - DOCKER-ISOLATION-STAGE-1/STAGE-2 : tách biệt container trên các bridge network khác nhau.
##### 7.3. Cấu trúc.
```
iptables -t nat -L -n
# Chain DOCKER (2 references)
#   DNAT tcp -- anywhere anywhere tcp dpt:80 to:172.17.0.2:80
```

```
iptables -t filter -L -n
# Chain DOCKER-USER (policy ACCEPT)
#   - các rule tùy chỉnh do admin đặt
```
##### 7.4. Thực nghiệm.

1. chặn toàn bộ traffic ra Internet từ 1 container :
```
# Lấy IP container
CONTAINER_IP=$(docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mycontainer)

# Chặn output từ IP container
sudo iptables -I DOCKER-USER -s $CONTAINER_IP -j DROP
```

2. cho phép container truy cập 1 domain/IP :
```
sudo iptables -I DOCKER-USER -s $CONTAINER_IP -d 8.8.8.8 -j ACCEPT
sudo iptables -A DOCKER-USER -s $CONTAINER_IP -j DROP
```

3. Chặn Container truy cập Container khác trên cùng host :
```
sudo iptables -I DOCKER-USER -s 172.18.0.0/16 -d 172.18.0.0/16 -j DROP
```
##### 7.5. Tích hợp FirewallD/UFW vơi Docker.
- Docker có thể bỏ qua các rule của UFW/FirewallD vì nó chèn iptables trực tiếp.
- kíc hoạt iptables forwarding trong UFW :
```
sudo nano /etc/default/ufw
DEFAULT_FORWARD_POLICY="ACCEPT"
```
- chèn rule vào chain DOCKER-USER thay vì chain của UFW.
##### 7.6. bảo mật.
- Không chỉnh trực tiếp chain `DOCKER` :
- Chỉ đặt rule tuỳ chỉnh trong `DOCKER-USER` :
- Cẩn thận khi xoá iptables rule vì có thể phá vỡ container networking.
- Kiểm tra rule:
```
sudo iptables -L -n -v
sudo iptables -t nat -L -n -v 
```

#### 8. Debug & Trouleshooting.
##### 8.1. Mục Tiêu.
- Xác định nguyên nhân sự cố network liên quan tới container.
- Kiểm tra cấu hình Network driver, iptables, DNS.
- Phân biệt lỗi đến từ Docker hay hạ tầng hệ điều hành.
##### 8.2. Các bước Debug.

1. Kiểm tra danh sách Network.
2. Inspect Network.
3. Kiểm tra IP Container.
4. Kiểm tra DNS trong Container.
5. Test Connectivity từ Container.
6. kiểm tra Firewall & iptables.
7. Kiểm tra xung đột cổng (port).
8. debug Overlay Network / multi-host.
##### 8.3. Công Cụ.
- tcpdump : capture traffic giữa container host :
```
```
- traceroute : xác định đường đi packet.
- ethtool : Kiểm tra trạng thái interface.

##### 8.4. Checklist xử lí sự cố.
- Container có attach đúng chưa ?
- Container có attach đúng network chưa?
- IP container có hợp lệ với subnet network không?
- DNS trong container có đúng không?
- Firewall host có chặn traffic container không?
- Có xung đột cổng với service khác không?
- Nếu overlay → swarm cluster có ổn định không? 
#### 9. Performance & Tuning.
##### 9.1. Mục tiêu.
- giảm latency & tăng throughput network cho container.
- Giảm overhead từ NAT, iptables.
- Tối ưu cho workload real-time, HPC, hoặc high-throughput.
##### 9.2. Yếu tố ảnh hưởng đến hiệu năng.

1. Loại network driver
- Host → nhanh nhất, không NAT.
- Bridge → có NAT, iptables → overhead.
- Macvlan / ipvlan → bypass docker0 bridge, giảm latency.
- Overlay → chậm nhất (VXLAN encapsulation, cross-node).

2. NAT & iptables
- NAT table lớn làm giảm hiệu năng.
- Quá nhiều rules gây CPU overhead khi lookup.

3. MTU (Maximum Transmission Unit)
- Overlay network thường cần giảm MTU (ví dụ: 1450) để tránh fragmentation.
- MTU không đồng bộ → packet loss.

4. CPU pinning & IRQ affinity
- NIC IRQ nên phân phối hợp lý giữa các CPU core.
- Container network processing chịu ảnh hưởng từ CPU scheduling.
##### 9.3. Kỹ thuật tuning.
1. Chọn driver phù hợp
- Intra-host high performance → host hoặc macvlan.
- Multi-host, nhưng yêu cầu tốc độ → cân nhắc ipvlan thay overlay.

2. Giảm NAT overhead
- Sử dụng --network host cho container cần tốc độ.
- Hoặc dùng macvlan/ipvlan để container có IP riêng → không cần NAT.
- Nếu bắt buộc dùng bridge:
```
sudo iptables -t nat -L -n -v
# Dọn rules cũ không dùng
```

3. Điều chỉnh MTU
Xác định MTU của NIC:
```
ip link show eth0
```
- Khi tạo network:
```
docker network create -d overlay --opt com.docker.network.driver.mtu=1450 mynet
```

4. Tối ưu sysctl
Một số kernel parameters có thể cải thiện throughput:
```
# Tăng buffer TCP
sysctl -w net.core.rmem_max=268435456
sysctl -w net.core.wmem_max=268435456

# Tăng backlog
sysctl -w net.core.netdev_max_backlog=5000

# Cho phép reuse TIME_WAIT socket
sysctl -w net.ipv4.tcp_tw_reuse=1
```
(Áp dụng cẩn thận, test trước khi dùng production)

5. CPU pinning cho container network stack
```
docker run --cpuset-cpus="2,3" --network host myapp
```
- Giúp giảm context switching.

6. Giảm latency overlay network.
- Dùng encrypted overlay (--opt encrypted) khi cần bảo mật, nhưng nếu không bắt buộc → tắt encryption để giảm overhead.
- Giảm hop giữa nodes bằng cách bố trí container gần nhau (same rack, same switch).

##### 9.4. Giám sát hiệu năng.
- docker stats → CPU, memory.
- ifstat, vnstat → bandwidth monitoring.
- iperf3 → đo tốc độ giữa containers:
```
# Server
docker run --rm -it --network mynet networkstatic/iperf3 -s
# Client
docker run --rm -it --network mynet networkstatic/iperf3 -c server_ip
```
##### 9.5. Checklist.
- Chọn driver phù hợp → tránh overlay nếu không cần.
- Giảm NAT → dùng host, macvlan, ipvlan.
- Đồng bộ MTU giữa host và container.
- Tối ưu sysctl & NIC IRQ.
- Giám sát băng thông định kỳ, phát hiện bottleneck sớm.

#### 10. Bảo mật.
##### 10.1. Mục tiêu bảo mật
- Ngăn truy cập trái phép giữa các container hoặc từ bên ngoài.
- Giảm bề mặt tấn công mạng.
- Bảo vệ dữ liệu khi truyền tải (confidentiality & integrity).
- Đáp ứng yêu cầu compliance (PCI-DSS, HIPAA, ISO27001…)
##### 10.2. Mối đe dọa phổ biến.

1. Container-to-container snooping.
- Container đọc traffic của container khác trên cùng network.

2. ARP Spoofing / MAC Flooding.
- Thao túng bảng ARP để redirect traffic.

3. Man-in-the-Middle (MitM).
- Chặn/gửi dữ liệu giả mạo.

4. Port scanning & service enumeration.
- Container/attacker quét tìm cổng mở trong network.

5. Data leakage.
- Gửi nhầm dữ liệu ra ngoài qua network không mã hóa.
##### 10.3. Nguyên tắc bảo mật.

1. Nguyên tắc "Least Privilege".
- Chỉ cho container join network thực sự cần thiết.
- Mỗi microservice nên chạy trong network riêng biệt.

2. Network segmentation.
- Sử dụng custom bridge hoặc overlay để cô lập nhóm container.
- Không để tất cả container vào bridge mặc định.
```
docker network create -d bridge frontend_net
docker network create -d bridge backend_net
```

3. Kiểm soát traffic bằng iptables.
- Docker tự tạo rules iptables, nhưng bạn có thể tùy chỉnh chain riêng để filter.

Ví dụ: chặn container truy cập internet:
```
iptables -I FORWARD -o eth0 -s 172.18.0.0/16 -j DROP
```

4. Sử dụng --icc=false & --iptables=true.
- `--icc=false` :chặn container giao tiếp với nhau trừ khi share network.
- `--iptables=true` : đảm bảo Docker áp dụng rules tự động.
5. Encryption in transit
- Overlay network hỗ trợ mã hóa (VXLAN + IPsec):
```
docker network create -d overlay --opt encrypted secure_net
```
- Hoặc dùng mTLS giữa services.

6. Giới hạn quyền root trong container.
- Dùng user non-root:
```
USER appuser
```
- Hoặc chạy container với --cap-drop ALL và chỉ add capabilities cần thiết:
```
docker run --cap-drop ALL --cap-add NET_BIND_SERVICE myapp
```

7. Bảo vệ daemon API.
- Mặc định Docker API chỉ listen local socket.
- Nếu mở TCP (-H tcp://0.0.0.0:2376), bắt buộc dùng TLS + client cert:
```
dockerd --tlsverify --tlscacert=ca.pem --tlscert=server-cert.pem --tlskey=server-key.pem
```

8. IDS/IPS & Audit.
- Kết hợp Suricata, Snort, hoặc Falco để phát hiện bất thường.
- Audit logs network connections giữa containers.

9. Bảo mật DNS.
- Docker DNS nội bộ chỉ nên trả lời cho container trong cùng network.
- Với sensitive service → disable Docker DNS, cấu hình thủ công `/etc/hosts`.
##### 10.4. Checklist bảo mật Docker Networking.
- Sử dụng network riêng cho từng nhóm service.
- Bật encryption khi truyền qua overlay.
- Chặn traffic không cần thiết bằng iptables.
- Không chạy container với quyền root trừ khi bắt buộc.
- Bảo vệ Docker API bằng TLS & xác thực client.
- Theo dõi logs & network traffic để phát hiện bất thường.
