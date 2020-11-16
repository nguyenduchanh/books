# Concurrency
- Concurrency gần giống với thread. Các goroutines giao tiếp với nhau thông qua channel, truyền dữ liệu giưã các goroutines bằng bất cứ loại dữ liệu nào.

- Mỗi Goroutines chỉ sử dụng 2kb bộ nhớ Heap, như vậy có thể tạo hàng triệu Goroutines bất cứ lúc nào
- Go khuyến khích cách tiếp cận mỗi thread chỉ access đến giá trị chia sẻ tại đúng 1 thời điểm nhất định và giá trị chia sẻ này được truyền giữa các thread thông qua các kệnh giao tiếp.
## Goroutines
- *Goroutines* là một luồng được quản lý bở Goroutines. Có thể hiểu đơn giản goroutine là một hàm được chạy đồng thời cùng các hàm khác
## Channel
- Channel giống như 1 chiếc hộp 2 đầu. 1 đầu truyền và 1 đầu nhận dữ liệu.
- Khai báo Unbuffered channel: a := make(chan int)
- Khai báo Buffered channel: a:= make(chan int, 10)
### Khác nhau giữu Unbuffered channel và Buffered channel
- Unbuffered channel không có khoảng trống để chứa dữ liệu, yêu cầu cả 2 goroutines gửi và nhận đều phải sẵn sáng cùng 1 lúc. Khi 1 goroutine gửi dữ liệu vào channel, luồng xử lý sẽ block lại cho tới khi có 1 goroutine đọc từ channel này ra.
