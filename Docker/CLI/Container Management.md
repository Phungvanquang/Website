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
- 

#### 4. Docker stop / docker kill – dừng container (SIGTERM / SIGKILL)

#### 5. Docker pause / docker unpause – tạm dừng & tiếp tục container (freezer cgroups)

#### 6. Docker exec – chạy lệnh trong container đang chạy

#### 7. Docker attach – gắn terminal vào container

#### 8. Docker rename – đổi tên container

#### 9. Docker update – thay đổi resource limit khi container đang chạy

#### 10. Docker wait – chờ container kết thúc và trả về exit code
