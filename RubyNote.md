# Ruby On Rails & Ruby Note :green_book: :memo:

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

### 9. Sự khác biệt giữa scope và class method
  #### 9.1. Định nghĩa 1 `scope`
  * VD: 2 cách định nghĩa `scope`
    ```ruby
      class Post < ActiveRecord::Base
        scope :published, where status: "published"
        scope :draft, ->{where status: "draft"}
      end
    ```
  * Sự khác biệt cơ bản giữa 2 `scope` này là cách dùng
    - Biểu thức điều kiện của `published` sẽ được gọi 1 lần duy nhất khi class được gọi lần đầu tiên => Gặp lỗi khi tham số trong điều kiện là `Time`
    - Biểu thức của `draft` sẽ được gọi lại mỗi khi scope được thực thi
  #### 9.2 Scope cũng là class method
  * Bản thân `ActiveRecord` cũng đã chuyển đổi `scope` thành `class method`
  * VD:
    ```ruby
      def self.scope name, body
        singleton_class.send :define_method, name, &body
      end

      def self.published
        where status: "published"
      end
    ```
  #### 9.3 Scope gọi liên tiếp được
  * Khi người dùng có thể lọc các bài viết theo trạng thái, và sắp xếp chúng
  ```ruby
    # Sử dụng scope
    class Post < ActiveRecord::Base
      scope :by_status, -> status {where status: status}
      scope :recent, -> {order "posts,updated_at DESC"}
    end

    # Gọi: 
    Post.by_status("published").recent
    Post.by_status(params[:status]).recent

    # Sử dụng class method
    class Post < ActiveRecord::Base
      class << self
        def by_status status
          where status: status
        end

        def recent
          order "posts.updated_at DESC"
        end
      end
    end
  ```
  * Khi `status` là `nil`, `blank`, ta sẽ chuyển như sau
    ```ruby
      Post.by_status(nil).recent
      Post.by_status('').recent

      scope :by_status, -> status {where status: status if status.present?}
    ```
  * Sau đó ta thay đổi như sau:
    ```ruby
      class Post < ActiveRecord::Base
        class << self
          def by_status status
            where status: status if status.present?
          end
        end
      end

      # Kết quả
      Post.by_status("").recent
      NoMethodError: undefined method `recent' for nil:NilClass
    ```
  * Không nên trả về `nil` với `class method`
  * Scope luôn trả về 1 `ActiveRecord Relation`
  #### 9.4. Scope mở rộng được
  * Điều quan trọng khi phân trang là biết được số lượng trang chia ra cho dữ liệu, sau đó là số bản ghi mỗi trang
    - ``` Post.page(2).per(15)```
  * Ta có thể mở rộng `scope` bằng cách thêm các thành phần bên trong nó, các thành phần mở rộng này chỉ có tác dụng với `object` nếu `scope` được gọi
  * VD:
    ```ruby
      scope :page, -> num { # some limit + offset logic here for pagination } do
        def per(num)
          # more logic here
        end

        def total_pages
          # some more here
        end

        def first_page?
          # and a bit more
        end

        def last_page?
          # and so on
        end
      end

      # Các thành phần mở rộng này sẽ được gọi khi page được gọi
  * 1 cách viết khác với `class method`
    ```ruby
      def self.page(num)
        scope = # some limit + offset logic here for pagination
        scope.extend PaginationExtensions
        scope
      end

      module PaginationExtensions
        def per(num)
          # more logic here
        end

        def total_pages
          # some more here
        end

        def first_page?
          # and a bit more
        end

        def last_page?
          # and so on
        end
      end
    ```

### 10. Sự khác nhau giữa resources và resource
  ```ruby
    resources :orders
    => rake routes

         orders GET        /orders(.:format)            orders#index
                POST       /orders(.:format)            orders#create
      new_order GET        /orders/new(.:format)        orders#new
     edit_order GET        /orders/:id/edit(.:format)   orders#edit
          order GET        /orders/:id(.:format)        orders#show
                PUT        /orders/:id(.:format)        orders#update
                DELETE     /orders/:id(.:format)        orders#destroy

    resource :order
    => rake routes
          order POST       /order(.:format)            orders#create
      new_order GET        /order/new(.:format)        orders#new
     edit_order GET        /order/:id/edit(.:format)   orders#edit
                GET        /order/:id(.:format)        orders#show
                PUT        /order/:id(.:format)        orders#update
                DELETE     /order/:id(.:format)        orders#destroy
  ```

### 11. Sự khác nhau giữa find và find_by
  * find: nếu không tìm thấy thì sẽ `raise Exception` - **ActiveRecord::RecordNotFound exception**
  * find_by: chỉ ra thuộc tính cần tìm kiếm theo, nếu không tìm thấy thì trả về `nil`

### 12. Sự khác nhau giữa member route và collection route
  #### 12.1. Advanced Routing
  ##### 12.1.1. Nested Routing
  * Ta hoàn toàn có thể viết các resource lồng nhau
  * VD: (http://demosite.com/courses/1/students/3)
  ```ruby
    DemoApp::Application.routes.draw do
      resources :courses do
        resources :students
      end
    end

    URL: course_student  GET  /courses/:course_id/students/:id(.:format)  student#show
    # Để truy cập được URL này ta cần params_id cho cả course, student
  ```
  ##### 12.1.2. Member and Collection Routes
  * Ta sẽ sử dụng `member method` để thêm 1 `non-RESTful route` vào 1 `resources`
  ```ruby
    #config/routes.rb
    DemoApp::Application.routes.draw do
      resources :courses do
        member do
          get "review"
        end
      end
    end

    # Route này sẽ ánh xạ đến action courses#review tương ứng trong controller
  ```
  * Ta sẽ sử dụng `collection method` để thêm 1 `non-RESTful route` có chức năng tương đương với `action index`
  ```ruby
      # config/routes.rb
      DemoApp::Application.routes.draw do
        resources :courses do
          member do
            get "review"  # review một khóa học (yêu cầu ID)
          end
          collection do
            get "upcoming"  # Liệt kê tất cả các khóa học sắp khai giảng
          end
        end
      end
  ```
  ##### 12.1.3. Redirects và Wildcard Routes
  * Sử dụng khi ta muốn tạo ra những URL dễ nhớ, ngắn gọn nhưng vẫn đảm bảo gửi đi những thông số cần thiết
  ```ruby
    # config/routes.rb
    DemoApp::Application.routes.draw do
      get 'courses/:course_name' => redirect('/courses/%{course_name}/students'), :as => "course"
    end

    URLs ban đầu
    /courses/demo01
    /courses/demo02

    # Khi các URLs này được click vào, chúng sẽ bị redirect sang các URLs tương ứng sau:
    /courses/demo01/students
    /courses/demo02/students
  ```
  * Lúc này các URLs sẽ thân thiện hơn với người dùng - pretty URL
  #### 12.2. Advanced Layouts
  * Việc sử dụng lại `layouts` sẽ giảm thiểu đi số lượng code trong các `view layer`
  * Ta có thể chia nhỏ 1 layouts thành các khối như: `body`, `footer`, `header`, `sidebar`, ...
  * Có thể sử dụng nhiều `layouts` cho 1 page bằng cách từ `layouts` hiện tại ta render ra `layout` khác sử dụng method
  ```ruby
    render template: "your_layout.html.erb"
  ```
  * Ta cũng có thể sử dụng `content_for method` để đưa thông tin từ 1 `layout` sang 1 `layout` khác
  * VD: Ta có 1 file `static_page.html.erb`: trang này sẽ dùng cho `StaticPagesController`
    - Nó sẽ kế thừa `layouts` từ application.html.erb, nhưng ta muốn loại bỏ đi 1 vài CSS không cần thiết
    ```ruby
      # app/views/layouts/static_pages.html.erb
      <% content_for :stylesheets do %>
        #navbar {display: none}
      <% end %>
      <%= render :template => "layouts/application" %>
    ```

    - Sau đó trong `application layout` ta cần thiết lập để lọc lấy nội dung đó và sử dụng chúng

    ```ruby
      # app/views/layouts/application.html.erb
      ...
      <head>
        ...
        <style><%= yield :stylesheets %></style>
      </head>
      ...
      render :template => "static_pages.html.erb"
      ...
    ```
    - Khi `yield` đến `block content :stylesheet` nó sẽ chèn nội dung bên trong `content_for` đến nơi chứa yield method
    - Như trong ví dụ trên, ta sẽ chèn 1 vài nội dung CSS vào trong content_for, toàn bộ nội dung này sẽ nằm trong 1 `layout` là `static_page`, sau đó trong `application.html.erb` ta sẽ `render` ra `layout` - `StaticPage`, toàn bộ nội dung CSS này sẽ được đưa vào trong `application.html.erb`
    ```ruby
      # app/views/layouts/application.html.erb
      ...
      <head>
        ...
        <style> #navbar {display: none} </style>
      </head>
      ...
    ```
  #### 12.3. Metaprogramming Rails
  * Có thể hiểu như lập trình trong lập trình
  * Ta có thể thay đổi codes của chương trình khi chương trình đang chạy
  * VD: Ta có thể tạo mới, xóa các phương thức hiện có trên `object, class` để tránh lặp codes
    - Tạo ra 1 cặp helper_url: xxx_path, xxx_url
    - Sử dụng `send method` để thực thi các `method` do `Metaprogramming` sinh ra, nó sẽ gửi đi với các đối số mình muốn
      * VD: 
      ```ruby
      > 1 + 2
      ==> 3
        
      > 1.send(:+, 2)
      ==> 3
      ```
    - VD:
    ```ruby
      class Rubyist
        define_method :hello do |my_arg|
          my_arg
        end
      end

      obj = Rubyist.new
      puts(obj.hello('User')) # => User

      # method hello sẽ được tạo ra khi gọi
    ```

### 13. .nil?, .empty?, .blank?, .presents?
  #### 13.1. .nil?
  * `Object class` có `method nil?` nên mọi object trong `ruby` đều có `nil? method`
  * Chỉ có `nil.nil? = true`
    - Trong `ruby` mọi object đều có gía trị `boolean` là `true`, ngoại trừ `false` và `nil`
  #### 13.2. .empty?
  * Là hàm có sẵn của `string`, `hash`, `array`
  * Trả về `true` khi `object.length == 0`
  #### 13.3. .blank?
  * `1 object` được coi là `blank` nếu nó `false`, `empty` hoặc là 1 chuỗi chỉ gồm các khoảng trắng
  * Là 1 hàm của `Rails`
  #### 13.4. .present?
  * Là `object` không `blank`
  * Ngoài ra còn 1 `method` hay dùng đó là `.presence`
    - `method` này sẽ trả về `object` gọi nó nếu `object` đó `present` còn không trả về `nil`

### 14. Validation
  * Là các thao tác kiểm tra dữ liệu trước khi lưu object vào db
  * Các method sau sẽ gọi validate
    - `create, create!`
    - `update, update!`
    - `save, save!`
  * Validate helpers của ActiveRecord
    - `Syntax: validates :attr_name, type`
    - `presence: true` không bỏ trống
    - `absence: true` phải để trống
    - `uniqueness: true` duy nhất
    - Tính đồng nhất - So sánh tính đồng nhất giữa tên thuộc tính và #{tên thuộc tính}_confirmation
      * `confirmation: true`
    - Độ dài
      * `validates :title, length: { [minimum | maximum | is]: 1 } | { in: 1..10 }`
    - `numericality: true` kiểu số
    - `format: { with: VALID_EMAIL_REGEX }`: email
    - `inclusion: {in: [true, false]}` xét gía trị có thuộc 1 tập cho trước không
    - `attr_reader: model`: map model với @model

### 15. Phân biệt destroy, delete
  * Cả 2 phương thức này đều xóa bản ghi trong db
  * `Delete`: Không gọi `callback` mà xóa bản ghi luôn
  * `Destroy`: Gọi tới `callback` - `VD: before_destroy` rồi mới xóa bản ghi trong db

### 16. Sự khác nhau giữa new, create, save
  * `new` sẽ chỉ tạo ra 1 `object` nhưng không lưu vào `db`
  * `create` tạo ra `object` và lưu vào db 1 lần
  * `save` lưu `object` nếu `object` mới được tạo, nếu không thì cập nhật

### 17. Eager loading
  * Thay vì truy xuất nhiều lần vào db dẫn đến việc mất an toàn và giảm `performance`
    - `Eager Loading` sẽ cải thiện vấn đề trên - (N + 1 query)
    
    ```ruby
      employees = Employee.includes(:title).limit(10)
      employees.each do |employee|
        puts employee.title.name
      end
    ```

### 18. Sự khác nhau giữa render và redirect_to
  * `render` Thường dùng để `render` ra khung `html`
  * `redirect_to` sẽ điều hướng trang tới liên kết

### 19. Active Record Associations
  #### 19.1. Giới thiệu
  * Associations là sự liên kết giữa 2 model với nhau
  * Giúp code dễ hiểu, ngắn gọn
  #### 19.2. Các loại Associations
  * Rails hỗ trợ 6 loại như sau:
    + belongs_to
    + has_one
    + has_many
    + has_many:through
    + has_one:through
    + has_and_belongs_to_many
  ##### 19.2.1. belongs_to association
  * Thiết lập kết nối 1-1 (one-one) giữa model này với model khác
  * Ta cần sử dụng khóa ngoại cho bảng
  ##### 19.2.2. has_one association
  * Cũng thiết lập kết nối 1-1 (one-one)
  * Nó cho thấy 1 đối tượng của model này sở hữu 1 đối tượng của model khác
  ##### 19.2.3. has_many Association
  * has_many thiết lập kết nối one-many tới 1 model khác
  * Ta thường thấy has_many trong model kia ở mối quan hệ belongs_to
  * 1 đối tượng của model này sẽ có 0 hoặc nhiều đối tượng của model khác
  ##### 19.2.4. has_many :through Association
  * Thường dùng để thiết lập mối quan hệ many-many (n-n) giữa 2 model với nhau
  * Thông thường đối với quan hệ (n-n) ta sẽ tạo ra 1 model thứ 3 ở giữa với 2 quan hệ (1-n)
  ##### 19.2.5. has_one :through Association
  * Thiết lập mối quan hệ one-one (1-1) giữa 2 model với nhau
  * Mối quan hệ 1-1 này là thông qua 1 model thứ 3
  ##### 19.2.6. has_and_belongs_to_many Association
  * Tạo ra 1 quan hệ n-n trực tiếp mà không thông qua 1 model khác
