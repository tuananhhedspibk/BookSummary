# Ruby On Rails Tutorials :rocket: :green_book: :memo:

## Chapter 3: Mostly static pages
  * Static Page Controller không sử dụng REST actions

  ```bash
    rails destroy controller(model) controller_name(model_name) # hủy controller(model)
    rails db:migrate: migrate db
    rails db:rollback: # undo 1 bước migration
    rails db:migrate VERSION=0: # quay về ban đầu
  ```

  * Khi truy cập 1 URL thì Rails sẽ tìm đến controller -> action -> gen ra view tương ứng với action

  ```html.erb
    provide() # function in view - thiết lập title cho các page khác nhau
    <%= yield %>: # khi visit 1 page, convert nội dung page đó sang code HTML rồi insert vào layout
  ```

## Chapter 5: Filling in the layout
  * Tên của `controller` luôn là số nhiều, `model` là số ít

## Chapter 6: Modeling users
  * `ActiveRecord` là thư viện mặc định của Rails cho việc tương tác với database
  * `ActiveRecord` chứa 1 tập các `method: creating, saving, finding data objects` mà không cần dùng `SQL`
  * `Rails` còn có tính năng `migration`: cho phép việc định nghĩa dữ liệu được viết bằng `Ruby`
  * Sau khi generate model, sẽ có file migration: cung cấp phương thức để thay đổi cấu trúc của database
  * Tên của `migration_file` được `prefixed` bởi `timestamp là thời điểm mà migration_file được gen`
  * `change method` trong `migration_file` đ/n sự thay đổi cho db
  * `create_table method`: tạo bảng -> `create_table block`
  * 1 model sẽ biểu diễn 1 bản ghi trong table, tên table là số nhiều
  * timestamps: tạo ra 1 cột: created_at, updated_at, được tự động ghi khi create, update bản ghi
  * cột id sẽ được tạo, tăng tự động

  ```shell
    rails db:migrate: # chạy file migrate
    rails console --sandbox: # chạy rails console mà không thay đổi db
  ```

  ```ruby
    Model.new: # trả về obj với nill attributes, chỉ tạo ra 1 model_object và lưu ở bộ nhớ chứ chưa lưu vào db
    object.save: # lưu vào db, trả về true nếu thành công, false nếu ngược lại
    Model.create([param]): # tạo và lưu vào db, trả về model_object 
    model_object.destroy: # hủy object, nhưng object vẫn còn ở bộ nhớ
    Model.find(): # nếu không thấy thì sẽ gây ra exception
    Model.find_by(attr: value): # tìm theo 1 attr nhất định, nếu không thấy sẽ trả về nil
    Model.first, Model.all
  ```

  * Sau khi `update attribute` cần phải `save object`, vì nếu không save thì có thể sẽ `object.reload` lại được
  * `object.update_attributes(attr: value, attr: value, ...)` - param của update_attributes có thể là 1 hash, nếu thành công sẽ trả về true và tự động save
  * `object.update_attribute(attr: value)`: sẽ chỉ update 1 attribute mà thôi
  * Cách đê validate 1 attr là sử dụng validates method - validates(:atrr, hash), nếu là presence ==> presence: true
  * `object.inspect`: trả về object gọi đến method inspect
  * Với `format validation`: sử dụng format: {}
  * `add_index(:table, :column, option) method`: add thêm index cho column của table, bản thân index không bắt buộc unique
  * Việc chạy rails console có thể lock db, ngăn migration
  * Adding secure password: trong db lưu trữ password đã băm, để authenticated user ta sẽ băm password được submit bởi user, rồi so sánh giá trị này với giá trị băm được lưu trong db -> kể cả khi db bị lộ thì password vẫn an toàn
  * `has_secure_password`: được dùng cho secure password, has_secure_password sẽ:
    - Lưu trữ `securely hashed password_digest attribute` vào db
    - Cặp thuộc tính ảo: `passwor`d và `password_confirmation`
    - `model_object.authenticate(password) method`: trả về object nếu password đúng, và false khi ngược lại
    - has_secure_password chỉ hoạt động khi có thuộc tính password_digest (hashed password và password digest là giống nhau)
  * Tên của các migration file add thêm cột nên có dạng: add_column_name_to_table_name
  * `add_index(:table, :column, :type)` method: add thêm `column` cho `table` với kiểu `type`

  ``` rails
    gem bycript # để băm password
  ```

  * `!!object`: convert object sang boolean value tương ứng

