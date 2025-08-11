# System & Prune.

Kiểm tra và dọn dẹp tài nguyên mà docker sử dụng : 
- image (kể cả dangling image, unused image).
- container (stopped container).
- volume (chưa mount vào container nào).
- network ( không container nào sử dụng).
- build cache (layer cache khi build image).
#### 1. Docker system df – xem dung lượng chiếm bởi image/container/volume
- hiển thị dung lượng đang bị chiếm bởi từng loại resource.
ví dụ :
```
docker system df
```
output : 
```
TYPE            TOTAL     ACTIVE    SIZE      RECLAIMABLE
Images          10        10        9.874GB   3.356GB (33%)
Containers      10        2         123.4MB   59.92MB (48%)
Local Volumes   3         3         269MB     0B (0%)
Build Cache     99        0         3.352GB   3.352GB
```
- TOTAL : tổng số resource.
- ACTIVE : đang dùng.
- SIZE : dung lượng tổng.
- RECLAIMABLE : có thể dọn dẹp.

hiển thị chi tiết từng image/container/volume : 
```
docker system df -v  # hiển thị chi tiết từng image/container/volume
```
output : 
```
Images space usage:

REPOSITORY                     TAG                  IMAGE ID       CREATED         SIZE      SHARED SIZE   UNIQUE SIZE   CONTAINERS
statement-viewer-level1-web    latest               8c82967dbb30   2 weeks ago     290MB     0B            290.1MB       1
vulnado-master-internal_site   latest               105801eef626   3 weeks ago     79.2MB    56.78MB       22.44MB       1
vulnado-master-client          latest               45437fca979e   3 weeks ago     79.5MB    56.78MB       22.72MB       1
vulnado-master-vulnado         latest               143bf79f711c   3 weeks ago     2GB       0B            2.004GB       1
postgres                       latest               3962158596da   2 months ago    621MB     0B            620.7MB       1
webgoat/webgoat                latest               3101bd9e7bcf   5 months ago    970MB     0B            969.5MB       1
mysql                          5.7                  4bc6bc963e6d   20 months ago   689MB     0B            688.7MB       1
fchipi/wordpress-xdebug        v1.5.0               a90b07c3bea7   5 years ago     767MB     0B            767.4MB       1
medicean/vulapps               s_struts2_s2-001     e4c5d7f7be47   8 years ago     518MB     0B            518.3MB       1
medicean/vulapps               b_bash_shellshock1   32e77b6919de   9 years ago     614MB     0B            614.4MB       1

Containers space usage:

CONTAINER ID   IMAGE                                 COMMAND                  LOCAL VOLUMES   SIZE      CREATED        STATUS                      NAMES
374d7714bf1e   statement-viewer-level1-web           "catalina.sh jpda run"   0               1.45MB    2 weeks ago    Exited (143) 31 hours ago   statement-viewer-level1-web-1
1acfb77e0144   medicean/vulapps:s_struts2_s2-001     "/usr/local/tomcat/b…"   0               3.9MB     2 weeks ago    Exited (143) 2 weeks ago    s2-001
1d1ea7af81a4   medicean/vulapps:b_bash_shellshock1   "httpd-foreground"       1               36.9kB    2 weeks ago    Exited (0) 2 weeks ago      shellshock1_CVE-2014-6271
7170a974432a   webgoat/webgoat                       "java -Duser.home=/h…"   0               2.99MB    3 weeks ago    Exited (143) 31 hours ago   silly_lederberg
3647a8f74323   vulnado-master-vulnado                "mvn spring-boot:run"    0               51.4MB    3 weeks ago    Exited (143) 3 weeks ago    vulnado-master-vulnado-1
02f089276efc   postgres                              "docker-entrypoint.s…"   1               16.4kB    3 weeks ago    Exited (0) 3 weeks ago      vulnado-master-db-1
edc232c7b42b   vulnado-master-internal_site          "/docker-entrypoint.…"   0               86kB      3 weeks ago    Exited (0) 3 weeks ago      vulnado-master-internal_site-1
f35c4a492908   vulnado-master-client                 "/docker-entrypoint.…"   0               86kB      3 weeks ago    Exited (0) 3 weeks ago      vulnado-master-client-1
c06890e04a77   fchipi/wordpress-xdebug:v1.5.0        "docker-entrypoint.s…"   0               63.4MB    2 months ago   Up 3 hours                  wordpress-xdebug-master-wordpress-1
cf681868e046   mysql:5.7                             "docker-entrypoint.s…"   1               28.7kB    2 months ago   Up 3 hours                  wordpress-xdebug-master-db-1

Local Volumes space usage:

VOLUME NAME                                                        LINKS     SIZE
0a3b249c93229fa266a625b5464ad3e6c22b3116c5cbbc75a697041ac6aef8ec   1         47.76MB
bbaf1a4fd10ea2cbcb51da010ab6871e7f9bde64a78dde7c19f6832011fe451d   1         221.3MB
shellshock1_CVE_2014_6271                                          1         1.37kB

Build cache usage: 3.352GB

CACHE ID       CACHE TYPE     SIZE      CREATED        LAST USED      USAGE     SHARED
xve5kg20ppx3   regular        114MB     2 months ago   2 months ago   1         false
ehhrl4qa4bou   regular        13.8MB    2 months ago   2 months ago   1         false
maj9n0ver698   regular        72.9MB    2 months ago   2 months ago   1         false
qpniei515g0b   regular        20.7kB    2 months ago   2 months ago   1         false
69ngzdw5ov6v   regular        16.3MB    2 months ago   2 months ago   1         false
gmma7zg9jzy7   regular        72.9kB    2 months ago   2 months ago   1         false
oxdu74vdkg8r   regular        426MB     2 months ago   2 months ago   1         false
0sf9c4447gfy   regular        4.13kB    2 months ago   2 months ago   1         false
hxw2j10ryge2   regular        353MB     2 months ago   2 months ago   1         false
x0i9a5afnx73   regular        406kB     2 months ago   2 months ago   2         false
7k4d29ldtrf8   regular        605kB     2 months ago   2 months ago   1         false
zkw1zz54onu9   regular        601kB     2 months ago   2 months ago   1         false
v8covwpyqlsc   source.local   8.19kB    2 months ago   2 months ago   1         false
pdckztfy21c8   source.local   8.19kB    2 months ago   2 months ago   1         false
qoofeh2ift25   regular        16.5kB    2 months ago   2 months ago   1         false
yo2vkcdsnjat   regular        474MB     2 months ago   2 months ago   1         false
koj4ac2xw66x   source.local   1.23MB    2 months ago   2 months ago   1         false
r4zy90hdxry4   regular        1.51MB    2 months ago   2 months ago   1         false
hhaczobi34br   regular        6.44MB    3 weeks ago    3 weeks ago    1         true
qlql8cgwe036   regular        20.7kB    3 weeks ago    3 weeks ago    1         true
ozoya2xol7li   regular        25.5MB    3 weeks ago    3 weeks ago    1         true
was8985gq493   regular        33.8kB    3 weeks ago    3 weeks ago    1         true
kw0dzafnkcx2   regular        4.13kB    3 weeks ago    3 weeks ago    1         true
lx8w8dd2jd6f   regular        21.1MB    3 weeks ago    3 weeks ago    1         true
qkg639rctdq5   regular        12.4kB    3 weeks ago    3 weeks ago    1         true
yc7oxdwu4zra   regular        31.6MB    3 weeks ago    3 weeks ago    1         true
a2epishsrt3b   regular        15.9MB    3 weeks ago    3 weeks ago    1         false
ijzvl7fp0i20   regular        53.7kB    3 weeks ago    3 weeks ago    1         true
c5fpkpdqyn0f   regular        37.3kB    3 weeks ago    3 weeks ago    1         true
h3g8e4kud3k5   regular        37.5kB    3 weeks ago    3 weeks ago    1         true
iilt2zbc0ju5   regular        519kB     3 weeks ago    3 weeks ago    1         false
qfdp130048ks   regular        8.22kB    3 weeks ago    3 weeks ago    1         true
x3slx6h6znx1   regular        121kB     3 weeks ago    3 weeks ago    1         true
yylsqoncq05x   regular        17.2MB    3 weeks ago    3 weeks ago    1         true
vy7yckp71ymv   regular        54.5kB    3 weeks ago    3 weeks ago    1         true
s6mp7gqt4ed0   regular        53.7kB    3 weeks ago    3 weeks ago    1         true
oil8spjwc0ec   regular        170MB     3 weeks ago    3 weeks ago    1         true
0633m436s53q   regular        1.85MB    3 weeks ago    3 weeks ago    3         false
sp9rk8sc7e63   regular        12.8MB    3 weeks ago    3 weeks ago    1         true
qexx6bj2xqkh   regular        7.26MB    3 weeks ago    3 weeks ago    1         true
5jio1g1qtbpl   regular        8.82kB    3 weeks ago    3 weeks ago    1         true
ev6f7dptq8sf   regular        13.2kB    3 weeks ago    3 weeks ago    1         true
id0mk4ba2gdg   regular        12.7kB    3 weeks ago    3 weeks ago    1         true
5hn6uri5p5p5   regular        13.5kB    3 weeks ago    3 weeks ago    1         true
3ac05ryifp1b   regular        17.8kB    3 weeks ago    3 weeks ago    1         true
rgnt8s44vkbs   regular        41.4kB    3 weeks ago    3 weeks ago    1         false
nkgi6ouhsosh   source.local   4.1kB     3 weeks ago    3 weeks ago    1         false
obxgdqdih820   source.local   12.3kB    3 weeks ago    3 weeks ago    1         false
zgo906qwtw5j   source.local   8.19kB    3 weeks ago    3 weeks ago    1         false
a7yves07j46m   source.local   8.19kB    3 weeks ago    3 weeks ago    1         false
x0gryz3u654s   source.local   176kB     3 weeks ago    3 weeks ago    1         false
w5tvydzwkktt   source.local   4.1kB     3 weeks ago    3 weeks ago    1         false
o1h7fjzzqarm   regular        330kB     3 weeks ago    3 weeks ago    1         false
gis2xdjri1or   regular        196MB     3 weeks ago    3 weeks ago    1         true
mn9dkmp03kcn   regular        18MB      3 weeks ago    3 weeks ago    1         true
lxad5zbuwaiy   regular        31.2MB    3 weeks ago    3 weeks ago    1         true
38hcrldcl8j9   regular        222MB     3 weeks ago    3 weeks ago    1         true
kccz27dtgmmn   regular        18.4MB    3 weeks ago    3 weeks ago    1         true
l60ys2uahhgj   regular        20.7kB    3 weeks ago    3 weeks ago    1         true
xyafh70b8wae   source.local   8.19kB    3 weeks ago    3 weeks ago    1         false
nu40c5g8h20j   source.local   4.1kB     3 weeks ago    3 weeks ago    1         false
nkifklsxlt36   regular        3.39MB    3 weeks ago    3 weeks ago    1         false
w5ilz631484s   regular        317MB     3 weeks ago    3 weeks ago    1         true
5q6mxwysn37d   source.local   1.97MB    3 weeks ago    3 weeks ago    1         false
lxw3rpub7zjc   regular        1.2GB     3 weeks ago    3 weeks ago    2         true
82uwkqasowo5   regular        330kB     3 weeks ago    3 weeks ago    1         true
mue2ytvo9ggb   source.local   8.19kB    3 weeks ago    3 weeks ago    1         false
62umi22zy3wl   source.local   176kB     3 weeks ago    3 weeks ago    1         false
8jxqvetzrbis   source.local   4.1kB     3 weeks ago    3 weeks ago    1         false
umjwb3xm3vtj   regular        59.1MB    3 weeks ago    3 weeks ago    2         true
gawhvdtl6tzw   source.local   4.1kB     3 weeks ago    3 weeks ago    1         false
xomiz2qkxp3e   source.local   8.19kB    3 weeks ago    3 weeks ago    1         false
loedffl5opbe   source.local   12.3kB    3 weeks ago    3 weeks ago    1         false
36rymnq76vs3   regular        41.4kB    3 weeks ago    3 weeks ago    1         true
gw3cn9mw4ga5   source.local   971kB     3 weeks ago    3 weeks ago    2         false
l4j25l5eb4mg   regular        1.56MB    3 weeks ago    3 weeks ago    2         true
ly0e446z3lax   source.local   8.19kB    3 weeks ago    3 weeks ago    2         false
u2ibqxbfs2db   source.local   4.1kB     3 weeks ago    3 weeks ago    2         false
tbbl348fb3eh   regular        162MB     2 weeks ago    2 weeks ago    1         false
d7f304xg20hl   regular        54.7MB    2 weeks ago    2 weeks ago    1         false
j938s4cd2g2d   regular        509MB     2 weeks ago    2 weeks ago    1         false
8sr3qvtadnhs   regular        493MB     2 weeks ago    2 weeks ago    1         false
2atk13je6bxt   regular        19.9MB    2 weeks ago    2 weeks ago    1         false
f3w85vs9ttmw   regular        21.3kB    2 weeks ago    2 weeks ago    1         false
vz5zi24vl1qe   source.local   8.19kB    2 weeks ago    2 weeks ago    1         false
z3ubg1op7t6t   regular        12.4kB    2 weeks ago    2 weeks ago    1         false
s2o36g31ynpa   source.local   4.1kB     2 weeks ago    2 weeks ago    1         false
zxc7fq44hax2   regular        8.28kB    2 weeks ago    2 weeks ago    1         false
pqkycs70i7x3   regular        24.9kB    2 weeks ago    2 weeks ago    1         false
8ui9qoi84yqw   source.local   840kB     2 weeks ago    2 weeks ago    1         false
z1pplhma03y2   regular        1.45MB    2 weeks ago    2 weeks ago    1         false
c7edq3kffzh5   regular        17.4kB    2 weeks ago    2 weeks ago    1         false
l8rapkcyp8n6   regular        610MB     2 weeks ago    2 weeks ago    1         false
r9s5zu9h1ua0   regular        517kB     2 weeks ago    2 weeks ago    1         true
i8zwxpe3m63t   source.local   8.19kB    3 weeks ago    2 weeks ago    4         false
rybx0d5xzmer   regular        1.85MB    2 weeks ago    2 weeks ago    1         true
egzaoep5dz80   source.local   360kB     3 weeks ago    2 weeks ago    4         false
nwadd183u0kg   regular        15.9MB    2 weeks ago    2 weeks ago    1         true
ynk4ci2mv4wr   source.local   4.1kB     3 weeks ago    2 weeks ago    4         false
```

