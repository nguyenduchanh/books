# Áp dụng nguyên tắc solid như thế nào
## Định nghĩa
 - SOLID là viết tắt của 5 chữ cái đầu trong 5 nguyên tắc:
1. Single responsibility principle
2. Open/Closed principle
3. Liskov substitution principle
4. Interface segregation principle
5. Dependency iversion principle
## Nguyên tắc trách nghiệm đơn lẻ (Single Responsibility principle)
Nội dung: Một class chỉ nên thực hiện một công việc duy nhất. Nói cho dễ hiểu thì một class chỉ nên thực hiện một công việc, thay vì thực hiện nhiều việc trong một class thì chúng ta có thể cho mỗi class thực hiện một công việc.
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
Nội dung: Có thể thoải mái mở rông 1 class nhưng không được sửa đổi bên trong class đó
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
Ví dụ khi ta muốn viết một chương trình để mô tả các loài chim bay được nhưng chim cánh cụt không bay được. Vì vậy khi đến hàm chim cánh cụt thì khi gọi hàm bay của chim cánh cụt, ta sẽ quảng NoFlyException.
```
    public class Bird{
        public virtual void Fly(){Console.Write("Fly");}
    }
    public class ChimUng: Bird{
        public override void Fly(){Console.Write("Eagle Fly");}
    }
    public class ChimCanhCut: Bird{
        public ovveride void Fly(){throw new NoFlyException();}
    }
```
    Tuy nhiên quya lại vòng lặp for ở hàm main, nếu như trong danh sách các con chim đó mà có một con chim cánh cụt thì sao? Chương trình sẽ quăng Exception vì chương trình của chúng ta đã vi phạm Liskov substitution principle.
    Để giải quyết vấn đề này ta sẽ tách class chim cánh cụt ra một interface riêng.
## 4. Nguyên tắc phân tách giao diện (Interface Segregation Principle)
Nội dung: Nếu intergace quá lớn thì nên tách thành các interface nhỏ hơn, với nhiều mục đích cụ thể.
Khi học đại học những năm đầu có thể sẽ học chung một số môn. Nhưng sau một thời gian các môn học của mỗi người sẽ khác nhau và tùy thuộc vào ngành mỗi sinh viên, nhưng tất cả đều học, do vậy dùng interface là hợp lý. Lúc thiết kế, bạn nghĩ tói việc thiết kế  một interface là Study có các function học, do chỉ có duy nhất một sự khác biệt nên bạn nhé hết tất cả các môn học vào cùng 1 interface này luôn.
```
    interface Study{
        function studyEnglish();
        function studyProgramingLanguage();
    }
    class NormalStudent implements Study{
        function studyEnglish(){
        }
        function studyProgrammingLanguage(){
            return NULL;
        }
    }
    class InformationTechnologyStudents implements Study{
        function studyEnglish(){}
        function studyProgrammingLanguage(){}
    }
```
Tuy thiết kế chương trình như vậy vẫn chạy bình thường nhưng khi ta muốn thêm một số môn học mới.Chúng ta nhận ra rằng bởi vì sự khác biệt ngày càng nhiều, rất có thể sau này sẽ phát sinh nhiều môn học tiêng cho từng hệ sinh viên nữa, do ddos chúng ta nên có cách giải quyết triệt để hơn, bởi vì nếu cứ viết chung vào interface Study thì sẽ phát inh việc phải implement nhiều hàm ko cần thiết. Do vậy, chúng ta quyết dịnh tách việc học thành các interface cụ thể khác nhau, phần học chung, phần học riêng.
```
    interface Study{
        function studyEnglish();
        function studyMath();
    }
    interface InformationInfTechnologyStudy{
        function studyProgrammingLanguage();
    }
    interface EconomicsStudy{
        function studyPilosophy();
    }
    class EconomicsStudent implements Study, EconomicStudy{
        function studyEnglish(){
        }
        fucntion studyMath(){}
        function studyPhilosophy(){}
    }
    class TechnologyStudent implements Study, InformationInfTechnologyStudy{
        function studyEnglish(){}
        function studyMath(){}
        function studyProgrammingLanguage(){}
    }
```
Với thiết ké này chúng ta ko cần phải lo lắng tời việc phải implement những hàm ko cần thiết, cũng sẽ dễ dàng kiểm soát được việc mở rộng hơn. Trong tương lai nếu phát sinh nhiều môn học riêng, việc triển khai cũng dễ dàng và tường minh hơn
## 5. Nguyên tắc đảo ngược phụ thuộc (Depe)ndency inversion principle)
Nội dung: Các module cấp cao không nên phụ thuộc vào các module cấp thấp. Cả 2 nên phụ thuộc vào abstraction. Interface (abstraction) không nên phụ thuộc vào chi tiết mà ngược lại (Các class giao tiếp với nhau thông qua interface, không phải thông qua implementation).
ví dụ: Khi ta thiết kế một chương trình gửi email đến user
```
    public class EmailSender{
        public void SendEmail(){
            // Send Email
        }
    }
    public class Notification
    {
        private EmailSender _email;
        public Notification(){
            _email = new EmailSender();
        }
        public void Send(){
            _email.SendEmail();
        }
    }
```
Nhưng bây giờ khách hàng muốn gởi cả SMS và Emai đến cho user thì phải làm sao?
Tạo một lớp SMSSender và chỉnh sửa cả class Notification. Như vậy sẽ vi phạm 2 nguyên tắc là Dependency Inversion về sự đảo ngược phụ thuộc và Open Close principle. Sofware đóng cho việc thay đổi nhưng mở cho việc mở rộng
Để thỏa mãn hai nguyên tắc trên, bạn phải refactoring code theo chiều hướng giảm sự phụ thuộc cứng bằng cách tạo ra một interface ISender dùng chung giữa hai class EmailSender và SMSSender
```
    public class EmailSender: ISender
    {
        public void Send(){
            // Send email
        }
    }
    public class SMSSender: ISender{
        public void Send(){
            // Send SMS
        }
    }
    public class Notification{
        private ICollection<ISener> _sender;
        public Notification(ICollection<ISender> senders){
            _sender = sender;
        }
        public void Send(){
            foreach(var message in _senders){
                message.Send();
            }
        }
    }
```
Giờ đây class Notification phụ thuộc mềm vào interface ISender, nếu khách hàng yêu cầu thêm một phương thức để chuyển tin nhắn ta có thể thêm dễ dàng bằng cách sử dụng interface ISender
# Tổng kết
## Single responsibility principle
Một class chỉ nên giữ 1 trách nhiệm duy nhất 
(Chỉ có thể sửa đổi class với 1 lý do duy nhất)
## Open/closed principle
Có thể thoải mái mở rộng 1 class, nhưng không được sửa đổi bên trong class đó 
(open for extension but closed for modification).
## Liskov Substitution Principle
Trong một chương trình, các object của class con có thể thay thế class cha mà không làm thay đổi tính đúng đắn của chương trình
## Interface Segregation Principle
Thay vì dùng 1 interface lớn, ta nên tách thành nhiều interface nhỏ, với nhiều mục đích cụ thể
## Dependency inversion principle
 Các module cấp cao không nên phụ thuộc vào các modules cấp thấp. 
   Cả 2 nên phụ thuộc vào abstraction.
 Interface (abstraction) không nên phụ thuộc vào chi tiết, mà ngược lại.
( Các class giao tiếp với nhau thông qua interface, 
không phải thông qua implementation.)