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
 
     


 