#### 2. Docker system prune – xóa resource không dùng
- xoá :
  - container đã stopped.
  - network không dùng.
  - image không còn container nào tham chiếu.
  - build cache (nếu dùng --all hoặc --volumes)
ví dụ 1:
```
docker system prune
```
ví dụ 2: 
```
docker system prune -a --volumes
```
- `-a,  --all` : xoá cả image không dùng (kể cả untagged).
- `--volumes` : xoá luôn volume rác.

#### 3. Docker builder prune – xóa cache build
- xoá cache layer từ quá trình build image.
ví dụ :
```
docker builder prune 
```
xoá toàn bộ cache build cũ : 
```
docker builder prune --all
```
#### 4. Docker info – thông tin daemon
- hiển thị thông tin đầy đủ về docker daemon và môi trường :
```
docker info
```
- server version :
- storage driver :
- cgroup driver :
- plugins (volume, network) :
- swarm (active/inactive) :
- docker root dir (nơi docker lưu trữ dữ liệu) :
- kernel verion, OS :
#### 5. Docker version – phiên bản CLI & daemon
- hiển thi phiên bản cảu CLI và daemon.
```
docker version
```
output : 
```
Client:
 Version:           28.3.2
 API version:       1.51
 Go version:        go1.24.5
 Git commit:        578ccf6
 Built:             Wed Jul  9 16:12:31 2025
 OS/Arch:           windows/amd64
 Context:           desktop-linux

Server: Docker Desktop 4.43.2 (199162)
 Engine:
  Version:          28.3.2
  API version:      1.51 (minimum version 1.24)
  Go version:       go1.24.5
  Git commit:       e77ff99
  Built:            Wed Jul  9 16:13:55 2025
  OS/Arch:          linux/amd64
  Experimental:     false
 containerd:
  Version:          1.7.27
  GitCommit:        05044ec0a9a75232cad458027ca83437aae3f4da
 runc:
  Version:          1.2.5
  GitCommit:        v1.2.5-0-g59923ef
 docker-init:
  Version:          0.19.0
  GitCommit:        de40ad0
```
