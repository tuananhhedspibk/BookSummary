# Pro-React :rocket: :green_book: :memo:

## Chapter1: Getting Started

### 1.1 Reactive Rendering is Simple
  * Các single page app fetch dữ liệu từ server rồi đưa nó vào DOM như là 1 phần của 1 user interact
  * Data-binding: là cách làm trước đây của các JS framework giúp giảm bớt đi tính phức tạp, giúp cho UI đồng bộ với` state`
  * Reactibe rendering: cho phép ta có thể viết ra định nghĩa về việc 1 component được render, look, hành động như thế nào
    - Khi dữ liệu thay đổi React sẽ render lại giao diện 1 lần nữa
    - Để tiết kiệm tài nguyên thay vì render lại toàn bộ giao diện thì React sử dụng 1 DOM "nhẹ": Virtual DOM - in-memory
    - Các thao tác với Virtual-DOM đều nhanh, khả chuyển hơn so với DOM
    - Khi `state` thay đổi (tương tác với user, fetching dữ liệu), React sẽ nhanh chóng tính toán sự thay đổi nhỏ nhất của `state` của real DOM

### 1.2. Component-Oriented Development Using Pure JavaScript
  * Trong React app, mọi thứ đều được coi là component, đều tự chứa (self-contained)
  * Dev app sử dụng components sẽ cho phép chúng ta dễ dàng chia để trị mà không cần quá nhiều sự phức tạp ở đây
  * Chúng được chia nhỏ - rất nhỏ => nên có thể tổ hợp với nhau, từ đó có thể tạo ra các components phức tạp và nhiều thuộc tính ,tính năng hơn
  * Thuần là JS

### 1.3. Tính linh hoạt, trừu tượng của Document Model
  * Ta có thể sử dụng cùng 1 nguyên lí cho việc render HTML cho web, cũng như iOS, Android app
  * Render ra các components cũng cải thiện hiệu năng, SEO

### 1.4. Building Your First React App
  * 1 React component đơn thuần là 1 JS class với render method, return description của 1 component
  * JSX là 1 cấu trúc mở rộng của JS cho phép chúng ta viết XML, HTML trong JS code
  * JSX convert sang Javascript function call

### 1.5. React Development Workflow
  * Trong các React app thông thường, ta sẽ có các workflow như sau:
    - Viết JSX, biến đổi nó qua JS 1 cách nhẹ nhàng
    - Viết code theo module pattern
    - Quản lí các dependencies
  * Cấu trúc của 1 React project
    - A source folder: gồm các JS modules
    - index.html file. Trong các React app, các HTML page thường có xu hướng trống (empty), trách nhiệm của chúng chỉ đơn thuần là load các JS codes, cung cấp các element như div cho React render ra
    - package.json: manifest file của npm cho việc quản lí app, sẽ tự động download, install các dependencies cho app
    - A module packager or build tool
  * Giúp tổ chức, chia code thành các module nhỏ, dễ quản lí

### 1.6. Creating Your First Component
  * `import React` để chắc chắn rằng React lib được bundle vào JS code
  * `{ Component }` để tránh việc code nhiều: React.Component
  * Trong JSX, {variable}: JS expression

