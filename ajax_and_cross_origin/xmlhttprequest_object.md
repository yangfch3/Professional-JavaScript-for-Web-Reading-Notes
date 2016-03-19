# XMLHTTPRequest 对象
1. IE 5 是第一款引入 `XHR` 对象的浏览器；

2. 在不同版本的 IE 中可能会遇到三种不同版本的 `XHR` 对象，即 `MSXML2.XMLHttp`、`MSXML2.XMLHttp.3.0`、`MSXML2.XMLHttp.6.0`；

3. 现代浏览器都支持标准的 `XMLHTTPRequest` 对象；

4. `Ajax` 实现的根基就在于实例化的 `XMLHTTPRequest` 对象；

## XHR 的用法
1. `open()` 接受三个参数：
    * 请求类型字符串
    * 请求的 `URL`（相对路径或绝对路径）
    * 是否异步布尔值

    > 其实还接受两个可选参数：`username` `password`，但考虑到安全性，一般不用。

2. `URL` 使用相对路径时是相对于执行代码的当前页面；

3. `open()` 方法并不会真正发送请求，而只是启动一个请求以备发送；

4. `send()` 方法接收一个参数：作为请求主体发送的字符串（`GET` 下没有请求主体一说）；如不需要请求主体，传入 `null`；

5. 在一般情况下，会受到跨域限制，这是 **浏览器** 做出的安全限制；

6. 响应的信息会自动填充 `XHR` 对象的属性
    * `responseText`
    * `responseXML`
    * `status`
    * `statusText`

7. 一般 `status`（请求状态码）在 **200-300** 之间即表示正常加载，`304` 则是从缓存读取，也属于正常请求；

8. `statusText` 在不同浏览器下表现不同，所以跨浏览器时检测请求状态都采用 `status` 检测；

9. 无论请求内容类型是什么，响应主体的内容都会保存到 `responseText` 属性中，非 `XML` 数据，`responseXML` 属性的值为 `null`；使用 `responseXML` 的优势只是在 `XML` 数据下才体现出来；

10. `readyState` 属性表示 **请求/响应** 过程的当前活动阶段
    * 0：未初始化
    * 1：启动，已调用 `XHR` 实例的 `open()`
    * 2：发送，已调用 `XHR` 实例的 `send()` 方法
    * 3：接收，已收到部分响应数据
    * 4：完成，已接收全部数据，且已可以使用

11. `XHR` 实例可以触发 `readystatechange` 事件，通过 DOM 0级 的方式为 `readystatechange` 事件指定事件处理程序 -- `onreadystatechange`；

12.  `readystatechange` 事件在一些浏览器下不支持 DOM 2 级事件处理程序；

13. 必须在 `open()` 之前指定 `onreadystatechange` 事件处理程序；

14. 在 `onreadystatechange` 事件处理程序中不要使用 `this` 指代 `XHR` 对象，请引用实际的 `XHR` 对象实例变量；

15. `XHR` 实例的 `abort()` 方法：**在接收到响应之前** 取消异步请求
    > `xhr.abort()` 之后，`XHR` 对象会停止触发事件，也不再允许访问任何与响应相关的对象属性；
在 `abort()` 之后还应该对 `XHR` 对象解引用；
由于内存问题，不建议重用 `XHR` 对象。

## HTTP 头部信息
`setRequestHeader()` `getResponseHeader()` `getAllResponseHeader()`

1. XHR 对象提供了操作（读写）两种头部 -- **请求头和响应头** 的方法；

2. 默认情况下 `XHR` 对象一般会发送的头部
    * `Accept`
    * `Accept-Charset`
    * `Accept-Encoding`
    * `Connection`
    * `Cookie`
    * `Host`
    * `Referer`
    * `User-Agent`

3. `setRequestHeader()` 方法可以设置自定义的请求头信息
    * 参数一：头部字段名
    * 参数二：头部字段值

4. 必须在调用 `open()` 方法之后，调用 `send()` 方法之前调用 `setRequestHeader()`；

5. **使用自定义的头部字段名称（与后端沟通确认好）**，不要重写浏览器正常发送的字段（部分浏览器是禁止重写的）；

6. `getResponseHeader()` 接受一个参数 -- 头部字段名，并返回对应的头部字段值；

7. `getAllResponseHeader()` 取得字符串形式的整个头部信息。

## GET 请求
使用 `GET` 请求最常犯的一个错误是：
> 查询字符串未使用 `encodeURIComponent()` 编码转换

## POST 请求
1. 通常用于向服务器发送 **应该被保存的数据**；

2. 数据作为 `POST` 请求的 **主体** 提交；

3. `POST` 请求的主体应该是符合服务器端要求的一串 **字符串**（所以一般对象、表单之类通过 `XHR` 对象提交需要先经过序列化）；

4. `POST` 请求的注意还能是 `XML` `DOM` 文档，因为 `XHR` 设计的最初目的是处理 `XML`；

5. 使用 `XHR` 对象来提交表单时需要做到以下几点
    1. 将 `Content-Type` 头部信息设置为表单内容类型： `application/x-www-form-urlencoded`；
    2. 将表单中的数据进行序列化为服务器能识别的字符串，再通过 `XHR` 做为 `POST` 请求的主体发送。

6. `POST` 请求与 `GET` 请求的联系与不同之处
    * `GET` 专职于获取数据，`POST` 一般用于向服务器发送应该被保存的数据；
    * `GET` 没有请求主体部分，只有链接后的查询字符串；`POST` 则有请求主体；
    * 安全性高低：`POST` 请求安全性略高于 `GET` 请求；
    * `POST` 请求消耗的资源比 `GET` 请求高，`GET` 请求的速度也较快。
