# React note :green_book: :memo:

## 0. React Javascript
### 0.1. Toán tử ...
  * Dùng để copy `object`, `array`
  * `obj2 = {...obj, [att: value]}`
    - `att`: có thể là att mới, hoặc có thể là att hiện có - ghi đè
  * `var arrCP = [[value, value, ...], ...arr, [value, value, ...]]`
### 0.2. Pure function
  * Là các function không có `side effect`
  * `Side effect` bao gồm:
    - Các tác dụng làm thay đổi `biến bên ngoài`, `tham số đầu vào`
    - Chứa `async task`
  * Nó còn là các function `one input - one output`: với cùng 1 đầu vào chỉ cho ra duy nhất 1 đầu ra

## 1. React, Route
  ```javascript
   import {Router, Route} from 'react-router';
   const Routes = (props) => (
      <Router {...props}>
        <Route path='/link/' component={React_Component_Name}>
        <Route path='/link/:model_id' component={React_Component_Name}>
      </Router>
    }
    export default Routes;
  ```
  * Để lấy về id ta sẽ sử dụng `this.props.params.model_id`

## 2. Axios:
  ```javascript
    import axios from 'axios';
    import {browserHistory} from 'react-router';
    <Routes history={browserHistory}/>
  ```
  * Dùng để tạo request từ client lên server
  * Bản chất là 1 `Promise` dựa trên `HTTP client`
  * Thực thi 1 `GET` request
    ```javascript
      axios.get('/url', [headers])
      .then(response => {
        response: dữ liệu trả về là 1 Object json - data, header, ...
      })
      .catch(error => {
        // Xử lí lỗi
      })
    ```

  * Thực thi 1 "POST" request
    - window.location: lấy về current page URL & redirect browser sang page mới luôn
      * Gồm các thuộc tính: 
        + window.location: .href, .hostname, ...
    - FormData([form]) constructor
      * formData có các entries dạng key/ value
        + pair: pair[0] -> key, pair[1] -> value
      * Tạo ra 1 formData object
      * formData.append(key, value)
      * form sẽ là 1 HTML <form> element
        + formData object sẽ bao gồm các key tương ứng với "name property" của các elements, value sẽ là value của các element đó (HTML element)
    - axios.post('/url', formData, [headers])
      .then

  * Thực thi các request khác như: PUT, DELETE: axios.put(url, [data, [conf]]) + axios.delete(url, [config]), ...

## 3. Local Storage
  * Sử dụng:
    - Lưu dữ liệu local trong browser
    - Trước HTML5, data thường lưu trong cookies (gồm cả server request)
    - Dữ liệu lưu trong localStorage được bảo mật hơn cookies, dung lượng lớn hơn, không ảnh hưởng đến hiệu năng
    - Dữ liệu lưu trong localStorage không được chuyển đến server
    - localStorage object lưu trữ dữ liệu mà không bị hết hạn
      * Dữ liệu sẽ không bao giờ bị xóa khi đóng browser
      * Các item con của localStorage có dạng key/value
      * localStorage = {key: value, key: value}
  * export const: các module khác có thể dùng hằng số này
  * JSON.parse: bóc tách dữ liệu gửi đến/ từ server => chuyển qua JSON
    - Dữ liệu nhận về từ server luôn là string

## 4. I18n
  * Sử dụng `counterpart`
  * let translate = require('counterpart');
    - translate.registerTranslations('en', require('./locales/en'));
    - Lưu "locale" trong localStorage
      * localStorage.setItem('locale', 'en');
        translate.setLocale('en'); - translate.setLocale(localStorage.locale); (nếu locale != NULL)
  * Các file ngôn ngữ: lan.js lưu trong locales/

## 5. State
  * this.checkNullInfo(response.data.restaurant) => Nên viết 1 hàm check null (dữ liệu) trước khi setState
  * Sử dụng vòng lặp trong React - nên dùng
    ```javascript
    this.state.vars.map((var, index) => {
      return (<Tag/>)
    })
    ```

