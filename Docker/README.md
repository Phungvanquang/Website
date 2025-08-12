# Docker 


Docker Learning Roadmap
#### [0. Kiến thức nền tảng (mới thêm - trước khi cài đặt)](https://github.com/Phungvanquang/Website/tree/main/Docker/Ki%E1%BA%BFn%20th%E1%BB%A9c%20n%E1%BB%81n%20t%E1%BA%A3ng)
- Container vs Virtual Machine
- Docker Architecture (Engine, Daemon, CLI, Registry)
- Images & Containers (layer, UnionFS)
- Docker workflow cơ bản (Pull → Run → Stop → Build → Push)
#### [1. Cài đặt & Chuẩn bị]()
- Install (Ubuntu, Debian, RHEL, Fedora, ...)
- Post-installation steps
#### [2. CLI & Quản lý cơ bản](https://github.com/Phungvanquang/Website/tree/main/Docker/CLI)
- Lệnh docker cơ bản (run, ps, stop, rm, exec, logs, pull, build, images, rmi)
- Completion & Proxy configuration
- Filter & Format output
- Contexts
- Object labels
- Prune unused objects
#### [3. Docker Hub & Registry (mới thêm)]()
- Tìm & pull image (docker search, pull)
- Tag & push image
- Docker login
- Private registry basics
#### [4. Storage]()
- Volumes
- Bind mounts
- tmpfs mounts
- containerd image store
#### [5. Networking]()
- Network drivers (bridge, host, none, ipvlan, macvlan, overlay)
- Networking scenarios
- CA certificates
- Packet filtering & firewalls
```
├── 6. Container Lifecycle nâng cao
│   ├── Start automatically
│   ├── Multi-process containers
│   ├── Resource constraints
│   ├── Runtime metrics
│   ├── IPv6 networking
│   ├── Live restore
│   ├── Alternative runtimes
│   ├── Docker metrics (Prometheus)
│   ├── Remote daemon access
│   └── Troubleshooting daemon
│
├── 7. Logging
│   ├── Logging drivers (JSON file, local, syslog, fluentd, splunk, gcloud, ...)
│   ├── Customize output
│   └── Remote logging integration
│
├── 8. Security
│   ├── Rootless mode
│   ├── AppArmor
│   ├── User namespace
│   ├── Protect daemon socket
│   ├── Seccomp profiles
│   ├── Content trust & Notary
│   └── Antivirus & Docker
│
├── 9. Build System & BuildKit
│   ├── Dockerfile cơ bản
│   ├── Multi-stage build
│   ├── Variables & Secrets
│   ├── Multi-platform build
│   ├── Build cache (local, registry, inline, s3, azure, github)
│   ├── Exporters
│   ├── Attestations & SBOM
│   ├── BuildKit config
│   └── Best practices
│
├── 10. Docker Compose
│   ├── Plugin vs Standalone
│   ├── Quickstart
│   ├── Service definition
│   ├── Env variables
│   ├── Networking in Compose
│   ├── Secrets
│   ├── GPU support
│   ├── Production usage
│   ├── Merge/Extend/Include
│   └── OCI artifact applications
│
├── 11. Swarm Mode
│   ├── Create & manage swarm
│   ├── Add nodes
│   ├── Deploy services & stacks
│   ├── Scale & rolling updates
│   ├── Swarm security (PKI)
│   ├── Secrets & configs
│   ├── Routing mesh
│   └── Raft consensus
│
└── 12. Plugins
    ├── Access authorization
    ├── Log driver plugins
    ├── Network driver plugins
    └── Volume plugins
```
# I. Tổng quan.
## 1. khái niệm. 
- `Docker` là một nền tảng mở để phát triển, vận chuyển và chạy các ứng dụng. Docker cho phép bạn tách biệt các ứng dụng của mình khỏi cơ sở hạ tầng để bạn có thể phân phối phần mềm nhanh chóng. Với Docker, bạn có thể quản lý cơ sở hạ tầng của mình theo cùng cách bạn quản lý các ứng dụng của mình. Bằng cách tận dụng các phương pháp của Docker để vận chuyển, thử nghiệm và triển khai mã, bạn có thể giảm đáng kể độ trễ giữa việc viết mã và chạy mã trong sản xuất.
- Docker cho phép bạn tách biệt các ứng dụng của mình khỏi cơ sở hạ tầng để bạn có thể phân phối phần mềm nhanh chóng. Với Docker, bạn có thể quản lý cơ sở hạ tầng của mình theo cùng cách bạn quản lý các ứng dụng của mình.
- Bằng cách tận dụng các phương pháp của Docker để vận chuyển, thử nghiệm và triển khai mã nhanh chóng, bạn có thể giảm đáng kể độ trễ giữa việc viết mã và chạy mã trong sản xuất.
### 2. Kiến trúc Docker
- Docker sử dụng kiến ​​trúc máy khách-máy chủ. Máy khách Docker giao tiếp với daemon Docker, thực hiện nhiệm vụ nặng nề là xây dựng, chạy và phân phối các container Docker của bạn. Máy khách Docker và daemon có thể chạy trên cùng một hệ thống hoặc bạn có thể kết nối máy khách Docker với daemon Docker từ xa. Máy khách Docker và daemon giao tiếp bằng REST API, qua socket UNIX hoặc giao diện mạng. Một máy khách Docker khác là Docker Compose, cho phép bạn làm việc với các ứng dụng bao gồm một tập hợp các container.

