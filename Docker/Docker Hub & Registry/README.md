# Docker Hub & Registry (mới thêm)

#### 1. Search & pull image.

1.1. docker search.
```
docker search nginx
```
- tìm kiếm các image public trên docker hub.

1.2. kéo image public về docker hub.
```
docker pull nginx:1.25
docker pull registry.example.com/team/myapp:latest
```

#### 2. Tag & Push image

2.1. docker tag - gán tên và tag cho image.
```
docker tag myimage:latest myuser/myimage:v1.0
```

2.2. docker push - đẩy image lên registry.
```
docker push myuser/myimage:1.0
```

#### 3. Private Registry Basics.
- registry mặc định : docker hub.
- Private registry : cho phép lưu trữ image riêng, không công khai.
