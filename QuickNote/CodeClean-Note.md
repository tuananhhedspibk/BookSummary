# Sơ lược về Code Complete :rocket: :green_book: :memo:

## 1. Mở đầu
  + Ngày nay theo như quy mô lớn hơn của project, mô hình tương tác Agile giữa các dev đòi hỏi cần phải code đẹp hơn, dễ đọc, dễ bảo trì
  + Khi code quá lùm xùm, code xấu thì đến khi có bug, hay khách hàng muốn thêm chức năng thì sẽ rất khó khi đọc lại code cũng như fix bug, scale ứng dụng

## 2. Thế nào là code đẹp
  + Để viết được code đẹp ta cần có cảm giác về code, cảm giác về tính sạch đẹp của code
  + Có thể nôm na code đẹp là như sau:
    - Là các đoạn code dễ đọc
    - Dễ dàng hiển thị ra ý đồ của người viết code
    - Logic rõ ràng
    - Sự phụ thuộc lẫn nhau giữa các thành phần được tối thiểu hóa để dễ dàng bảo trì

## 3. Để viết code đẹp hơn

### 3.1. Đặt tên có ý nghĩa
  + Sử dụng tên thể hiện rõ ý đồ
    - Chọn tên đúng có thể mất thời gian suy nghĩ nhưng nó sẽ có lợi về lâu về dài
    - Vì thế nên chọn tên cẩn thận và đổi tên khi tìm được từ tốt hơn
    - Tên của biến, hàm, class phải nói lên
      * Nó là gì
      * Nó làm gì
      * Sử dụng như thế nào
    - Nếu tên biến cần chú thích thì mới hiểu được thì đó là tên chưa thể hiện được ý đồ

    ```java
      // Tên chưa tốt:
      int d; // elapsed time in days

      // Tên tốt:
      int elapsedTimeInDays;
      int daysSinceCreation;
    ```
  + Sử dụng tên có thể phát âm được, tìm kiếm được
    - Tìm kiếm dễ dàng và nhớ nhanh hơn

    ```java
      // Sử dụng:
      class Customer {
        private Date generationTimestamp;
        private Date modificationTimestamp;;
        private final String recordId = "102";
        /* ... */
      };
      
      // Thay vì:
      class DtaRcrd102 {
        private Date genymdhms;
        private Date modymdhms;
        private final String pszqint = "102";
        /* ... */
      };
    ```
  + Tránh mã hóa
    - Mã hóa tên biến chỉ làm chúng ta mất công giải mã
    - Một cách mã hóa phổ biến trước đây thường dùng là: "Hungarian Notation"
			* Thêm 1 vài chữ cái biểu thị kiểu trước tên biến: txtName, iAge
    - Việc mã hóa chỉ làm cho việc đổi tên biến, hàm, class trở nên khó hơn, đọc code cũng vướng víu hơn
  + Tránh mental mapping
    - Hạn chế việc người đọc code phải dịch tên mà bạn đặt ra 1 tên khác mà họ biết
    - Vấn đề này có thể xảy ra khi sử dụng những tên không nằm trong domain của bài toán hoặc những tên khác với tư duy thông thường
    - Ví dụ như đặt tên biến chỉ có 1 chữ cái hay sử dụng các hằng số magic

    ```java
      int i, j;
      int secondsInADay = 24 * 60 * 60;
			
      // Viết lại đoạn trên một cách rõ ràng hơn:

      static final int SECONDS_IN_A_MINUTE = 60;
      static final int MINUTES_IN_AN_HOUR = 60;
      static final int HOURS_IN_A_DAY = 24;
      
      int numberOfEmployees, numberOfRooms;
      int secondsInADay = HOURS_IN_A_DAY * MINUTES_IN_AN_HOUR * SECONDS_IN_A_MINUTE;
    ```
  + Tên class
    - Class và các objects nên có tên là các danh từ hoặc cụm danh từ: Customer, WikiPage, Account
    - Hạn chế các từ như: Manager, Processor, Data, Info khi đặt tên class & đối tượng
  + Tên hàm
    - Nên được đặt bằng động từ hoặc cụm động từ: postPayment, deletePage, save