### 1.7. Composing Components
  #### 1.7.1. Props
  * Chìa khóa giúp ta có thể tái sử dụng, tổ hợp các components là config cho chúng
  * Props là cơ chế dùng để truyền dữ liệu từ parent xuống children components
  * Chúng không thể bị thay đổi ở child components, Props được truyền và sở hữu bởi parent components
  * Trong JSX, Props giống như tag attribute tương tự như HTML
  #### 1.7.2. Build a Kanban App
  ##### 1.7.2.1. Defining Component Hierarchy
  * Một trong những điều quan trọng khi dùng React đó là phải bóc giao diện thành các components lồng nhau. Ta cần thực hiện 3 bước sau:
    - Mỗi component nên đủ nhỏ và có 1 nhiệm vụ duy nhất, ta luôn nên cân nhắc việc tách nhỏ các components thành các subcomponents
    - Nên phân tích kĩ project's wireframe và layout vì chúng sẽ hỗ trợ ta trong việc tạo ra cây kế thừa của các components
    - Xem xét kĩ về data-model vì interface, data-model thường có cùng 1 kiến trúc thông tin. Ta sẽ break nhỏ giao diện thành các components để chúng là 1 phần trong data-model
  ##### 1.7.2.2. Building the Components
  * Có 2 cách tiếp cận chính để xây dựng các components
    - top-down
    - bottom-up
  ##### 1.7.2.3. Introducing State
  * Là dữ liệu có thể thay đổi
  * Các dữ liệu thay đổi này sẽ được lưu trong `this.state`
  * `this.state` là private với component, chỉ có thể thay đổi thông qua việc gọi tới: `this.setState`
  * Khi `state` được update thì component, con của component sẽ được re-render (reactive render)
  * Ta có thể set `state` bất cứ lúc này tuy nhiên nếu ta muốn component có initial `state` thì ta nên thiết lập trong class constructor