## 6. ModalDialog
  ```javascript
    import {ModalContainer, ModalDialog} from 'react-modal-dialog';
      handleClick() {
        this.setState({isShowingModal: true});
      }

      handleClose() {
        this.setState({isShowingModal: false});
      }
      
        {
          this.state.isShowingModal &&
            <ModalContainer onClose={this.handleClose.bind(this)}>
              <ModalDialog onClose={this.handleClose.bind(this)}>
                // content
              </ModalDialog>
            </ModalContainer>
        }
    // isShowingModal lưu trong state
  ```

## 7. Alert
  ```javascript
  import AlertContainer from 'react-alert';
    alert_option = {
      offset: 14,
      position: 'top right',
      theme: 'dark',
      time: 5000,
      transition: 'scale'
    }
    <AlertContainer ref={a => this.msg = a} {...constant.ALERT_OPTIONS} />
    showAlert = () => {
      this.msg.show('Some text or component', {
        time: 2000,
        type: 'success',
        icon: <img src="path/to/some/img/32x32.png" />
      })
    }
  ```

## 8. Component và elements
  + `component` được tạo ra thông qua việc định nghĩa `class`
  + Khi chạy ứng dụng, sẽ có các `instances` của `component` chạy trên `UI View`
    - Mỗi `instace` có các thuộc tính cũng như `states` riêng
    - Ở các mô hình truyền thống các `component instances` sẽ phải giữ tham chiếu của nó đến `DOM nodes` tương ứng, và tới các `instances` của `child component`
      * `parent` phải kết nối trực tiếp tới các `children component instances`
  + Trong `React`: `element` bản chất là `component instance` hoặc `DOM node`, `js object`
  + `element` không hẳn là `instance`, nó chỉ là cách ta nói với `React` những gì ta muốn nhìn trên màn hình, cho nên ta không thể gọi bất cứ `methods` nào trên `element` được
    - Nó là `immutable description object` với 2 `fields`:
      * `type: (string | ReactClass)`
      * `props: Object`

### 8.1. DOM elements
  + Khi `type` là 1 `string` ta sẽ có 1 `DOM element`, `props` sẽ tương ứng với các `propeties của tag tương ứng`
  ```javascript
    {
      type: 'button',
      props: {
        className: 'button button-blue',
        children: {
          type: 'b',
          props: {
            children: 'OK!'
          }
        }
      }
    }
    // Nếu children chỉ là 1 string thì đó chính là nội dung của tag, nếu là 1 object thì nó sẽ là tag con của tag cha hiện tại
    // Nếu muốn có 1 children tree, ta chỉ cần tạo ra 1 mảng các child trong children props
  ```

  + Các element kiểu này chỉ là các `mô tả - description` chú không phải là các `instance` thực sự, chúng không hề tham chiếu tới bất cứ thành phần nào trên màn hình
  + Chúng nhẹ hơn `DOM elements` khá nhiều - chúng chỉ là `object`

### 8.2. Component elements
  + Khi `type` là các `functions, class tương ứng với React component` chúng sẽ được coi là `Component elements`
  + Hoàn toàn có thể mix `2 loại elements` với nhau

## 9. React Router

### 9.0. Sơ lược
  + Trong `React`, chỉ có `1 file HTML` được gọi. Bất cứ khi nào người dùng nhập 1 đường dẫn mới, thay vì lấy dữ liệu từ `server`, `Router` sẽ chuyển sang `1 Component` khác ứng với mỗi đường dẫn mới. Đối với người dùng thì họ đang chuyển qua lại giữa nhiều trang nhưng thực tế, mỗi `Component` đơn lẻ sẽ được `render lại phụ thuộc đường dẫn`
  + Trong `React`, `Router` sẽ kiểm tra `History` cho mỗi `Component` và bất cứ khi nào có thay đổi trong `History`, `Component` đó sẽ được `render lại`
  + Trước khi có `React Router v4`, ta phải set giá trị `History` thủ công. Việc này đã được tự động ở `Router v4` bằng thẻ `<BrowserRouter>`

