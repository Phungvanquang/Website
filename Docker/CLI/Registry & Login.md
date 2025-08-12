# Registry & Login.

- registry là nơi lưu trữ và phân phối image.
- docker hub là registry mặc định của docker (public + private repo).Các registry :AWS ECR, GCP Artifact Registry, Azure ACR, Github Container Registry, Harbor, Nexus.

#### 1. docker login / logout – đăng nhập registry.

Đăng nhập vào 1 Registry để có thể push/pull các image private.

```
docker login
```
CLI sẽ yêu cầu : 
```
Username :....
Password : .....
```

#### 2. docker push – đẩy image lên registry.

để push được , bạn cần : 
1. đăng nhập thành công.
2. tag image đúng format.

```
[registry_host]/[namespace]/[image_name]:[tag]
```

nếu registry là docker hub và user là myuser.
```
docker tag myimage:latest myuser/myimage:1.0
docker push myuser/myimage:1.0
```

nếu registry tự host : 

```
docker tag myimage:lattest mỷegistry.example.com/team/myimage:1.0
docker push mỷegistry.example.com/team/myimage:1.0
```


#### 3. docker search – tìm image trên Docker Hub.

Tìm kiếm trực tiếp từ CLI (chỉ hỗ trợ Docker Hub):
```
docker search nginx
```
Output:

```
NAME                               DESCRIPTION                                     STARS   OFFICIAL   AUTOMATED
nginx                              Official build of Nginx.                        17000   [OK]
jwilder/nginx-proxy                Automated Nginx reverse proxy for docker... 
```