## Chapter 7: Sign up
  * `<%= debug(params) %>`: in ra thông tin debug
  * `Rails.env`: hiển thị thông tin về môi trường trong Rails
  * `rails db:migrate RAILS_ENV=production | test | development[default]`
  > Môi trường của heroku là production
  * `resources`: sẽ tự thêm `7 phương thức chuẩn của RESTful cho resource`
  * `resource` trong REST được tham chiếu theo `resource's name, resource's id`
  * `debugger` sẽ giúp debug trực tiếp
  * `form_for method`: tham số đầu vào là 1 Active Record object, sử dụng các object's attribute để build form, khi đó ta sẽ tạo ra 1 object trong new method của controllers

    ```html.erb
      <%= form_for @variable do |f| %> 
        <%= f.label %> -> sẽ thiết lập attribute cho @variable
      <% end %>
      <%= f.text_field :name %> --> 
      <input id="user_name" name="user[name]" type="text" />
    ```

    - Các giá trị của attribute name trong thẻ `<input>` cho phép Rails tạo ra 1 hash thông qua params variable
    - Rails tạo ra thẻ `<form>` dựa vào `@variable`, form này sẽ có `method là post`
  * `MD5 hasing algorithm` được triển khai sử dụng hexdigest method là 1 phần của Digest library
    - Digest::MD5::hexdigest(string)

  ```html.erb
    <input name="utf8" type="hidden" value="&#x2713;" />
    <input name="authenticity_token" type="hidden" value="NNb6*J/j46LcrgYUC60wQ2titMuJQ5lLqyAbnbAUkdo=" />
      <!-- Không xuất hiện ở browsers, nhưng được sử dụng bên trong Rails
      - &#x2713; : ✓ -> ép browsers submit dữ liệu sử dụng kiểu encoding đúng
      - Ngoài ra thì authenticity_token: được sử dụng để chống CSRF -->
  ```

  * `params hash` bao gồm các thông tin về mỗi request
  * `strong_parameter`: là kĩ thuật chỉ tiếp nhận 1 vài các dữ liệu thực sự cần thiết khi submit dữ liệu lên server để tránh mass assignment(việc submit các dữ liệu nhạy cảm với mục đích xấu)
  * `params.require(:symbol)` -> symbol: là 1 trường trong params
  * Khi submit failure sẽ gen ra 1 list các errors liên kết với @object
  * `pluralize(integer, string)` -> nối string với integer biến string thành dạng số nhiều.
  * Với create action sẽ không có view template nào tương ứng với nó
  * `Rails` cung cấp 1 `method: flash()` ~ hash để hiển thị temporary message

  ```ruby
    flash[:success] = "Mess" # thông báo thành công
  ```

  * Trong `Embedded Ruby 1 symbol` sẽ tự động chuyển qua string
  * content_tag(name, content, options): trả về 1 block HTML tag
  * Mặc định: `Heroku` sử dụng `WEBrick - webserver thuần Ruby`, dễ set up và run, tuy nhiên không phù hợp khi có nhiều request đến
    - WEBrick mặc định là đơn thread, đơn process - có nghĩa là khi có 2 requests đến thì cái thứ 2 sẽ phải chờ
  * `Puma: HTTP server` - có khả năng xử lí lượng lớn các request
  * @variable trong create lưu dữ liệu khi submit sai
  * `PUT: replace & create`, `PATCH: modified`