### 9.1. Router
  * `<Router>`: giao diện level thấp cho mọi `routers components`
  * Thường thì app sẽ sử dụng 1 trong số các `router level cao hơn` sau đây
    - `<BrowserRouter>`
    - `<HashRouter>`
    - `<MemoryRouter>`
    - `<NativeRouter>`
    - `<StaticRouter>`
  * Thường dùng khi muốn đồng bộ hóa `custom history` với `state management lib: Redux`
  * `history: object`: sử dụng cho navigation - điều hướng

  ```javascript
    import createBrowserHistory from 'history/createBrowserHistory'
    
    const customHistory = createBrowserHistory()
    <Router history={customHistory}/>
  ```

### 9.2. BrowserRouter
  * `router` sử dụng `HTML5 API history` `(pushState, replaceState, popstate event)` gíup UI đồng bộ với URL

  ```javascript
    import { BrowserRouter } from 'react-router-dom'
      
    <BrowserRouter
      basename={optionalString}
      forceRefresh={optionalBool}
      getUserConfirmation={optionalFunc}
      keyLength={optionalNumber}>
      <App/>
    </BrowserRouter>
  ```

  * `basename: string`: `base URL` cho mọi locations, nếu `server` có `sub-directory` ta nên thiết lập `basename` với `sub-directory`
    - `basename` có `format` đúng là `basename` bắt đầu với '/', ko kết thúc bởi '/'

  ```javascript
    <BrowserRouter basename="/calendar" />
      <Link to="/today"/> // renders <a href="/calendar/today">
  ```

  * `getUserConfirmation: func`: hàm sử dụng để xác nhận navigation - mặc định là sử dụng `window.confirm`
    - `window.confirm`: hiển thị `modal dialogs` với `optional message` và `2 button`: `OK`, `Cancel`
    - `result = window.confirm(message);`: `result`: gía trị trả về: `true` nếu ấn `OK`, `false` nếu ngược lại
    - `message`: là xâu truyền vào trong `modal dialog`

  ```javascript
    // this is the default behavior
      const getConfirmation = (message, callback) => {
        const allowTransition = window.confirm(message)
        callback(allowTransition)
      }

      <BrowserRouter getUserConfirmation={getConfirmation}/>
  ```
### 9.3. Route
  + Sử dụng để render ra UI khi `location match với route's path`

  ```javascript
    import { BrowserRouter as Router, Route } from 'react-router-dom'
    <Router>
      <div>
        <Route exact path="/" component={Home}/>
        <Route path="/news" component={NewsFeed}/>
      </div>
    </Router>

    // '/':
      <div>
        <Home/>
        <!-- react-empty: 2 -->
      </div>
    // '/news':
      <div>
        <!-- react-empty: 1 -->
        <NewsFeed/>
      </div>
  ```
  + `react-empty`: `implement` của `React's null rendering`
  + Luôn luôn phải render ra Route kể cả khi `React's null rendering`
  + `Route render methods`
    - Có `3 cách` để `render với <Route>`
      * `<Route component>`
      * `<Route render>`
      * `<Route children>`
    - Chỉ nên sử dụng `duy nhất 1 trong số 3 loại trên` - tuy nhiên nên sử dụng `<Route component>`
    - `Route props`: 3 `render methods` trên sẽ truyền `3 props`: `match`, `location`, `history`
    - `Component`
      * Lúc này khi location được match thì `React.createElement` sẽ được gọi để tạo ra element mới, tương ứng với `Component` được truyền vào `component` - props
      * `React elements`: là đơn vị nhỏ nhất trong `React`, tương đương như `1 object plain`, `React DOM` sẽ quản lí việc update `DOM match với React elements`
      * Bản chất ở đây là `unmount component hiện tại` và `mounting component mới` thay vì `update component hiện tại`
      * Nếu trong `component` là `1 inline function` -> mỗi khi render sẽ tạo ra `1 component mới` -> thường dùng với `render`, `children`
    - `Render`: func
      * Render không cần `mount`
      * `<Route component>` ưu tiên cao hơn `<Route render>` -> ko dùng trong cùng `<Route>`
    - `Children`: func
      * Đôi khi ta cần render ra kể cả khi location `match path` hay `ko match`
      * `children` hoạt động giống như `render` ngoại trừ việc nó được gọi ngay cả khi `ko match`
    - `path`
      + `Route` ko có path thì luôn luôn match
    - `location`: object