### 3. images
- Image là một mẫu chỉ đọc có hướng dẫn để tạo một container Docker. Thường thì một image dựa trên một image khác, với một số tùy chỉnh bổ sung. Ví dụ, bạn có thể xây dựng một image dựa trên ubuntu image đó, nhưng cài đặt máy chủ web Apache và ứng dụng của bạn, cũng như các chi tiết cấu hình cần thiết để chạy ứng dụng của bạn.
- Bạn có thể tạo hình ảnh của riêng mình hoặc bạn chỉ có thể sử dụng những hình ảnh do người khác tạo ra và được xuất bản trong sổ đăng ký. Để xây dựng hình ảnh của riêng mình, bạn tạo Dockerfile với cú pháp đơn giản để xác định các bước cần thiết để tạo hình ảnh và chạy hình ảnh đó. Mỗi lệnh trong Dockerfile tạo ra một lớp trong hình ảnh. Khi bạn thay đổi Dockerfile và xây dựng lại hình ảnh, chỉ những lớp đã thay đổi mới được xây dựng lại. Đây là một phần tạo nên hình ảnh nhẹ, nhỏ và nhanh như vậy khi so sánh với các công nghệ ảo hóa khác.
### 4. Container
- Container là một phiên bản có thể chạy của một hình ảnh. Bạn có thể tạo, bắt đầu, dừng, di chuyển hoặc xóa một container bằng Docker API hoặc CLI. Bạn có thể kết nối một container với một hoặc nhiều mạng, đính kèm bộ lưu trữ vào container hoặc thậm chí tạo một hình ảnh mới dựa trên trạng thái hiện tại của nó.
- Theo mặc định, một container được cô lập tương đối tốt với các container khác và máy chủ của nó. Bạn có thể kiểm soát mức độ cô lập của mạng, bộ lưu trữ hoặc các hệ thống con cơ bản khác của container với các container khác hoặc với máy chủ.
- Một container được định nghĩa bởi hình ảnh của nó cũng như bất kỳ tùy chọn cấu hình nào bạn cung cấp cho nó khi bạn tạo hoặc khởi động nó. Khi một container bị xóa, bất kỳ thay đổi nào đối với trạng thái của nó không được lưu trữ trong bộ nhớ liên tục sẽ biến mất.
### 5. Docker Compose
- Docker Compose là công cụ để xác định và chạy các ứng dụng đa container. Đây là chìa khóa để mở khóa trải nghiệm phát triển và triển khai hợp lý và hiệu quả.
- Compose đơn giản hóa việc kiểm soát toàn bộ ngăn xếp ứng dụng của bạn, giúp bạn dễ dàng quản lý các dịch vụ, mạng và khối lượng trong một tệp cấu hình YAML dễ hiểu. Sau đó, chỉ bằng một lệnh, bạn có thể tạo và khởi động tất cả các dịch vụ từ tệp cấu hình của mình.

\- Compose hoạt động trong mọi môi trường; sản xuất, dàn dựng, phát triển, thử nghiệm cũng như quy trình làm việc CI. Nó cũng có các lệnh để quản lý toàn bộ vòng đời của ứng dụng của bạn:

+ Bắt đầu, dừng và xây dựng lại các dịch vụ
+ Xem trạng thái của các dịch vụ đang chạy
+ Truyền phát đầu ra nhật ký của các dịch vụ đang chạy
+ Chạy một lệnh một lần trên một dịch vụ
### 6. Docker Hub
- Docker Hub cung cấp một thư viện lớn các hình ảnh và tài nguyên được dựng sẵn, giúp tăng tốc quy trình phát triển và giảm thời gian thiết lập. Bạn có thể xây dựng dựa trên các hình ảnh dựng sẵn từ Docker Hub và sau đó sử dụng kho lưu trữ để chia sẻ và phân phối hình ảnh của riêng bạn với nhóm của bạn hoặc hàng triệu nhà phát triển khác.
- Hướng dẫn này chỉ cho bạn cách tìm và chạy một hình ảnh dựng sẵn. Sau đó, hướng dẫn bạn cách tạo một hình ảnh tùy chỉnh và chia sẻ nó thông qua Docker Hub.
