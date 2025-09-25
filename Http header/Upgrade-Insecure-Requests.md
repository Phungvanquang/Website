# Upgrade-Insecure-Requests



- Upgrade-Insecure-Requests gửi tín hiệu đến máy chủ cho biết tuỳ chọn của máy khác đối với phản hồi được mã hoá và xác thực máy khách có thể xử lí thành công lênh CSP.

**Cú pháp  Upgrade-Insecure-Requests : 1**

- thường là số 1 nếu còn không sẽ không có header này.
  
`Nếu có số 1 thì nội dùng đang dùng HTTp, vui lòng cung cấp bản HTTPS nếu có.`

--> nếu server không có HTTPS, nó có thể phản hồi lại bằng các 
- trả về link HTTPS cho HTTP.
- hoặc chèn header **Content-Security-Policy: upgrade-ínecure-requests** để ép trình duyệt tự động chuyển các requests HTTP thành HTTPS
