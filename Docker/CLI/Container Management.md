# Container Management

#### 1. Docker run – tạo & chạy container
- là sự kết hợp `docker create` + `docker start`.câu lênh bắt đầu luôn :  `docker run....`

ví dụ : 
```
 docker run -d --name web -p 8080:80 nginx:alpine
```
  - `-d` :chạy nền.
  - `--name` : là tên của container muốn đóng.
  - `-p` : là port nguồn và port đích.
  - `nginx:alpine` :tên image.
    
#### 2. Docker create – chỉ tạo, chưa chạy
- chỉ thực hiệno bước `container config` + `filesystem` mà không chạy process.
ví dụ :
```
docker create --name db redis:latest
docker start db
```
#### 3. Docker start / docker restart – khởi động lại container đã tạo
- start : khởi động container đã tạo / dừng trước đó ( khôi phục process ).
- restart : stop --> start ( thường dùng khi cần reload cấu hình ).
ví dụ :
```
docker start web
docker restart web
```

#### 4. Docker stop / docker kill – dừng container (SIGTERM / SIGKILL)
- stop : gửi tín hiệu sigterm , chờ thời gian --time (mặc định 10s) rời gửi sigkill.
- kill : gửi sigkill ngay lập tức (hoặc tín hiệu khác nếu chỉ định). Dùng để treo process , không phản hồi.
ví dụ :
```
docker stop web
docker kill web
docker kill signal SIGUSR1 web
``` 

#### 5. Docker pause / docker unpause – tạm dừng & tiếp tục container (freezer cgroups)
- pause : đóng băng process bằng cgroups freeezer subsystem --> dừng hoàn toàn việc thực thi nhưng không giải phóng tài nguyên.
- unpause : tiếp tục.
ví dụ :
```
docker pause web
docker unpause web
```

#### 6. Docker exec – chạy lệnh trong container đang chạy
- tạo process mới trong cùng namespace với process chính của contianer.
ví dụ :
```
docker exec -it web sh
docker exec web ls /usr/share/nginx/html
```

#### 7. Docker attach – gắn terminal vào container
- nối STDIN/STDOUT/STDERR của contianer với terminal của bạn.
- dùng khi container chạy Foreground app.
ví dụ :
```
docker attach web
```

#### 8. Docker rename – đổi tên container
ví dụ : 
```
docker rename web old_web
```

#### 9. Docker update – thay đổi resource limit khi container đang chạy
- điều chỉnh cgroups (CPU, RAM,blkio) runtime.
ví dụ :
```
docker update --cpus=1.5 --memory=512m web
```

#### 10. Docker wait – chờ container kết thúc và trả về exit code
- block CLI đến khi container dùng , trả về exit code.
ví dụ :
```
docker wait web
```
kết hợp trong script để chạy step tiếp theo khi contianer kết thúc.