## Chapter 8: Basic Login
  * `HTTP` là giao thức phi trạng thái, nó sẽ coi 1 request như 1 transaction độc lập, có nghĩa là sẽ không sử dụng các thông tin từ các requests trước
  * Sử dụng `session` để login - session là một dạng connection bán   vĩnh cửu giữa 2 máy tính
  * Để dùng `session` trong Rails ta sẽ dùng cookies
  * Sử dụng `session method` của Rails để tạo ra 1 session tạm thời và sẽ hết hạn tự động khi đóng trình duyệt, mọi dữ liệu trong đó sẽ biến mất khi đóng trình duyệt
  * Sẽ mô hình hóa session như là 1 `RESTful resource`
    - Khi visit 1 login page sẽ render ra 1 form cho `new session`
    - `Loggin` sẽ `create session`
    - `Logout` sẽ `destroy session`
  * `Session resource` sẽ sử dụng `cookies`. Các công việc khi login đều được build từ 1 hệ thống `authentication cookie-based`
  * Thông thường thì `create, destroy actions` sẽ không map đến views nào cả
  * Các `error-messages` chỉ được dùng với Active Record, session không phải là `Active Record`
  * Khi sử dụng session mọi thông tin cần lấy từ: `params[:session]`. 

  ```ruby
    params[:session][:email]
  ```

  * Việc `re-rendering` 1 page không tính là 1 request nên sẽ không load lại trang
  * Nội dung của `flash.now` sẽ biến mất ngay khi có 1 request mới thêm vào
  * Các helpers sẽ có mặt trong các views khi chúng ta include các helper modules vào base class của mọi controller `Application Controller`
  * `session method` sẽ tạo ra 1 cookie tạm thời ở browser
  * `temporary cookie` được tạo ra bởi `session` sẽ tự động được mã hóa
  * `redirect_to` variable sẽ tự convert sang route như sau: ```ruby redirect_to user_url(user)```
  * `log_out` về bản chất là xóa đi id trong session

## Chapter 9: Advanced login
  * Các thông tin lưu trữ trong `cookie` sẽ không được mã hóa, nên không có tính bảo mật
  * Có `4` cách để đánh cắp cookie:
    - `packet sniffer`: phát hiện cookie được truyền trong các network bảo mật kém -> dùng `SSL` để chống
    - Truy nhập trái phép vào db lưu trữ `remember token`  -> lưu trữ `hash digest` của `remember token` ~ lưu `password digest`
    - `XSS` -> Rails sẽ tự động phòng chống bằng việc từ chối mọi việc insert các content vào view template
    - Truy nhập vật lí vào các máy đã loggin
  * Chiến lược sử dụng `persistent sessions`:
    - Tạo ra `1 xâu số ngẫu nhiên` và dùng nó làm `remember token`
    - Đặt `token` ở `cookie` trong browser với ngày hết hạn xa trong tương lai
    - Lưu `hash digest` của token vào db
    - Đặt phiên bản đã mã hóa của id trong browser cookie
    - Tìm dữ liệu trong db dựa vào id đó và xác nhận remember token trong `cookie` match với `hash digest` của `remember token` trong db

  ```bash
    rails g migration add_column_to_models column:data_type
  ```

  * `urlsafe_base64 method` của `SecureRandom module` trong Ruby sẽ trả về 1 string độ dài 22 gồm các kí tự: "A-Z", "a-z", 0-9, "-", "_"
  , khá an toàn trong URL
  * Remember user sẽ tạo 1 remember token và băm nó rồi lưu vào db, remember_token sẽ được tạo ra thông qua 1 method tự viết, và được lưu vào cookies
  * `attr_accessor` có thể tạo `accessible attribute`
  * Ruby xử lí assignment trong object mà ko có self thì sẽ tạo ra 1 local variable

    ```ruby
      def func
        email = "123"     # tạo ra local variable là email
        self.email = "123"    # sẽ gán attribute email của object
      end
    ```

  * `cookies method` có thể xử lí như 1 hash, 1 element của cookie có 2 phần: value, expire date(option)
  * `permanent method`, sẽ thiết lập ngày hết hạn của cookie là 20 năm sau (tự động)
  * Sử dụng `signed cookie` để mã hóa bảo mật cookie trước khi đặt vào browser vì nếu chỉ dùng `cookie[:attribute]` dễ bị ăn cắp thông tin

  ```ruby 
    cookies.signed[:attribute] # sẽ tự động giải mã attribute
  ```