## Chapter 2: Inside the DOM Abstraction
  ### 2.1. Events in React
  * React cung cấp 1 `event system` cải thiện sự đồng nhất và hiệu năng cho React app và interfaces
  * React tạo ra sự đồng nhất thông qua việc đơn giản hóa các events để chúng có cùng properties ở các browsers và các platforms khác nhau
  * React tạo ra hiệu năng cao thông qua việc tự động sử dụng `ủy nhiệm event`
    - React không thực sự gán các `event handler` vào trong các nodes trong cây DOM ảo của nó
    - Mà sẽ có 1 `single event listener` được gắn vào nút gốc của document
    - Khi 1 event xảy ra, React sẽ map nó với component element tương ứng
    - React cũng sẽ tự động remove event listeners khi component unmount
  #### 2.1.1. DOM Event Listeners
  * HTML cũng đã cung cấp event handling API cho `tag attributes`: onClick, onFocus
    - Tuy nhiên nó không được sử dụng trong các project lớn
  * Nó làm xấu đi global scope
  * Khá khó kiểm soát trong file `HTML` lớn
  * Chậm, tốn bộ nhớ
  * JSX sẽ loại bỏ đi sự phức tạp này từ `HTML counterpart`
  * Callback function là scope của các components
    - Nó đủ thông minh để sử dụng `ủy nhiệm event`, tự động quản lí unmounting
  * Trong React các properties đều là `camel case` (onClick)
    #### 2.1.2. Kanban App: Managing the `DOM Event`
    #### 2.1.3. Digging Deeper in `JSX`
      * `JSX`: cú pháp mở rộng của Javascript, dùng để viết các định nghĩa XML trong JS code
      * `React JSX` cung cấp các `XML tags` tương tự như HTML tags
      * XML tags có thể được sử dụng đẻ miêu tả UI
      * Khi được chuyển đổi về Javascript thuần - để browser, server có thể hiểu được code, XML được biến đổi thành function call tới React lib
        - VD: 
          ```javascript
            <h1>Hello</h1>
            React.createElement('h1', null, 'Hello world');
          ```
      * Ưu điểm của `JSX`
        - Dễ dàng biểu diễn UIs trong `element trees` với `attributes`
        - Dễ dàng theo dõi cấu trúc của app
        - Chỉ là JS
  ### 2.2. JSX vs. HTML
  * JSX có thể khá giống HTML về mặt cú pháp nhưng cách triển khai lại hoàn toàn khác nhau
    - Vẫn đảm bảo cú pháp bản chất của JS
  #### 2.2.1. Differences Between JSX and HTML
  * 3 điều cần chú ý khi viết HTML trong JS
    - Tag attributes đều là "camel case"
    - Mọi element đều phải cân bằng: Với các tag không có ending tag thì nên self-closed `(<br> => <br/>)`
    - Các attributes name đều dựa trên DOM API không phải là HTML
      - "class, className"
  #### 2.2.2. JSX Quirks
  ##### 2.2.2.1. Single Root Node
  * Các React components chỉ có thể render 1 single root node
  * VD:
    ```javascript
        return(
          <h1>Hello World</h1>
        )

        ==> return React.createElement("h1", null, "Hello World");

        return (
          <h1>Hello World</h1>
          <h2>Have a nice day</h2>
        ) => invalid

        ==> return statement chỉ có thể return 1 gía trị đơn

        return (
          <div>
            <h1>Hello World</h1>
            <h2>Have a nice day</h2>
          </div>
        )

        ==> return React.createElement("div", null,
          React.createElement("h1", null, "Hello World"),
          React.createElement("h2", null, " Have a nice day"),
        )
    ```
  ##### 2.2.2.2. Conditional Clauses
  * `if statement` không sử dụng trong JSX
  * What Are the Alternatives?
    - Sử dụng toán tử 3 ngôi
    - null và undefined values được xử lí bởi React và output `nothing` khi escaped trong JSX
  * Move the Condition Out
    - Nếu toán tử 3 ngôi không đủ mạnh, ta sẽ không sử dụng Conditional trong JSX, ta sẽ di chuyển conditional ra bên ngoài
    - React sẽ xử lí các undefined value và sẽ không render ra className nếu conditional = false
  #### 2.2.3. Kanban App: Indicating Whether a Card Is Open or Closed
  ##### 2.2.3.1. Blank Space
  * Trong HTML, browsers sẽ render ra khoảng cách giữa các elements khác nhau ở trên nhiều dòng
  * `React JSX` sẽ chỉ render ra khoảng trắng nếu chỉ định tường minh thông qua: {" "}
  ##### 2.2.3.2. Comments in JSX
  * Đặt {} xung quanh JS comment khi ở trong inner của tag
  #### 2.2.4. Rendering Dynamic HTML
  * React có sẵn sự bảo vệ `XSS-attack` - mặc định thì React sẽ không cho phép các thẻ HTML gen động và gắn vào JSX
  * Tuy nhiên trong 1 số trường hợp ta vẫn muốn gen ra HTML tags động - dữ liệu dưới dạng markdown
  ### 2.3. Kanban App: Rendering Markdown
  ### 2.4. React Without JSX
  * XML tron JS: JSX
  * React.createElement có thể sử dụng để render ra element - tag name, component, properties object là arguments
  * React cung cấp các factory function dưới dạng React.DOM
  ### 2.5. Inline Styling
  * Sử dụng JSX: tổ hợp UI defination và tương tác JS trong cùng 1 file
  ### 2.6. Inline Styling
  * Viết inline style sử dụng JS
  #### 2.6.1. Defining Inline Styles
  * Trong React's components, inline style được chỉ ra như là JS object
  * Style name là camel Cased
  * Không cần chỉ ra các đơn vị như px - React sẽ tự động append
  * VD:
    ```javascript
      let divStyle = {
        width: 100,
        height: 30,
        padding: 5
      }
      return <div style={divStyle}>Hello World</div>
    ```
  #### 2.6.2. Kanban App: Card Color via Inline Styling
  * Sử dụng inline-styling sẽ gíup config được style động, dựa theo `state`
  #### 2.6.3. Working With Forms
  * Trong React thì số lượng` state` sẽ được gĩư nhỏ nhất vì mỗi khi `state` thay đổi thì components sẽ được render lại
  * Các form components trong React <input>, <textarea> khác so với HTML, gíup cho React gĩư cho giao diện được đồng bộ
  * React có 2 cách xử lí form như các components: controlled components, uncontrolled component
  ##### 2.6.3.1. Controlled Components
  * 1 form components có gía trị hoặc checked `props` được gọi là: controlled component
  * Gía trị ở bên trong controlled component sẽ luôn luôn tham chiếu theo gía trị của `props`
  * Mặc định thì user không thể thay đổi nó
  * onChange event sẽ là cách giúp user thay đổi được gía trị của `state`
  * Trong React form:
    - `State` được cách li, cách biệt so với interfaces, được quản lí hoàn toàn bởi JS code
    - Cách làm này sẽ gíup interface dễ dàng triển khai để có thể đáp ứng, validate user interactions
  ##### 2.6.3.2. Special Cases
  * Textarea: sử dụng `props` để set value
  * Select:
    ```javascript
      <select value='B'>
        <option value='A'>Mobile</option>
        <option value='B'>Work</option>
        <option value='C'>Home</option>
      </select>
    ```
  ##### 2.6.3.3. Uncontrolled Components
  * Anti-pattern
  * Đôi khi không cần thiết theo dõi các components - input field
  * Điều này khá hữu dụng khi form dài, ta sẽ để cho người dùng hoàn thiện toàn bộ form xong rồi sau đó mới check
  * Các input không cung cấp gía trị thì gọi là: `uncontrolled components`
  * Gía trị của rendered element sẽ reflect với user input - VD: khi render ra các text field trống rồi điền gía trị vào thì các gía trị này sẽ được hiển thị - reflect ngay lập tức
  * Có thể xử lí 1 form thông qua `onSubmit`
  #### 2.6.4. Kanban App: Creating a Task Form
  #### 2.6.5. Virtual DOM Under the Hood
  * React's key design: khiến cho API dường như tự re-render khi app update
  * Việc điều khiển DOM là 1 task khá chậm -> giảm hiệu năng
  * React sẽ sử dụng 1 virtual DOM để cải thiện hiệu năng
  * Thay vì update DOM mỗi khi `state` của app thay đổi thì React sẽ tạo ra 1 cây ảo giống hệt DOM
  * Khi đó sẽ không phải tạo lại toàn bộ DOM nodes
  * Quá trình tìm ra số lượng thay đổi nhỏ nhất khiến cho `Virtual DOM tree` khác biệt so với `DOM tree` gọi là: reconciliation -> phức tạp, tốn kém
  * React đưa ra 1 vài gỉa định để khiến cho các ứng dụng hoạt động tốt với các gỉai thuật nhanh hơn
    - Khi so sánh các nodes trong DOM tree, nếu các nodes này là khác loại (div --> span) thì React sẽ xử lí chúng như 2 cây con riêng biệt, bỏ đi cây con thứ nhất và build/insert vào cây con thứ 2
    - Logic trên cũng được áp dụng cho các `Custom Element`. Khi có sự khác biệt đó thì React sẽ không match những gì mà chúng render ra mà React sẽ remove cái đầu tiên trong DOM tree và insert vào cái thứ 2
  * Nếu các nodes này là cùng kiểu thì sẽ có 2 cách để React xử lí nó:
    - Nếu là DOM's element
      ```javascript
        <div id="before"/> --> <div id="after"/>
      ```
      * Thì React sẽ chỉ thay đổi các attributes và style (mà không thay thế element tree)
    - Nếu là "Custom components" 
      ```javascript
        <Custom details={false}/> --> <Custom details={true}/>
      ```
      * React sẽ không thay thế các components, React sẽ gửi các properties mới tới Component đang được mounted -> sẽ gọi lại hàm `render()` -> tái khởi tạo với kết quả mới
  #### 2.6.6. Keys
  * Việc liệt kê các phần tử có cấu trúc giống nhau được chú ý khi xử lí
    - Thay đổi 1 list: (abc, def) --> (def, bgf) là 1 thao tác hoàn toàn có thể xảy ra sẽ có 2 khả năng như sau
      * Thay đổi gía trị các item
      * Chuyển vị trí và chỉ thay đổi gía trị của 1 item
    - Trong các list lớn hơn thì sẽ có khá nhiều khả năng, cách tiếp cận khác nhau
    - Từ việc các items, nodes có thể được CRUD thì khá khó để có thể đưa ra 1 cách tiếp cận tốt nhất cho mọi trường hợp chỉ với 1 gỉai thuật duy nhất
  * Do đó React đưa ra `key attribute`
    - Các key này là khác nhau để phân biệt các cây nhanh chóng, tìm kiếm nhanh chóng khi CRUD
    - Khi ta tạo ra các items thông qua 1 vòng lặp thì ta nên đưa key vào mỗi item để React lib có thể match và ngăn chặn các vấn đề ảnh hưởng đến hiệu năng
  #### 2.6.7. Kanban App: Keys
  * Các gía trị của key `props` sẽ phải là hằng số và khác nhau
  #### 2.6.8. Refs
  * Khi React render ra 1 component, luôn luôn có sự thỏa thuận với `Virtual DOM`
  * Nếu ta thay đổi component's `state`, hay gửi `props` xuống 1 child thì sẽ re-render lại `Virtual DOM`
  * Sau đó React sẽ update DOM thật sau phrase `reconciliation`
  * Khi đó các dev sẽ không phải đụng vào `DOM tree`
  * Ta có thể sử dụng "ref" như là 1 string `props`
  * Các DOM markup tương ứng có thể được truy cập thông qua `this.refs`.
    ```javascript 
      <input refs='myInput'> 
      let input = this.refs.myInput, let inputValue = input.value
    ```