### 9.4. Redirect
  + `current location` sẽ được lưu trữ tại `history stack`
  + `<Redirect> element` sẽ override `current location`
  + `to`: `string`: `URL` sẽ `redirect tới`
  + `to`: `Object`: `location` sẽ `redirect tới`
  + Mặc định thì `<Redirect>` sẽ thay thế `1 entry trong history stack` hiện tại
    - Nếu `push: bool = true` -> sẽ `push 1 "entry mới"` vào trong `history stack`
    - `from: string`: chỉ được sử dụng để chỉ `URL gốc` trong TH nó được lồng trong `<Switch>`

### 9.5. Switch
  + Sẽ bao ngoài nhiều `<Route>`
  - Nếu `1 <Route>` có thể match nhiều `URL`
  ```javascript
    <Route path="/about" component={About}/>
    <Route path="/:user" component={User}/>
    <Route component={NoMatch}/>
    // Nếu URL là: "/about" -> cả 3 component đều match
    // Tuy nhiên ta muốn chỉ 1 <Route> được render ra mà thôi

    import { Switch, Route } from 'react-router'
    <Switch>
      <Route exact path="/" component={Home}/>
      <Route path="/about" component={About}/>
      <Route path="/:user" component={User}/>
      <Route component={NoMatch}/>
    </Switch>
    // Nếu vậy thì khi URL là /about thì sẽ chỉ có about match mà thôi, sau khi match xong thì Switch sẽ ngừng tìm kiếm các Route matched khác
    // Route sẽ match dựa vào path, Redirect sẽ match dựa vào from
  ```
  - `location`: object
    * `match` với `children elements` thay vì `current history location`

### 9.6. history
  + Thường sẽ dùng `browser history` với các browsers hỗ trợ `HTML5 history API`
  + Các `methods, properties` của `history object`
    - `location` - `object`
    - `push(path, [state])`: push `new entry` vào `history stack`
    - `history is mutable`: có thể thay đổi vì thế nên truy nhập vào location nhận từ `props của <Route>` ko phải từ `history.location`

### 9.7. location
  + `Route` chứa `location` trong: `props.location` -> không nên sử dụng `location trong history.location`
  + `location object`: không bao giờ bị thay đổi
  ```javascript
    const location = {
      pathname: '/somewhere'
      state: { fromDashboard: true }
    }

    <Link to={location}/>
  ```

## 10. Flux

### 10.1. Giải pháp dữ liệu 1 hướng
  + Dữ liệu chỉ di chuyển theo 1 và chỉ 1 hướng duy nhất
  + Khi 1 dữ liệu mới xuất hiện, luồng sẽ bắt đầu lại từ đầu
  ```javascript
    Action -> Dispatcher -> Store -> View
    ^	                          |			
	|_______________________________v
  ```

### 10.2. The Action Creator
  + Có nhiệm vụ là tạo ra các `action`
  + Là bước đầu tiên trong luồng mà các thay đổi, tương tác đều đi qua
  + Khi nào mà trạng thái của `Web app` thay đổi, `rendered view` thay đổi thì đầu tiên là 1 `action` sẽ được tạo ra
    - `Action` sẽ có định dạng mà mọi thành phần hiểu được
    - Biết ta cần truyền đạt điều gì
  + `The Action Creator` sẽ tạo ra 1 `action` cùng với `type` & `payload` của `action` đó
    - `Type` thường sẽ là `1 hằng số` được định nghĩa trước: `MESSAGE_CREATE` hay `MESSAGE_READ`
    - Nên định nghĩa trước các `Type` dưới dạng `hằng số` trong 1 file vì khi đó sẽ dễ dàng hiểu được project
  + Sau khi `The Action Creator"`tạo ra 1 `Action` -> sẽ gửi nó cho `The Dispatcher`

