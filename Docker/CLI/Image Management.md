# Image Management.

#### 1. Docker pull – tải image.
- docker CLI gửi request tới docker registry (mặc định là docker hub).
- Image được tải về theo từng layer (dựa trên sha256 checksum) --> layer nào tồn tại trong local thì không tải lại.
- lưu ở `/var/lib/docker/<storage-driver>/`.
ví dụ :
```
# pull latest tag
docker pull nginx
# pull tag cụ thể
docker pull nginx:1.25-alpine
# pull từ registry khác.
docker pull mỷegistry.local:5000/myimage:latest
```
#### 2. Docker build – build image từ Dockerfile.
- docker deamon đọc dockerfile và context (thư mục build).
- môi instruction (FROM, RUN, COPY...) tạo ta 1 layer mới.
- layer được cache để tăng tốc build.
ví dụ :
```
docker build -t myapp:1.0
docker build --no-cache -t myapp:clean .
```
-- > sắp xếp lệnh trong docker file để tận dụng layer cache.Sử dụng `.dockerignore` để giảm context --> build nhanh hơn.

#### 3. Docker commit – tạo image từ container đang chạy (ít dùng, khó reproduce).
- snapshot filesystem của container --> tạo image mới.
- không ghi lại dockerfile, nên khó tái tạo môi trường sau này.
ví dụ :
```
docker commit mycontainer myimage;debug
```
- chỉ dùng để debug nhanh hoặc chụp lại trạng thái tạm thời
- không dùng cho production vì thiếtinhs reproducible.

#### 4. Docker images – liệt kê image.
-liệt kê image trong local registry cache.
- thông tin gồm repository, tag, image ID, kích thước , ngày tạo.
ví dụ :
```
dokcer images
docker images --filter dangling=true  # image "mồ côi"
```
#### 5. Docker inspect (image) – xem metadata image.
- hiển thị JSON chứa cấu hình : ENV mặc định, CMD, ENTRYPOINT, layer history.
ví dụ :
```
docker inspect nginx:latest
docker inspect -f '{{ .Config.Entrypoint }}' nginx
```
#### 6. Docker rmi – xóa image.
- xoá metadata và layer không còn container nào sử dụng.
- nếu layer vẫn đang dùng trong container khác --> không xoá được.
ví dụ :
```
docker rmi nginx:alpine
docker rmi $(docker images -q)   # xóa toàn bộ image
```
#### 7. Docker tag – đặt thêm tag cho image.
- chỉ thêm metadata tag trỏ tới cùng image ID, không copy dữ liệu.
ví dụ :
```
docker tag myapp:1.0 mỷegistry.local/myapp:prod
```
#### 8. Docker save / docker load – export/import image dạng tar.
- `save` : đóng gói toàn bộ image _metadata thành file .tar
- `load` : giải nén tar và load vào local image store.
#### 9. Docker import / docker export – từ file system container.
- `export` : chỉ lưu filesystem container (không giữ metadata như ENV, CMD).
- `import` : tạo image từ filesystem đó.
ví dụ :
```
docker export mycontainer -o fs.tar
docker import fs.tar myimage:bare 
```
