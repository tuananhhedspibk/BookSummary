# Ruby Note :green_book: :memo:

### 1. Block
  * Mọi thứ nằm giữa {} hoặc `do` và `end` là 1 `Block`
  * VD:
    ```ruby
      [1, 2, 3].each do |x|
        puts x * x
      end
    ```
  * 1 `block` truyền vào 1 `method` sẽ được coi như là 1 `Proc - có thể hiểu như 1 con trỏ tới block`
  * `Block` không phải là 1 `object`
  * Khi ```ruby function() do block end```
    + Thì khi đó block sẽ được ngầm định truyền vào trong function()
    + Khi `block` truyền ngầm định - không qua tham số vào `function`, ta sẽ gọi thực thi `block` bằng lời gọi `yield`
    + Khi `block` được truyền tường minh vào trong hàm, block sẽ được gọi thông qua lời gọi `call`
      - ```ruby block.call [param]```
### 2. Procs
  * Là 1 `object` biểu diễn 1 `Block`
  * Syntax: ```ruby procs_name = Proc.new block```
  * Gọi thực thi 1 `Procs`: ```ruby procs.call [param]```

### 3. Lambda
  * Là 1 dạng `Anonymous function`
  * Vận hành giống như `global function`
  * VD:
    ```ruby
      succ = lambda {|x| x + 1}
      succ = -> (x) {x + 1}
      # succ: có thể coi như con trỏ hàm

      f = -> (x, y ; z, t)
      # x,y: tham số vào, z, t: biến cục bộ của hàm
  * ```ruby Lambda.class = Proc```

### 4. Chú ý giữa Block, Procs, Lambda
  * Chỉ truyền được nhiều nhất là 1 tham số `block` vào `method`
  * Có thể truyền nhiều `Procs` vào `method`
  * `Lambda`, `Procs` đều là `Procs object`
  * `Lambda` kiểm tra số lượng tham số truyền vào, còn `Procs` thì không
    - Nếu `params` truyền vào `lambda` không đủ thì sẽ báo lỗi `argument method`
    - Nếu `params` truyền vào `procs` không đủ thì mặc định các cái thiếu là `null`
  * `return` trong `Lambda` thì đoạn `code` bên ngoài `Lambda` vẫn được thực thi 1 cách trọn vẹn (gần như độc lập với `Lambda`)
    - Chạy hết `Block` cha rồi mới ngắt
  * `return` trong `Procs` sẽ khiến cho đoạn `code` bên ngoài `Procs` không được thực thi
  * `Lambda` có thể được sử dụng ở bên ngoài phạm vi mà nó được định nghĩa

### 5. Sự khác biệt giữa count, length, size
  * `count` sẽ đếm bằng lệnh `select count` trong SQL
  * `length` sẽ kéo dữ liệu về 1 mảng, rồi bóc ra độ dài của mảng đó
  * `size` lúc đầu hoạt động như `count` nhưng khi đã có `cache` thì `size` sẽ lấy gía trị từ `cache` ra

### 6. So sánh giữa update_attribute và update_attributes
  * `update_attribute` chỉ update 1 trường đơn
    - Không có `validate`
    - Gọi `callback`, chỉ `update` dữ liệu khi có sự thay đổi
    - Chỉ update khi dữ liệu thay đổi
    - Nếu trường chỉ đọc => báo lỗi `ActiveRecord::ActiveRecordError`
  * `update_attributes` có thể update nhiều trường
    - Có validates dữ liệu
    - Nếu đối tượng không hợp lệ thì `return false`

### 7. Cách form_for phân biệt edit, new
  * Khi 1 biến instance truyền vào `form_for`
  * Thông qua gía trị trả về của `@variable.persisted?`
    - `true`: `edit form`
    - `false`: `new form`

### 8. Sự khác biệt của hidden_field, hidden_field tag
  * `hidden_field :email => :email` sẽ được truyền vào `params[:email]`
  * `hidden_field_tag :email => :email` sẽ được truyền vào `params[:user][:email]`