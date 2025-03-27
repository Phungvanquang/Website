# Procedure



## bước 1: Xác Định Mục Tiêu Của Image

✅ Ứng dụng của bạn chạy trên ngôn ngữ lập trình nào?
➡ (Node.js, Python, Golang, PHP, Java, Ruby, Rust, v.v.)

✅ Ứng dụng có cần dependencies bên ngoài không?
➡ (Cơ sở dữ liệu, Redis, thư viện hệ thống, package từ NPM/PIP/Maven...)

✅ Làm sao để chạy ứng dụng?
➡ (Chạy bằng node app.js, python main.py, java -jar app.jar hay ./app?)

✅ Có cần biến môi trường hoặc file cấu hình không?
➡ (.env, config.json, settings.yaml, v.v.)

✅ Ứng dụng có cần compile trước khi chạy không?
➡ (Golang, Java, Rust cần biên dịch thành file nhị phân .exe, .bin...)
## bước 2 : xác định image và kiến trúc phù hợp dockerfile.
- cần chon các dịch dịch cần và có sẵn trên docker hub phù hợp với source.
## bước 3 : tiến hành viết dockerfile và viết docker-compose.yml.
## bước 4 : thực hiện câu lênh để dựng images và container.