### 10.3. The Dispatcher
  + Về cơ bản là 1 tập hợp của rất nhiều các `callbacks`
  + Luôn biết trước 1 danh sách các `store` để gửi `action` tới
  + Khi 1 `action` được gửi tới `The Dispatcher` thì nó sẽ gửi `action` tới `store` tương ứng theo quy tắc đồng bộ
  + `action` được gửi đến mọi `store` đăng kí với `Dispatcher` - bất kể `type gì`
  + Mỗi `store` sẽ nhận nghe mọi `action` & tự `filter` để xử lí

### 10.4. The Store
  + Gĩư toàn bộ `trạng thái` & `logic chuyển trạng thái` của app
  + Tất cả mọi thay đổi trạng thái đều được thực thi trực tiếp ở đây
  + Mỗi khi muốn thay đổi 1 trạng thái ta cần tạo ra 1 `action`, `submit` vào `Action creator`, đi qua `The Dispatcher` rồi mới được `The Store` xử lí
  + `1 store` sẽ nhận về khá nhiều `actions` nhưng trong `store` sẽ có 1 `cấu trúc switch` để quyết định xem có cần phải quan tâm tới `action` hay không

### 10.5. The controller view and the view
  + `The view` có nhiệm vụ nhận các thay đổi trạng thái và render hiển thị, cũng như nhận input từ người dùng
  + `The view` sẽ chỉ biết đến dữ liệu và cách hiển thị dữ liệu đó (thành HTML) cho người dùng
  + `The controller view` sẽ đứng giữa `store` và `view`
    - Nhận thông báo khi trạng thái thay đổi
    - Tổng hợp lại những nội dung cần thay đổi và truyền đến view trực thuộc

### 10.6. Workflow (Setup)
  1. `Store` sẽ đưa cho `Dispatcher`: `callback`, khi nào có `action` thì `callbacks` sẽ được `trigger`
  2. `Controller view` hỏi `Store` về trạng thái cuối
  3. Sau khi `Store` đưa trạng thái cuối cho `Controller view`, `Controller view` sẽ đưa cho `view` để `render hiển thị`
  4. `Controller` và `View` cũng nhắn với `Store`

### 10.7. Dataflow
  + Sau khi `setup`, app đã sẵn sàng nhận input
  + Nếu lúc này `trigger 1 action` bằng cách để người dùng tạo ra 1 sự thay đổi
    1. `View` sẽ nói với `Action Creator` tạo ra `1 action`
    2. `Action Creator` tạo ra `1 action` và chuyển tới `The Dispatcher`
    3. `Action Dispatcher` gửi lại `action` cho `mọi store liên quan`. `Mỗi store` nhận `action` xong sẽ quyết định xem có thay đổi trạng thái dựa vào action hay không
    4. Sau khi thay đổi trạng thái xong, `Store` sẽ lại thông báo cho `View Controller` đã đăng kí
    5. `View Controller` hỏi lại `Store` cụ thể về trạng thái mới
    6. Sau khi nói lại cho `View Controller` các thông tin trạng thái cụ thể, `View Controller` sẽ nói lại với các `Views` của mình `render lại hiển thị`

## 11. Redux

### 11.0. Mô hình

#### 11.0.1. The Action Creator
  + Cứ khi nào có tương tác từ phiá người dùng, hay trạng thái của app thay đổi thì `action creator` sẽ tạo ra các `action`
  + Mỗi `1 action` sẽ là 1 `formatted object` chứa `type` cũng như các dữ liệu khác
    - `type` thường sẽ là 1 hằng số được định nghĩa trước: `INCREASE`, `DECREASE`

#### 11.0.2. The Store
  + Quản lí `state tree (getState, updateState, registerListener...)`
  + Thực ra `The Store` sẽ chỉ quản lí `state` mà thôi
  + Sau khi nhận được `actions` nó sẽ gửi cho `The Reducer` để xem xem `state` thay đổi ra sao
  + Trong `Redux` sử dụng tư tưởng của `Functional programming` nên `Store` tự biết cách hiểu `action` và tự điều phối nó
  + Các `state` là các `private object` được bao bởi `store`

