# Big O
Thời gian big O là ngôn ngữ và thước đo chúng ta sử dụng để miêu tả sự hiệu quả của thuật toán. Không thực sự hiểu nó có thể làm bạn gặp nhiều vấn đề trong lập trình. Cần phải làm chủ về vấn đề này.
## An Analogy
Giả sử diễn ra những việc sau:
- Bạn có 1 file trong ổ cứng cần gửi cho bạn bè đang sống ở nước ngoài và bạn cần gửi nó một cách nhanh nhất có thể. Bạn phải gửi nó như thế nào?
- Nhiều người lựa chọn sử dụng email, FTP ... Điều đó là đúng nhưng chỉ một nửa.
- Nếu là 1 file nhỏ, bạn hoàn toàn đúng. bạn có thể đi ra sân bay, bắt 1 chuyến và mất 5-10h để đến nơi.
- Nhưng điều gì sẽ xảy ra nếu file thực sự rất lớn? Có thể vận chuyển vật lý qua máy bay nhanh hơn không?
- Vâng, thực sự là như vậy. Tệp một terabyte (1 TB) có thể mất hơn một ngày để chuyển bằng điện tử. Nó sẽ là nhanh hơn nhiều chỉ để bay nó trên khắp đất nước. Nếu tệp của bạn khẩn cấp (và chi phí không phải là vấn đề), bạn có thể chỉ muốn làm điều đó.
## Time Complexity
Chúng ta có thể miêu tả data tranfer trong thuật toán như sau:
- Electronic Tranfer: O(s) Ta có thể coi s là kích thước của file. có nghĩa là thời gian vận chuyển sẽ tăng tuyến tính bới kích thước file.
- Aiplane tranfer: O(1) đây là kích thước mong muốn của file. Kích thước của file tăng lên thì nó sẽ không lấy nhiều hơn gian để nhận file của người bạn. thời gian là hằng số.
- Có nhiều thời gian chạy hơn như trên, một số kiểu phổ biến như O(log N) O(N log N).
Bạn có thể có nhiều biến trong thời gian chạy của mình, ví dụ: để sơn 1 hàng rào cao h mét, rộng w mét thì thời gian mô tả là O(wh), bạn cần sơn p lớp thì thời gian là O(whp)
##Best Case, Worst Case, and Expected Case

## Space Complexity
Thời gian không phải thứ quan trọng duy nhất trong thuật toán. Chúng ta phải quan tâm để tổng bộ nhớ- không gian cần thiết bởi thuật toán.
Space Complexity là khái niệm song song với Time Complexity. Nếu cần tạo một mảng với kích thước n, nó cần O(n) không gian bộ nhớ. Nếu cần một mảng 2 chiều kích thước n x n thì nó càn O(n^2) không gian bộ nhớ.
Không gian stack trong đệ quy cũng vậy. ví dụ đoạn có sau lấy O(n) thời gian và O(n) không gian bộ nhớ.
```
int sum(int n) {
  if(n <= 0) return 0;
  return n + sum(n -1)
}
```
miêu tả khi chạy hàm với giá trị n =4 như sau
sum(4)
   -> sum(3)
     ->sum(2)
       ->sum(1)
        ->sum(0)
Mỗi lần gọi đệ quy sẽ gọi đến bộ nhớ stack và chiếm không gian thực tế.
Tuy nhiên bạn gọi n lần không có nghĩa là chiếm O(n) không gian. Xem ví dụ dưới đây
```
int pairSumSequence(int n) {/* Ex 2.*/
    int sum = 0;
   for (inti= 0; i < n; i++) {
      sum+= pairSum(i, i + 1);
      }
      return sum;
}
int pairSum(int a, int b) {
 return a + b;
 }
```
sẽ có khoảng O(n) lệnh gọi tới hàm pairSum. Tuy nhiên các lần gọi đó ko tồn tại đồng thời trên stack nên bạn chỉ mất 0(n) không gian
## Drop the constant

Nguồn: Cracking the Coding Interview 6th Edition