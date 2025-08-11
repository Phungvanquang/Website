# Container Inspection & Logs.

#### 1. Docker ps – liệt kê container.
- liệt kê các danh contianer metadata store.
- thông tin bao gồm : ID, Image, command , trạng thái, cổng , tên.
ví dụ :
```
# liệt kê container đang chạy
docker ps
# liệt kê tất cả các contianer.
docker ps - a
# chỉ hiển thị ID
docker ps -q
# lọc theo trạng thái.
docker ps -f status=exited
```

#### 2. Docker inspect – xem thông tin JSON đầy đủ
- truy cập metadata dạng JSON từ deamon (bao gồm config, network settings, volkume mounts, state).
ví dụ :
```
# docker container
docker inspect <container>
# inspect image .
docjer inspect <image>
# chỉ lấy IP address
docker inspect -f '{{ .NetworkSettings.IPAddress }}' <container>
```
#### 3. Docker logs – xem log container
- đọc log từ logging driver ( mặc địng json-file ,lưu ở ```/var/lib/docker/containers/<id>/<id>-json.log```).
ví dụ :
```
# xem toàn bộ log
docker logs <container>
# xem log realtime
docker logs -f <contianer>
# xem log 10 dòng cuối
docker logs --tail 10 <contianer>
# xem log từ thời điểm cụ thể
docker logs --since 2023-08-09T15:00:00 <container>
```
#### 4. Docker top – xem process trong container
- liệt kê process trong container bằng ps từ host , log theo namespace của container.
vi dụ :
```
docker top <contianer>
```
#### 5. Docker stats – realtime CPU/mem/IO usage
- docker deamon đọc số liệu từ cgroups (CPU, memory,  blkio) và container network interface.
ví dụ :
```
docker stats
docker stats <container> db
```

#### 6. Docker events – stream sự kiện từ daemon
- lắng nghe docker event stream từ deamon.
- sự kiện bao gồm container create/start/stop, network connect/disconnect, image pull/remove.
ví dụ :
```
# stream toàn bộ sự kiện.
docker events
# lọc theo loại object.
docker events --filter type=container
# lọc theo hành động
docker event --filter event=start
```