### 3.2. Hàm
  + Ngắn gọn và làm 1 việc duy nhất
    - Hàm phải ngắn gọn nhất có thể
    - 1 hàm lí tưởng viết không quá 20 dòng
    - Về nguyên tắc, nếu hàm quá dài ta sẽ chia nhỏ hàm ra thành từng phần nhỏ, mỗi phần lại thực hiện 1 chức năng duy nhất, tên cũng nên thể hiện rõ tác dụng duy nhất của nó
  + Tham số
    - Số lượng tham số không nên quá 2 (lí tưởng)
    - Nếu quá nhiều nên đóng gói các tham số liên quan thành 1 class thích hợp

    ```java
      Circle makeCircle(double x, double y, double radius);
      Circle makeCircle(Point center, double radius);
    ```
    - Không nên sử dụng tham số ra, vì chúng ta đã quen với việc nhận thông tin từ tham số đầu vào và trả về qua return
  + Không có tác dụng phụ
    - Hàm không nên thực hiện 1 việc nào khác ngoài nội dung thể hiện qua tên của nó
    - Như thế sẽ gây ra những lỗi khó hiểu do hàm thực hiện các hành vi ngoài hiểu biết của người dùng
  + Tách biệt giữa hành động và truy vấn
    - Mỗi hàm chỉ nên thực hiện hành động hoặc trả lời câu hỏi
    - Thực hiện cả 2 sẽ gây khó hiểu cho người đọc
  + Don't Repeat Yourself (DRY)
    - Luôn tách các đoạn code giống nhau thành các hàm riêng biệt để dễ dàng tái sử dụng và bảo trì

## 4. Comment

### 4.1. Comment không thể chữa code xấu
  + Khi module viết lộn xộn đừng nên comment, hãy viết lại code
  + Code rõ ràng, ít comment tốt hơn nhiều so với code rắc rối, tùm lum comment
  + Thay vì mất thời gian viết comment, giải thích code hãy viết lại code
  + Thay comment bằng các hàm, biến với tên thể hiện rõ ý đồ mong muốn

### 4.2. Comment code không dùng
  + Có những đoạn code không sử dụng ta comment và nghĩ rằng sau này có thể dùng lại
  + Đừng nên làm như vậy vì nó sẽ khiến việc đọc code rối rắm, hãy xóa nó đi
  + Sử dụng các source control như Git sẽ giúp ta có thể xem lại lịch sử file bất cứ lúc nào

## 5. Định dạng

### 5.1. Độ mở theo chiều dọc giữa các nhóm code
  + Hầu như code được đọc từ trái sang phải, từ trên xuống dưới
  + Mỗi dòng code thể hiện 1 ý đồ nhất định
  + Một nhóm các dòng code thể hiện 1 suy nghĩ và có sự liên quan giữa các dòng code
  + Các suy nghĩ đó nên được tách biệt với nhau bằng 1 khoảng trắng

  ```java
    package fitnesse.wikitext.widgets;
    
    import java.util.regex.*;
    
    public class BoldWidget extends ParentWidget {
      public static final String REGEXP = "'''.+?'''";
      private static final Pattern pattern = Pattern.compile("'''(.+?)'''",
        Pattern.MULTILINE + Pattern.DOTALL
      );
    }
  ```
### 5.2. Khoảng cách giữa các thành phần liên quan
  + Các thành phần có mối liên quan mật thiết tới nhau nên được đặt gần nhau
  + Tránh trường hợp phải cuộn lên cuộn xuống hay chạy từ file này sang file khác chỉ để xem các hàm gọi đến nhau như thế nào

#### 5.2.1. Khai báo biến
  + Biến nên được khai báo gần vị trí sử dụng hết mức có thể
  + Vì hàm rất ngắn nên khai báo có thể đặt ở đầu hàm
  + Các thuộc tính của class nên được khai báo ở đầu class để đảm bảo tính nhất quán

#### 5.2.2. Các hàm phụ thuộc
  + Nếu một hàm gọi 1 hàm khác, chúng nên ở gần nhau
  + Hàm được gọi nằm ngay dưới hàm gọi
  + Điều này giúp việc tìm kiếm hàm, đọc hiểu chương trình dễ hơn, tự nhiên hơn

#### 5.2.3. Định dạng theo chiều ngang
  + Số kí tự tối đa trên mỗi dòng nên được giới hạn, thường là 80

## 6. Sơ lược về code design
