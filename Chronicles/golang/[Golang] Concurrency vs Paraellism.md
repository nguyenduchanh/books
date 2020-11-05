# Phân biệt concurrency và parallelism
- Concurrency là đồng thời có nhiều công việc cần xử lý. Chỉ xử lý 1 việc duy nhất 1 thời điểm, switch giữa các việc sao cho hạnh chế có thời gian rảnh nhất.
- Parallelism là xử lý song song các công việc, có thể xử lý được nhiều việc 1 lúc.
- Parallelism chưa chắc đã nhanh hơn concurrency do sau khi xử lý xong cần giao tiếp với luồng chính, bước giao tiếp này có thể tốn nhiều thời gian.
- Golang là ngôn ngữ concurrency, hỗ trợ thông qua Goroutines.
- Parallelism thường được thực hiện trên nhiều core của CPU. Sau khi thực hiên xong các task, kết quả sẽ được đẩy về core đang xử lý chương trình chính, việc truyền dữ liệu giữa các core khá tốn tài nguyên, thậm chí tốn hơn cả thời gian làm trên 1 core.

## Goroutines
- Kích thước nhỏ: khi khởi tạo goroutines chỉ tốn vài kb trong vùng nhớ stack, có thể resize được. Thread thì tốn khoảng 2MB khi khởi tạo và không resize được.
- Có thể chạy nhiều Goroutines trong 1 OS thread. Nếu thread này bị block, 1 thread mới được tạo ra. Một vài goroutines ở lại thread cũ để xử lý tiếp, số goroutines còn lại chuyển qua thread mới để process tiếp. Cái này là do Golang tự xử lý.
- Xử lý đọc/ghi dữ liệu cùng lúc giữa các vùng nhớ chung đơn giản thông quan channel.
- Start 1 goroutines bằng cách đặt từ khóa 'go' trước method/function

## Xử lý race condition bằng atomic, mutex
- *Race condition*: là hiện tượng nhiều tiến trình cùng truy cập và muốn thay đổi giá trị của biến, nhưng không theo quy tắc nào khiến kết quả không như mong muốn.
- Golang xử lý race condition bằng việc dùng atomic,muctex hoặc channel.
- *Waitgroup*: là công cụ để quản lý luồng chạy của goroutines: go routines chính đợi cho tới khi các goroutines con có tín hiệu xử lý xong mới chạy tiếp.
- *Atomic*: package của Go dùng để đảm bảo tại 1 thời điểm, chỉ có 1 tiến trình duy nhất được đọc/ghi giá trị vào 1 biến. Atomic chỉ support các kiểu số nguyên (int32, int64, uint32....)

- *Mutex*: gần giống với Atomic. khác ở chỗ atomic chỉ lock 1 biến còn mutext lock cả 1 đoạn code.
- *Channel*:...

### Wait Group
- *Wait Group*: thường được dùng để đợi một tập các goroutines hoàn thành xong mới cho goroutines chính chạy tiếp.
- Tưởng tương *Wait Group* như bộ đếm vậy, khi nào giá trị bộ đếm về 0 thì coi như xong không cần đợi nữa.
#### Ví dụ 1: 
```
package main
import (
	"fmt"
	"sync"
	"time"
)
func test1(wg *sync.WaitGroup) {
	fmt.Println("Chạy hàm test 1")
	// Oánh dấu đã done
	wg.Done()
}
func test2(wg *sync.WaitGroup) {
	fmt.Println("Chạy hàm test 2")
	// Oánh dấu đã done
	wg.Done()
}
func main() {
	// Khai báo 1 wait group
	var wg sync.WaitGroup
	// Set giá trị counter = 2
	wg.Add(2)
	go test1(&wg)
	go test2(&wg)
	// Block main goroutines chạy tiếp, khi nào counter = 0 thì mới cho chạy q
	wg.Wait()
	fmt.Println("Good bye")
}
```
- Trong ví dụ trên ta khởi tạo 1 *Wait Group* với giá trị bộ đếm là 2: *wg.Add(2)*, tương đương  với 2 goroutines
- Sau đó, trong các goroutines, khi xử lý xong sẽ gọi hàm *Done()* để giảm giá trị counter đi
- Khi giá trị của counter > 0, hàm *wg.Wait()* sẽ block goroutines đang gọi hàm này lại. Khi nào counter = 0 thì mới nhả ra để chạy tiếp.
#### Ví dụ 2
```
package main

import (
	"fmt"
	"sync"
)

var x int64 = 0

func addOne(wg *sync.WaitGroup) {
	x = x + 1
	wg.Done()
}
func main() {
	var w sync.WaitGroup
	for i := 0; i < 1000; i++ {
		w.Add(1)
		go addOne(&w)
	}
	w.Wait()
	fmt.Println("Giá trị của x là: ", x)
}
```
- Ví dụ trên tạo 1000 goroutines khác nhau. Mỗi goroutines có nhiệm vụ tăng giá trị x thêm 1. Kết quả mong muốn là 1000. nhưng kết quả nhận được lúc là 1000, líc là 981 ...
- Nguyên dân là đoạn code trên bị race condition, tức là  tình trạng các tiến trình cùng truy cập 1 tài nguyên cà cùng thay đổi chúng. nhưng việc truy cập lại không có trình tự, dẫn đên kết qủa sai lệch với mong muốn.

                |    |       G1          |   x   |         G2         |
                |    | ------------------|       | -------------------|
                | 1  |        0         <--  0  -->        0          |  đọc
                | 2  |      0 + 1 = 1    |   0   |      0 + 1 = 1     |
                | 3  |        1         <--  1  -->        1          |  ghi
- 