#### 11.0.3. The Reducers
  + Trong `The Reducers` sẽ có 1 `root reducer`
  + `root reducer` sẽ có nhiệm vụ cắt ra `state cần thay đổi` dựa trên `keys` mà `Store` gửi tới và đưa cho `reducer` biết cách xử lí
  + Các `reducer` sẽ tạo ra bản sao cho những thứ được đưa cho và thao tác trên bản sao đó
  + Sau khi các thao tác hoàn tất thì `các mảnh state` sẽ được ghép lại với nhau để tạo ra `1 state mới`
  + `Redux` không thao tác trên `state`
  + `root reducer` sẽ ghép các `mảnh state` lại
  + Sau đó `root reducer` sẽ gửi lại các `state mới` cho `Store` và `Store` sẽ sử dụng các `state mới` này 

#### 11.0.4. The views: smart and dumb components
  + `Smart component`: có thể gọi là `containers`
    - Khi các thành viên phía dưới của nó (`Dumb component`) muốn phát ra các `actions`, `Smart component` sẽ gửi `action` cho các thành viên dưới dạng `props`
    - `Dumb components` chỉ cần quan tâm chúng như các `callbacks`
    - Khi cần thay đổi `DOM` thì nó sẽ sắp xếp cho các thành viên phía dưới làm chứ không tự làm
    - Không có `CSS`
  + `Dumb components`: có thể gọi là `components`
    - Có `CSS` riêng
    - Nhận `props type` từ `Smart component`
    - Không phụ thuộc trực tiếp vào `action`

#### 11.0.5. The view layer binding
  + Trong `React` nó sẽ là `react-redux`
  + Kết nối `The Store` và `The views`
  + Cung cấp 3 khái niệm
    - `The Provider component`: Là thành phần bao quanh `components tree`, giúp các `components con` kết nối dễ dàng với `Store` thông qua `connect()`
    - `connect()` là hàm cung cấp bởi `The Provider component`, nếu `1 component` nào đó muốn nhận được `update State` thì nó phải tự bao lại bằng `connect()`,
    sau đó `connect()` sẽ thiết lập tất cả các hệ thống liên kết cho `component` bằng cách sử dụng `selector`
    - `selector` sẽ là hàm tự viết, nó sẽ chỉ rõ phần nào của `state` mà `component` cần như `properties`

#### 11.0.6. The root component
  + Mọi `React app` đều có `root component`
  + Tạo ra `The Store`, chỉ định `The reducers` nào được sử dụng, tập hợp `The view layer binding` cùng với `The Views`
  + Sau khi tập hợp đầy đủ, các bộ phận bên dưới sẽ tự hoạt động

#### 11.0.7. Setup
  1. `Root component` sẽ tạo ra `Store`, chỉ cho `Store` dùng `Root reducer` nào thông qua `createStore()`, `Root reducer` sẽ tập hợp các `reducers` lại thông qua `combineReducers()`
  2. `Root component` sẽ bao các `subcomponents` với `Provider component(The view layer binding)`, tạo ra kết nối giữa các `Provider`, `Provider` sẽ tạo ra 1 mạng cơ bản để cập nhật các `components`, `Smart Component` kết nối vào mạng bằng `connect()`, đảm bảo các `components` này nhận được `state`
  3. Chuẩn bị các `actions callback`: Để các `Dump Components` làm việc với `Action` dễ dàng hơn, các `Smart Components` có thể chuẩn bị các `action callback` thông qua `bindActionCreators()`. Bằng cách này, họ chỉ có cần truyền các callback cho Dump Components. Các Actions sẽ được tự động gửi đi sau khi nó được định dạng.

### 11.1. Basics

