# Áp dụng nguyên tắc solid như thế nào
## Định nghĩa
 - SOLID là viết tắt của 5 chữ cái đầu trong 5 nguyên tắc:
1. Single responsibility principle
2. Open/Closed principle
3. Liskov substitution principle
4. Interface segregation principle
5. Dependency iversion principle
## Nguyên tắc trách nghiệm đơn lẻ (Single Responsibility principle)
Một class chỉ nên thực hiện một công việc. Nói cho dễ hiểu thì một class chỉ nên thực hiện một công việc, thay vì thực hiện nhiều việc trong một class thì chúng ta có thể cho mỗi class thực hiện một công việc.
Trong cuộc sống cũng vậy, nếu ta làm 2 hoặc nhiều công việc một lúc sẽ không thể có hiệu suất cao được. Như ví dụ dưới trong Blogeer phải thực hiện 2 công việc là đọc và viết nhưnng sau này chúng ta cần thêm nhiều công việc hơn nữ thì sao? Mỗi lần muốn thay đổi ta lại phải vào Blogger để sửa, chỉ cần một nhầm lẫn sẽ có hậu quả nghiêm trọng. Và để đảm bảo an toàn ta tách 2 công việc này thành 2 class khác nhau.
```
    class Blogger{
        public void writerBblog(){
            write();
        }
        public void readBBlog(){
            read();
        }
    }
```
Với việc chia nhỏ ra ta thấy có thể dễ dàng gọi đến lớp tương ứng với từng công việc, nó cũng dễ hơn khi maintain code và không phải sửa ở lớp chính quá nhiều, các đối tượng đã tách biệt hoàn toàn về nhiệm vụ
```
    class writeBlog{
        public void write(){
        }
    }
    class readBlog{
        public void read(){
        }
    }
```
## Nguyên tắc đóng mở (The Open-Closed Principle)
Theo nguyên tắc này mỗi khi ta muốn thêm chức năng cho chương trình, chúng ta nên viết class mới mở rộng class cũ bằng cách kế thừa hoặc sở hữu class cũ chứ không nên sửa đổi class cũ. Việc này dẫ đến tình trạng phát sinh nhiều class nhưng chúng ta sẽ không cần phải test lại các class cũ nữa, mà chỉ tập trung vào test các class mới, nơi chứa các chức năng mới.
Ví dụ khi trang web muốn tăng một số cơ chế nhuận bút mới chúng ta thường sẽ thêm thuộc tính cho class Blogger
```
    class Blogger{
        private string name;
        private int post;
        int paySalary(){
            if(this -> post ==3)
                return salary;
            if(this -> post == 4)
                return salary * 1.2;
            if(this -> post == 5)
                return salary * 1.5;
        }
    }
```
Theo cách làm trên hoàn toàn đúng. Tuy nhiên, nếu bạn thiết kế chương trình như thế này thì thực sự có nhiều điểm không hợp lý lắm, nếu chúng ta lại có thêm 1 kiểu nhuận bút nữa thì sao, khi đó chúng ta lại phải vào sửa lại hàm để đáp ứng được nhu cầu mới hay sao? Code mới sẽ ảnh hưởng code cũ, như vậy có khả năng là sẽ làm hỏng luôn code cũ.
Rõ ràng là chúng ta nên có một phương pháp an toàn và thân thiện hơn.
```
    class Blogger {
        private string name;
        private int post;
        int paySalary(){
            return salary;
        }
    }
    class PostedFourPost extends Blogger{
        int paySalary(){
            return salary * 1.2;
        }
    }
```
Có thể thấy rằng, cách thiết kế này làm cho lớp Blogger trở nên: Đóng với mọi sự thay đổi bên trong, nhưng luôn mở cho sự kế thừa để mở rộng sau này. Trong tương lai, khi nhu cầu mở rộng chương trình xuất hiện, có thêm nhiều đối tượng nữa cần xử lý thì chúng ta chỉ cần thêm lớp mới là sẽ giải quyết được vấn đề, trong khi vẫn đảm bảo được chương trình có sẵn không bị ảnh hưởng, nhờ đó mà hạn chế được phạm vi test, giúp giảm chi phí phát triển. Đó cũng là một trong những lợi ích ở khía cạnh dễ bảo trì sản phẩm.
## Nguyên tắc phân vùng Lislov (The Liskov Substitution Principle)
Nội dung của nguyên tắc này là: Trong một chương trình, các object của class con có thể thay thế class cha  mà không làm thay đổi tính đúng đắn của chương trình.
