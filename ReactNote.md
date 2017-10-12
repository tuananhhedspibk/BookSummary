# React note :green_book: :memo:

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
## 8. React Router
### 8.1. Router
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

### 8.2. BrowserRouter
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