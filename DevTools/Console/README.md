# Console

- nhằm tương tác với javscript trong trình duyệt.

#### 1. Console - Developer.
- chạy javascript trực tiếp trên Internet.
- xem log, error, waring, info từ script.
- gỡ lỗi js runtime.

###### `Các lệnh :` 
| Lệnh                                     | Mục đích                       | Ví dụ                                                                                     |
| ---------------------------------------- | ------------------------------ | ----------------------------------------------------------------------------------------- |
| `console.log()`                          | Hiển thị thông tin bình thường | `console.log('Hello World')`                                                              |
| `console.error()`                        | Hiển thị lỗi, màu đỏ           | `console.error('Something went wrong')`                                                   |
| `console.warn()`                         | Cảnh báo, màu vàng             | `console.warn('Be careful')`                                                              |
| `console.info()`                         | Thông tin, màu xanh            | `console.info('Info message')`                                                            |
| `console.table()`                        | Hiển thị dữ liệu dạng bảng     | `console.table([{name:'A', age:20}, {name:'B', age:25}])`                                 |
| `console.group()` / `console.groupEnd()` | Gom log theo nhóm              | `console.group('Users'); console.log('User1'); console.log('User2'); console.groupEnd();` |
| `console.time()` / `console.timeEnd()`   | Đo thời gian thực thi          | `console.time('fetch'); fetch('/api'); console.timeEnd('fetch');`                         |
###### `Advanced Developer Tips.`
- Inspect element trực tiếp: console.dir($0) → $0 là element đang chọn trong Elements tab.
- Trigger JS events: document.querySelector('button').click() để simulate click.
- Monitor function calls:
```
monitor(functionName)
unmonitor(functionName)
```
- Break on exception: debug(functionName) → dừng khi hàm được gọi.