#### 11.1.1 Actions
  + Là các `payloads`
  + Có nhiệm vụ gửi dữ liệu từ `app` xuống tới `store`
  + Chúng là nguồn thông tin duy nhất cho `store`
  + Các `actions` được gửi đến `store` thông qua phương thức: `store.dispatch()`
  + `actions`: là các `JS objects thuần`
  + `action` phải có `type property` để định nghĩa loại hành động mà nó thực thi
  ```javascript
    const ADD_TODO = 'ADD_TODO'
    {
      type: ADD_TODO,
      text: 'Build my first Redux app'
    }
  ```
  + `type` nên được định nghĩa như là các `string constant`
  + Có thể để các `actions` vào trong mảng, mỗi khi có 1 phần tử mới được tạo ra, ta sẽ gen cho nó `1 ID duy nhất`
  ```javascript
    {
      type: TOGGLE_TODO,
      index: 5
    }
  ```

#### 11.1.2. Action Creators
  + Bản chất là các `functions` tạo ra các `actions`
  + Trong `Redux` thì `actions creator` chỉ đơn thuần trả về `1 action`
  ```javascript
    function addTodo(text) {
      return {
        type: ADD_TODO,
        text
      }
    }
  ```
  + Trong `Redux` không có `The Dispatcher` nên sau khi tạo ra `action`, nó sẽ được truyền cho `Store` thông qua hàm `dispatch()`
  ```javascript
    dispatch(addTodo(text))
    dispatch(completeTodo(index))
  ```
  + Có thể tạo ra `bound action creator`
  ```javascript
    const boundAddTodo = text => dispatch(addTodo(text))
    const boundCompleteTodo = index => dispatch(completeTodo(index))
  ```
  + Bây giờ có thể gọi trực tiếp:
  ```javascript
    boundAddTodo(text)
    boundCompleteTodo(index)
  ```
  + Có thể dùng `store.dispatch()` nhưng nên dùng `connect()` của `react-redux`
  + Có thể sử dụng `bindActionCreators()` để tự động bind các `action creators` vào hàm `dispatch()`

#### 11.1.3. Reducers
  + Chỉ rõ cách `state` thay đổi như thế nào trong `app`

##### 11.1.3.1. Designing the State Shape
  + Trong `Redux` mọi `state` được lưu trong `1 object` duy nhất
  + Trước khi code, nên thiết kế `State Shape` trước
  + Luôn giữ cho `data` tách biệt so với `UI State`

##### 11.1.3.2. Handling Actions
  + Reducer là 1 hàm, nhận vào `previousState`, `action` và trả về `nextState`
  ```javascript
    (previousState, action) => newState
  ```
  + Trong `Reducer` sẽ không:
    - Thay đổi arguments
    - Thực hiện `call API`, `routing transitions`
    - Gọi `non-pure function`
  + Khi `reducer` được gọi lần đầu tiên, nó sẽ được trả về `undefined state`
  + Trong trường hợp mà `reducer` không match bất cứ `actions` nào, ta sẽ trả về `state ban đầu`

##### 11.1.3.3. Handling More Actions
##### 11.1.3.4. Splitting Reducers
  + `reducer composition`: là việc ta tạo ra 1 `reducer` và nó sẽ bao lấy các `reducers con` khác, các `reducers con` này
    sẽ tương ứng với từng `state` một, `reducer lớn hơn kia` được gọi là `root reducer`, các `reducers con` sẽ quản lí các
    `state` khác nhau và đều thuộc về `global state`
  + `Redux` cung cấp `combineReducers()` để tổ hợp các `reducers` lại với nhau
  ```javascript
    const todoApp = combineReducers({
      [visi: ]visibilityFilter,
      [td: ]todos
    })
  ```

#### 11.1.4. Store
  + Là `object` bao ngoài `The reducers và The state`
  + Nó có chức năng:
    - Lưu các `states`
    - Cho phép truy cập `state` thông qua `getState()`
    - Cho phép `update state` thông qua `dispatch(action)`
    - Đăng kí `lister`: `subscribe(listener)`
    - Xử lí bỏ listeners thông qua hàm trả về bởi `subscribe(listener)`