## Chapter 10: Updating, showing, and deleting users
  * Sử dụng `edit action` để render ra `edit template view`
  * Do browser không thực hiện `PATCH method` nên `PATCH form` trong rails sẽ fake `POST request` với 1 thẻ như sau:

    ```html
      <input name="_method" type="hidden" value="patch" />
    ```

  * Để phân biệt form `POST` vs `PATCH` Rails sử dụng `Active Record’s new_record? boolean method`

    ```ruby
      - User.new.new_record?
      => true   -> POST form
      >> User.first.new_record?
      => false  -> PATCH form
    ```

  * Với `has_secure_password`: `nil password` sẽ gây ra lỗi
  * `authorization` giúp điều khiển những gì mà người dùng có thể làm
  * `authentication` giúp định danh người dùng
  * `before filter` sử dụng `before_action` để triển khai 1 method đặc biệt được gọi trước 1 số actions nhất định
  * `before filter` mặc định sẽ được áp dụng mọi `action trong controller`

    ```ruby
      before_filter only: [:action]
      # chỉ áp dụng before_filter cho method "action"
    ```

  * Trong mọi Controller có 2 `accessor methods` trỏ đến `request` và `response object` gắn với 1 `request cycle` đang được thực thi
    - `request object`:
      * Chứa mọi thông tin về `request` từ `client`
      * Có `3 accessors`: `path_parameters`, `query_parameters`, và `request_parameters`
        * `query_parameters hash`: gồm các `parameters` được gửi như là 1 phần của `query string`
        * `request_parameters hash`: gồm các `parameters` được gửi như là 1 phần của `post body`
    - `response object`:
      * Không thường xuyên được sử dụng trực tiếp, nó được tạo nên trong quá trình `action` và `render data` được gửi về cho client
  * `redirect` sẽ không xảy ra cho đến khi `hết method` hoặc `gặp return` nên mọi code xuất hiện sau `redirect` vẫn có thể được thực thi
  * Giá trị mặc định của các thuộc tính trong table là `nil`

  ```ruby
    object.toggle!(:attribute)
    # chuyển attribute thành true <-> false
  ```

  ```html.erb
    <%= link_to method: :delete %>
    <!-- thực thi DELETE request -->
  ```

  * Web browser không thể gửi `DELETE request` 1 cách tự nhiên, Rails sẽ `fake` nó bằng `JS` -> `delete link` chỉ hoạt động khi bật JS cho trình duyệt
  * Cũng có thể fake `DELETE request` bằng việc sử dụng `1 form` và `POST request` - chỉ hoạt động khi không có JS
  * Trong file view, ta sẽ sử dụng `paginate` bằng: `will_paginate method`, nó sẽ tự động tìm `@users object`, và sau đấy sẽ hiển thị các `pagination link` sang các page khác. Để giúp cho `will_paginate method` hoạt động ta sẽ cần sử dụng `paginate method. paginate` sẽ có `1 hash argument` có `key` là `:page` có `value` bằng với `page được requested`. `paginate method` sẽ trích rút từ db ra 1 cụm dữ liệu (vd: 30 bản ghi) ở 1 thời điểm dựa trên `:page parameter`, nếu `page = nil` thì `paginate method` sẽ trả về page đầu tiên. `params[:page]` được gen 1 cách tự động từ `will_paginate`, có thể `config` số bản ghi mỗi trang bằng: `per_page: 6`