## Chapter3: Architecting Applications with Components
  ### 3.1. Prop Validation
  * Các components trong React luôn có thể tổ hợp lại thành các components lớn hơn và có thể tái sử dụng
  * Sử dụng propTypes: tường minh chỉ ra rằng: `props` nào được sử dụng, được required. type values nào được acceppted
  * propType sẽ tạo ra document cho component, khá tiện ích trong tương lai theo 2 cách:
    - Kiểm tra `props` nào được yêu cầu, data type nào nên sử dụng
    - Thông qua console React sẽ thông báo lỗi khi sai, `props` nào bị thiếu, sai, ...
  * propTypes được định nghĩa như là class constructor property
    
    ```javascript
      ComponentName.propTypes = {
        propertyName: PropTypes.data_type.option
      }
    ```
  * Default Prop Values
    ```javascript
      ComponentName.defaultProps{

      }
    ```
    - Để cả khi `props` không được cung cấp thì component vẫn có dữ liệu dồn xuống
  * Built-in propType Validators
    - Các validator này là tùy chọn tuy nhiên ta có thể thêm vào với isRequired
    - 1 số các validator proptype đặc biệt
      * PropTypes.node: number, string, elements của mảng
      * PropTypes.element: React element
      * PropTypes.oneOf: đảm bảo prop được giới hạn bởi các gía trị cụ thể được chỉ ra như enum
        - PropTypes.oneOf(['News', 'Photos'])
  * Ta nên định nghĩa ra PropTypes ngay khi mà Component được tạo ra
  * Custom PropType Validators
    - Các validators này là các functions nhận 1 list properties, tên của properties, của component để check
    - Các functions này hoặc không trả về gì nếu (`props` valid) hoặc trả về instance của Error phù hợp với lỗi của prop
  ### 3.2. Component Composition Strategies and Best Practices
  #### 3.2.1. Stateful & Pure Components
  * Components có dữ liệu ở dạng `props` và `state`
    - `Props`: là component config, được nhận từ parent và không thể thay đổi
    - `State`: có gía trị mặc định được khởi tạo trong constructor của component
      * Được quản lí bởi component chứa nó, cứ mỗi khi `state` thay đổi, component được render lại
      * Trong các react app sẽ có 2 loại components:
        - Stateful component là các component có `state`
          * Khá tốt khi dùng để respond lại user input, server request
          * Khá tốt trong kết thừa các components, bao lấy 1 hoặc nhiều `stateful` hoặc `pure components`
        - Pure component: không hề có internal` state`, chỉ đơn thuần là hiển thị dữ liệu
          * Sẽ chỉ nhận về `props` và render ra - dễ dàng test và tái sử dụng
      * Ta nên gĩư cho các component không có `state`
        - Nếu có nhiều `component's state` thì khá khó để quản lí
        - Flow của app sẽ thiếu trong suốt
  * Which Components Should Be `Stateful`?
    - Xác nhận rằng các components đều render ra dựa trên 1 vài `states` nào đó
    - Tìm 1 component chung của các components mà cần state
    - Hoặc là cha chung hoặc là component cao hơn trong cây kế thừa nên có state của riêng nó
    - Nếu như không thể tìm ra component nào chứa state, ta sẽ tạo ra 1 component mới, chỉ chứa state, sau đó add nó vào cây kế thừa ở phía trên của component chủ chung
  * Data Flow and Component Communication
    - Trong thực tế, các `nested components` cũng cần tương tác lại với `parent component`
    - Để làm được điều này, ta có thể sử dụng callback được truyền từ component cha như là 1 `props`
    - Cứ mỗi khi form hay input field thay đổi, ta cũng sẽ thay đổi `state` để reflect lại sự thay đổi đó
    - Callback được truyền xuống như 1 `props` và sẽ thực thi mỗi khi mà `state` thay đổi
    - Sử dụng `onChange` event của input field để thực hiện
  #### 3.3. Component life cycle
  * `Mounting`: 
    - `class constructor`

      -> `componentWillMount` (được gọi 1 lần ngay trước khi lần render đầu tiên xảy ra - việc thiết lập state trong phase này sẽ không làm compnent re-rendering)

      -> `render`

      -> `componentDidMount` (được gọi ngay sau khi lần render đầu tiên diễn ra - gọi 1 lần, component đã có DOM representation - có thể truy cập được - khá thích hợp khi fetching dữ liệu ở phase này)
  * `Unmounting`:
    - `componentWillUmount` (được gọi ngay trước khi component được unmount ra khỏi DOM - khá hữu dụng khi cần phải clean up các operations - remove event listener's timers được định nghĩa trong Mounting lifecycle)
  * `Props` change:
    - `componentWillReceiveProps` (được gọi khi component nhận 1 `props` mới, gọi this.setState() trong hàm này sẽ không trigger render nào cả)

      -> `shouldComponentUpdate` (được gọi trước render function)

      -> `componentWillUpdate` (được gọi ngay trước khi rendering khi mà `props` hoặc `state` mới được nhận, mọi `this.setState` không cho phép trong hàm này để tránh tự update)

      -> `render`

      -> `componentDidUpdate` (được gọi ngay sau khi component's update được đưa vào DOM - flushed)
  * State change:
    - `shouldComponentUpdate`

      -> `componentWillUpdate`

      -> `render`

      -> `componentDidUpdate`
  #### 3.4. Lifecycle Functions in Practice: Data Fetching
  * Khi fetch data ta nên fetch trên `componentDidMount`
  * Container Component:
    - Ta nên tạo ra 1 `stateful component` có các chức năng sau:
      * Tương tác với API
      * Truyền dữ liệu và callbacks xuống như là `props`
    - Dữ liệu json nên ở trong `public/` hoặc `static/` folder
  * Nên sử dụng this.setState thay vì thay đổi `state` trực tiếp
    - `this.state` là không thay đổi được
    - Khi thay đổi trực tiếp state thì sẽ phá vỡ `React state management`
    - Để cải thiện hiệu năng, khi 1 object thay đổi thay vì edit ta sẽ replace
  #### 3.5. Immutability in Plain JavaScript
  * Ý tưởng cơ bản của Immutability đó là thay thế object thay vì edit nó
  * Trong JS object và array được truyền theo tham chiếu (reference)
  * `this.state.passengers` sẽ tạo ra 1 tham chiếu đến object đó chứ không tạo ra 1 bản sao
  * Để tạo ra bản sao thực sự ta sẽ sử dụng: `non-destructive methods` - trả về 1 mảng với sự thay đổi đột biến thay vì sửa trực tiếp bản gốc của object
  * `map`, `filter`, `concat` là 1 vài ví dụ về non-destructive array methods
  * `Object.assign` : gen ra objects mới với mutation (có thể thay đổi)
      - `Object.assign` sẽ merged tất cả các properties của object truyền vào tới target object
        * `Object.assign(target, source_1, ..., source_n)` 
          -> copy lần lượt properties của các `source_1, ..., n`
  * Nested Objects
    - Object.assign, array’s non-destructive methods nó khá khó vận dụng với "nested object, arrays"
      * Do đặc tính của Javascript: object, arrays được truyền theo tham chiếu -> sẽ copies khá xâu
      * Khi đó các objects, array được lồng bên trong vẫn sẽ tham chiếu tới cùng objects, arrays lồng của object, array cũ
  * React cung cấp 1 add-ons package - chứa các hàm công cụ (tool functions) - immutability helper - update các objects complex, nested nhiều hơn
  #### 3.6. React Immutability Helper
  * Immutability Helper: update() method - thay đổi các object
  * update() method sẽ có 2 params
    - object, array ta muốn update
    - Nơi mutation xảy ra và loại mutation
    - VD:
      ```javascript
        let newStudent = update(student, {grades:{$push: ['A']}})
          // grades: where
          // push: what
      ```
  * Nếu muốn thay đổi toàn bộ mảng, ta có thể sử dụng $set thay cho $push
  * Array Indexes
    - Có thể sử dụng index khi update nested object, array
  * Available Commands
  * Khi fetch dữ liệu từ API về, ta luôn cần HEADER
  #### 3.7. Wiring Up the Task Callbacks as `Props`
  * Các hàm được viết này sẽ được truyền xuống như là các `props`
  * Ta có thể tạo ra 1 object đơn mà nó tham chiếu đến các functions này, và truyền object này xuống như là prop
  * Trước khi add task ta nên làm 1 vài thao tác tiền xử lí
    - Kiểm tra user có nhấn nút "enter" hay không
    - Clear input field sau khi gọi callback xong
  #### 3.8. Manipulating Tasks
  * `findIndex() array method`: chạy 1 testing function trên các elements, trả về index của elements thỏa mãn
  `testing function`
  * `JSON.stringify()`: sử dụng để chuyển dạng dữ liệu
    - Khi gửi dữ liệu lên server, data phải là string
    - `JSON.stringify()`: convert JSON object -> string
  #### 3.9. Basic Optimistic Updates Rollback
  * Để cải thiện UX, ta nên khiến cho UI cảm giác như được update ngay tức thì, kể cả khi server failed sẽ phải revert lại UI, thông báo cho user, ...
  * Tuy nhiên việc revert back lại là hoàn toàn có thể, vì ta vẫn track đến old state
  * Ta có thể sử dụng setState() để revert back lại state cũ, khi server response "not ok" hoặc fetch() thực thi bị lỗi

## 4. Sophisticated Interactions
  * React cung cấp 1 cách để làm animation với level cao: ReactCSSTransitionGroup
  ### 4.1. Animation in React
  * ReactCSSTransitionGroup tổ hợp CSS transitioning với React bởi việc cho phép ta có thể trigger CSS transitions và animations khi component được add, remove từ DOM
  ### 4.2. CSS Transition and Animation 101
  * Animation trong CSS: CSS transitions, CSS keyframe
    - CSS keyframe: định nghĩa animation
    - CSS transitions: định nghĩa trạng thái đầu, cuối của animation
      * Các bước trung gian sẽ được nội suy bởi trình duyệt
  * CSS keyframe
    - Có thể control cụ thể hơn ở các bước trung gian
    ```css
      @keyframes name_animation {
        
      }
    ```
  ### 4.3. Programmatically Starting CSS Transitions and Animations
  ### 4.4. React CSSTransitionGroup
  * Có thể trigger các CSS animations tại các thời điểm thích hợp, có liên quan tới component's lifecycle
  * Với các children của `ReactCSSTransitionGroup`, ta cần cung cấp key attribute ngay cả khi render ra 1 item duy nhất vì đây chính là cách mà React tìm ra children nào được thêm, loại bỏ
  ### 4.5. Adding the ReactCSSTransitionGroup Element
  * `ReactCSSTransitionGroup` sẽ bao lấy các elements mà ta muốn style
  * Nó nhận 3 `props`: transitionName, transitionEnterTimeout, transitionLeaveTimeout
    - transitionName: CSS class name bao gồm animation được định nghĩa
  * 1 trong những đặc tính của CSS transitions
    - Ban đầu sẽ có 1 class với 1 số các thuộc tính nhất định
    - Sau khi animation được triggers thì sẽ add thêm class thứ 2 với các animation transitions khác vào element
    - Sau khi thời gian trong transitionEnterTimeout hết hạn cả 2 classes sẽ bị loại bỏ
  ### 4.6. Animate Initial Mounting
  * Lúc này các initial items sẽ không có animation
  * `ReactCSSTransitionGroup` có 1 optional prop `transitionAppear` để add thêm transitions cho các initial component
    - Gía trị mặc định của `transitionAppear` là false
  ### 4.7. Drag and Drop
  * HTML5 hỗ trợ Drag-Drop API nhưng vẫn gặp conflict
  * React DnD sẽ hỗ trợ Drag, Drop
    - Không tương tác với DOM
    - Bao lấy data-flow theo 1 hướng duy nhất
    - Định nghĩa drag source, drop target chỉ là dữ liệu thuần
  #### 4.7.1. React DnD Implementation Overview
  * higher-order component sẽ dùng để implement cho React DnD
  * Nó là các JS functions
    - `import ... from 'react-dnd';`
      * Ơ 1 app cần `Drag-Drop` ta cần 3 loại components bên cạnh `App component`
        - `Draggable Snack` sẽ kế thừa từ `DragSource higher-order component`
        - `ShoppingCart` sẽ kế thừa từ `DropTarget higher-order component`
        - `Container component` sẽ bao gồm: `ShoppingCart` + rất nhiều Snacks, kế thừa từ `DragDropContext`
      * The Container
        - `export` ra dưới dạng 1 component ở cấp cao hơn, `import` và sử dụng `HTML5Backend` với `React-dnd`
        - `React Drag-Drop` cũng hỗ trợ các `backend` khác
      * DragSource and DropTarget Higher Order Components
        - Với các components kế thừa từ `dragSource`, `dropTarget` ta cần cung cấp 3 tham số: type, spec, tập hợp các `function` `(collecting function)`
        - `Type`
          * Bản chất là name của component
          * Với các UI phức tạp, có thể có nhiều types của `dragSource` và `dropTarget` tương tác với nhau - quan trọng là chúng phải khác nhau
        - `Spec Object`
          * Miêu tả cách mà các components con tương tác, phản ứng với các `drag, drop events`
          * Nó chỉ đơn thuần là JS object, có các functions sẽ được gọi khi sự kiện kéo thả xảy ra
            + `beginDrag`, `endDrag` - `DragSource`
            + `canDrag`, `onDrop` - `DropTarget`
        - `Collecting Function`
          * Trong `ReactDnD` thì `dragSource` và `dropTarget` cũng sẽ truyền các props xuống các component con
          * Thay vì truyền toàn bộ props xuống, collecting function sẽ đem đến cho người dùng khả năng kiểm soát, điều khiển cách các props được truyền xuống cũng như số lượng props được truyền - có thể tiền xử lí props được
          * Khi tương tác kéo thả xảy ra `ReactDnD` sẽ gọi thực thi các `collecting function` định nghĩa trong component, truyền vào 2 tham số: `connector`, `monitor`
          * `connector` sẽ phải map với props dùng để phân cách component vs DOM's component
            + Với dragSource thì thành phần này sẽ dùng để biểu diễn component khi nó được kéo
            + Với dropTarget thì thành phần phân cách này sẽ được sử dụng như drop area
          * `monitor` sẽ map props với Drag'nDrop state
            + drag drop là thao tác stateful - nó có thể đang xử lí hoặc nhàn rỗi
            + Nếu nó đang xử lí thì sẽ có current type, current item
            + Sử dụng monitor ta có thể tạo ra các props: isDragging, canDrop, .. -> khá hữu dụng khi render các component dựa theo nội dung
          * map DnD state -> component's props
        - `splice(index, howmany, item_1, ..., item_n)`: chèn phần tử
          * `index`: vị trí bắt đầu chèn
          * `howmany`: số phần tử bị bỏ đi
      * Throttle Callbacks
        - Vì vấn đề về mặt hiệu năng nên thay vì dùng phiên bản gốc của function ta sẽ sử dụng 1 phiên bản `throttled` của các functions
          * Tham số truyền vào bao gồm 2 cái: function gốc, khoảng thời gian timeout (wait)
          * Khi ta gọi hàm nhiều lần thì phiên bản gốc cùa hàm sẽ được gọi nhiều nhất 1 lần trong khoảng `wait milisecond`