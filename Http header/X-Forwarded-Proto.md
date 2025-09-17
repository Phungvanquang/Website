# X-Forwarded-Proto

- X-Forwarded-Proto là sử dụng để xác định các giao thực (http hoặc https) mà máy khác sử dụng để kết nối với proxy hoặc cân bằng tải.

- - Nhật ký truy cập máy chủ chứa giao thức được sử dụng giữa máy chủ và cân bằng tải, nhưng không chứa giao thức được sử dụng giữa máy khách và cân bằng tải. Để xác định giao thức được sử dụng giữa máy khách và cân bằng tải, bạn có thể sử dụng tiêu đề yêu cầu.X-Forwarded-Proto


    **X-Forwarded-Proto: <protocol>**
