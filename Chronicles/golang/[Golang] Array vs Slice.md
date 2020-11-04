### Array

 - Là danh sách các phần tử **cùng kiểu**
 - Khai báo
   - var arr [2]int        // [0 0]
   - a := [2]int{1, 2}     // khai báo short hand
   - a := […]int{1, 2}     // Go tự detect length
 - Size là 1 phần của kiểu dữ liệu => [3]int và [5]int là 2 kiểu dữ liệu khác nhau
 - Array không thể resize
 - Array là value type, không phải reference. Gán lại array hoặc đưa array vào hàm đều sẽ copy sang một biến thể khác, không liên quan đến biến ban đầu
 - Tính độ dài của array dùng hàm len(arr)
> [Lưu ý]: bắt buộc phải có len khi khai báo array, nếu không có len khi khai báo array thì là slice không phải array

### Slice

 - Tiện lợi, linh hoạt hơn array
 - Slice không chứa dữ liệu mà chỉ link tới 1 array chứa dữ liệu. Thay đổi trên slice là thay đổi trên array mà slice link tới
 - Khởi tao slice
   - var slice []int
   - var slice2 = arr[1:4] // Copy arr[start -> end -1] -> arr[1->3]
 - Len và capacity
   - Len là độ dài số phần tử đang giữ của slice
   - Capacity là độ dài của mảng mà slice đang link tới
 - Thêm vào slice dùng hàm append(). Khi array đầy, Go tạo một array mới dài gấp đôi array cũ và gán vào
 ### Tối ưu hóa memory
 - Việc tạo slice từ arrat thực chất là link tới array đó
 - Cùng xem ví dụ sau đây
 ```
 arr1 := [5]string{"string1", "string2", "string3", "string4", "string5"}
 slice1 := arr1[0:2]
 
 fmt.Println(slice1)
 fmt.Println("Len: ", len(slice1), ";capacity: ", cap(slice1))
 ```
- Kết quả là
```
[string1 string2]
Len:  2 ;capacity:  5
```
- Mặc dù slice1 chỉ dùng "string1" và "string2" nhưng thực tế vẫn link đến arr1. arr1 vẫn được đánh dấu là đang sử dụng nên không bị Garbage Collector của Go dọn nên tốn ram
- Cách tốt nhất:  dùng hàm copy([]dest, []source) để copy sang 1 slice khác.
```
 arr1 := [5]string{"string1", "string2", "string3", "string4", "string5"}
 arr2 := arr1[0:2]
 slice1 := make([]string, len(arr2))
 copy(slice1, arr2)

 fmt.Println(slice1)
 fmt.Println("Len: ", len(slice1), ";capacity: ", cap(slice1))
```
Nguồn [[Golang] Array và slice](https://minhphong306.wordpress.com/2020/03/26/golang-array-va-slice/)