### Chapter 11: Account activation
  * Sử dụng activation token và digest đối với user
  * Gửi cho user 1 email với token và active link
  * Ý tưởng cơ bản
    - user sẽ được khởi tạo với trạng thái `unactive`
    - Khi user sign up sẽ có 1 activation token được gen ra cùng với activation digest tương ứng
    - Lưu activation digest trong db, gửi cho user 1 email cho user bao gồm activation token và user's email
    - Sau khi người dùng click vào link thì sẽ tìm trong db theo địa chỉ email và so khớp activation token và digest
    - Nếu user đã được authenticated (xác thực) thì sẽ chuyển trạng thái từ "unactivated" -> "activated"
  * Ta sẽ mô hình hóa "account activation" như là 1 resource mặc dùng nó không liên kết với Active Record model
    - Ta sẽ tương tác với chúng thông qua REST URL
    - `Activation link` sẽ modified user's activation status bằng cách thực hiện 1 PATCH request - gọi UPDATE method
  * Các activation token phải khác nhau đôi một
    - Nếu ta lưu trữ token dưới dạng 1 string trong db và activation URL -> ảnh hưởng đến bảo mật
  * Ta nên add thêm activation_token và activation_digest cho mỗi user object trước khi nó được save vào db
    - Ta sẽ sử dụng [before_save callback]: tự động được gọi trc khi object được lưu vào db - bao gồm cả trường hợp created và updated
    - Với activation digest ta chỉ cần created -> before_create
      * before_create :create_activation_digest => method reference => thường dùng hơn là cách truyền block
    - Để chuyển từ token -> digest: sử dụng gem BCrypt
  * Với remember user ta sẽ sử dụng "update_attribute" do người dùng cần remember khi user object đã có trong db còn ở đây là khi user còn chưa được lưu vào db - vời mới tạo, ta đã tạo ra activation_token, activation_digest
    - Khi user được định nghĩa bởi User.new thì nó sẽ tự động activation_token và activation_digest 
    - Sau đó 2 thuộc tính này sẽ được tham chiếu tới column trong db, ngay khi user được lưu vào db chúng sẽ được ghi vào db
    - Với các thuộc tính ảo ta sẽ sử dụng :attr_accessor
  * Để gửi mail ta cần 1 mailer từ Action Mailer library
    - Mailers về cơ bản được cấu trúc giống như controller actions
    - Email templates được định nghĩa như là các views
  * Gen mailer: rails g mailer Name method_name
    - Câu lệnh này ngoài việc gen ra các method có tên như trên mà còn gen ra các templates tương ứng với nó
      * Với mỗi mailer sẽ có 2 templates: plain_text mail, HTML mail
    - Trong mailer: 
      * appllication_mailer: default from: địa chỉ gửi chung & mặc định cho app
      * Với các mailer cụ thể đều có địa chỉ nhận
    - Trong view template: có biến instance @greeting cũng tương tự như với "view-controller"
  * mail to: email_add, subject: "string"
  + Cách authenticated?
    - Tìm ra user thông qua user mail
    - Sau đó dùng activation_token để kích hoạt
    - link trong email: mail + activation_token
  + Do ta mô hình hóa activation sử dụng Account Activation resource nên bản thân token có thể sử dụng như argument trong named routed
    - edit_account_activation_url(@user.activation_token, ...)
    - Khi đó với: edit_user_url(user) -> URL: /users/1/edit
      => account activation link:   -> URL: /account_activations/q5lt38hQDc_959PVoo6b7A/edit
        * q5lt38hQDc_959PVoo6b7A: URL-safe base64 string được gen bởi new_token method <-> tương tự như user_id
        * Trong Activation controller edit method: token sẽ có mặt trong params
        * Để include cả email -> sử dụng query parameter: key=value đằng sau ? của URL
          + account_activations/q5lt38hQDc_959PVoo6b7A/edit?email=foo%40example.com (%40 = @ - mã hóa cho @ để có được URL hợp lệ)
        * Trong Rails để có thể thiết lập "query parameter" include hash trong named route
          + edit_account_activation_url(@user.activation_token, email: @user.email)
            - Khi đó Rails sẽ tự động mã hóa các kí tự đặc biệt
            - Trong controller thì email sẽ ko được mã hóa, và có trong params[:email]
  + Có thể sử dụng email preview để xem kết quả của email
  + Metaprogramming
    - Bản chất là sử dụng 1 program để viết ra 1 program
    - Ta có thể sử dụng send method để gọi 1 method khác, thông qua việc truyền vào 1 symbol, 1 string cho method send đóng

## Chapter 13: User microposts
  * Sử dụng `model:reference` -> tạo ra 1 cột là khóa ngoại, có index mặc định ở đó
  * `build method` tạo ra 1 object và lưu nó vào bộ nhớ chứ không lưu vào db
  * `new method` tạo ra 1 object và lưu nó vào bộ nhớ và lưu vào db
  * `create method` tạo ra 1 object
  * `resource :model only[:action]`: chỉ tạo ra các action được liệt kê trong mảng only cho controller
  * Để truyền object vào partial ta sẽ sử dụng 1 hash, value = object, key sẽ trùng tên với biến trong partial
  * time_ago_in_words helper method
  * Khi ở trong User Controller `<%= will_paginate %>` sẽ không cần biến, còn nếu là paginate cho micropost thì sẽ cần instance variable @micropost 
  * `count method` sẽ không pull toàn bộ dữ liệu ở db ra rồi đếm, mà nó sẽ yêu cầu đếm trực tiếp trên db(thao tác này đã được tối ưu hóa cao)
  * `size method` đếm 1 lần rồi lưu `cache` nên các lần sau sẽ chỉ lấy từ `cache` ra
  * `Micropost.where("user_id = ?", id)` "?" trong query trên đảm bảo rằng `id` được che dấu đi trước khi được include vào `SQL query` qua đó tránh được `SQL injection`
  * `CarrierWave` thêm `Rails generator` cho việc tạo 1 `image uploader`

  ```bash
    rails generate uploader name
  ```

  * Để nói cho `CarrierWave` kết nối `1 image với model` là sử dụng `mount_uploader method`
    - `mount_uploader method` có 2 arguments: - `symbol`: biểu diễn `attribute` và `class name` của uploader
  * html: { multipart: true } trong arguments của `form_for`: dùng để upload file
  * `request.referrer method` trả về `URL` của trang trước
  * `image_tag helper` được dùng để render ra ảnh sao khi upload
  * `picture? boolean method` được dùng để ngăn việc hiển thị 1 image tag khi nó không phải là ảnh - được tạo ra bởi `CarrierWave`
  * Sử dụng `validate :custom_method` cho các `custom validation`, nó sẽ gọi method tương ứng với `symbol :custom_method`
  * Trong `file_field` `input tag, accept: "type"`
  * Khi `deploy production` trong `Heroku` thi `ImageMagick` đã được installed từ trước
  * Để dùng cloud lưu trữ, ta sẽ phải sủ dụng `gem fog`
  * Trong file `uploader: storage :file` -> sử dụng `local file system` cho việc lưu trữ `images` - không nên dùng trong `môi trường production`

  ```html.erb
    <%= link_to image_tag("url") %> # ảnh vs link
  ```

  * `validates :user` thì nó kiểm tra mọi trường có trong user model, nếu user tồn tại thì nó mới cho pass còn `validates: user_id` thì nó chỉ kiểm tra user_id có khác `null` không, kể cả `user_id`này không có trong bảng Users thì nó vẫn cho pass

## Chapter 14: Following users
  * `A following B` --> `A: follower`, `B: followed(following)`
  * Tập hợp mọi user1 -> n follow 1 userA thì tập đó gọi là `userA.follower`
  * Theo như kiến trúc REST: khi 1 user follow 1 user khác thì sẽ có 1 cái gì đó được tạo ra và hủy đi nếu unfollow
  * following: mình theo dõi, followers: người theo dõi mình
  * A `following` B, B `ko following` A: A có `mối quan hệ active` với B, B có `mối quan hệ passive` với A
  * active_relationships = following
  * 1 user `has_many relationships`, 1 relationships `belongs_to follower, followed user`
  * source parameter
  * association có các method có thể xử lí như 1 mảng, sẽ thực hiện các thao tác trực tiếp trên db

  ```ruby
    resources :users do
      member do
        get :following, :followers  -> GET request, chỉ render data
      end
    end
  ```

    URLs for following and followers: /users/1/following and /users/1/followers
  * 1 cách tự nhiên, form sẽ gửi 1 POST request sang create method trong controller
  * Khi template cho 2 action tương tự nhau thì chỉ nên tạo ra 1 template rồi custom
  * `AJAX` in Rails
    - AJAX: khi browser gửi request lên server khi đó sau khi dữ liệu được trả về, browser sẽ phải render hoặc redirect sang trang khác -> cần phải load lại trang -> có thể load các dữ liệu không cần thiết cũng như tải về các dữ liệu không cần thiết trong khi chỉ cần lấy về 1 lượng dữ liệu nhỏ -> tốn băng thông, thời gian, không an toàn. Dùng AJAX sẽ giúp cho chúng ta có thể gửi request lên server bằng JS mà ko cần browser, dữ liệu sẽ được cập nhật thông qua JS, cải thiện trải nghiệm người dùng, không tốn tài nguyên
    - Được triển khai theo những bước sau:
      * `Some trigger fires`: có thể là button click, link click khiến data thay đổi
      * `The web client calls the server`: là 1 JS method, XMLHttpRequest, gửi các dữ liệu liên qua tới trigger phía trên tới action handler ở server
      * `The server does processing`: action handler phía server(Rails controller action), thao tác với dữ liệu và trả về dữ liệu cho client
      * `The client receives the response`: JS bên phía client, sẽ nhận HTML trả về và update lên phía client
    - `Introduction about AJAX`:
      * Khi gửi request lên server, server xử lí, gửi cho client response, client sẽ phân tích response, và fetch toàn bộ JS, image, css và nhúng vào page -> 'request response cycle'
      * AJAX là kĩ thuật sử dụng JS để tạo request lên server, parse response và update information cho page
    - `Built-in Helper`:
      * form_for:

        ```html.erb
          <%= form_for(remote: true) %> <!-- form sẽ được submit bằng ajax -->
        ```

        * `bind ajax:success, ajax:error event`: để xử lí khi submit thành công hoặc không
      * link_to:

        ```html.erb
          <%= link_to remote: true %>
        ```

      * button_to:

        ```html.erb
          <%= button_to remote: true %>
        ```

    - Server-Side Concerns:
      * AJAX không chỉ ở phía client, mà còn ở phía server, thông thường AJAX request sẽ trả về JSON hơn là HTML

    ```html.erb
      <form data-remote="true"> <!-- cho phép form được xử lí bởi JS -->
    ```

  * Bằng cách sử dụng `respond_to` trong controller để respond lại AJAX request, respond_to method sẽ đáp ứng 1 cách thích hợp tùy vào loại request
    - pattern phổ biến:

    ```ruby
      respond_to do |format|
        format.html { redirect_to user }
        format.js
      end
    ```

  * Với Ajax request, rails sẽ tự động gọi `JS Embedded Ruby(.js.erb)` file có cùng tên với action. Các file này là các file mix JS, embedded Ruby sẽ giúp chúng ta update giao diện bằng JS mà ko phải render lại trang. Trong các file JS-ERB file này đã cung cấp `jQuery` tự động.
  * `following_ids method` được tạo ra bởi `Active Record` dựa trên `has_many :following association` -> `association_name_ids` -> sẽ kéo toàn bộ id vào bộ nhớ và tạo ra 1 mảng cho các ids đó -> để giải quyết: sử dụng subselect

  ```ruby
    collection.each_with_index do |ele, index|
    
    end
  ```

  * Nếu submit sai, thì vẫn ở trong `create action`, có thể url có dạng : `/posts` nhưng đây là `HTTP POST method`, khác với `/posts index` là `HTTP GET method`
  * Với `default_url` trong rails `carrierwave` nếu muốn dùng thì phải gọi trực tiếp
  * Trong Rails thường thì khóa ngoại trong 1 table sẽ có dạng: `<class_name>_id (class: lower_case)`
  * user sẽ có nhiều following "thông qua" relationships
  * Trong rails ta hoàn toàn có thể override các default
  * `has_many :through` tìm khóa ngoại tương ứng với "singular version" của association
  * Sử dụng source param ta có thể custom cho tên của `has_many ...`
  * Trong trường hợp của `:followers attribute` thì Rails sẽ `singularize followers` và tự động tìm kiếm khóa ngoại `follower_id`
