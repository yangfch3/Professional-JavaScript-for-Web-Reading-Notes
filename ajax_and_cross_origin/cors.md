# 跨源资源共享
1. Web 发展早期使用 `Flash` `Java applet` 这些中间件实现跨源资源共享；

2. 在 Ajax 出现之前还有使用内嵌框架、多窗口实现跨源资源共享的 `hack` 方案；

3. 浏览器本地对不同域资源的共享存在限制；
    > 关闭 Chrome 的同源政策：[链接][1]

4. `Ajax` 实现跨域资源共享的前提是：得到目标域对本域的许可（`Access-Control-Allow-Origin`），服务器未做过多地限制。

## IE 对 CORS 的实现
1. 微软在 IE 8 中引入 `XDR` （XDomainRequest）类型，与 `XHR` 对象类似，但能实现可靠的跨域通信；

2. `XDR` 在实现跨域通信的同时，同时对于安全性做了以下限制
    * `cookie` 不会随之发送，也不会随响应返回 -- 防止 cookie 盗用
    * 只能设置请求头部信息中的 `Content-Type` -- 防止欺骗浏览器
    * 不能访问响应头部信息
    * 只支持 `GET` 和 `POST` 请求

3.  被请求的资源 **服务器** 可以根据它认为合适的任意数据（用户代理、来源页面等）来 **决定是否设置 `Access-Control-Allow-Origin` 头部**。作为请求头的一部分，`Origin` 的值表示请求的来源域

4. `XDR` 对象的使用方法与 `XHR` 对象非常相似
    * 先实例化一个 `XDR` 对象
    * `open()`
        * 参数一：请求类型（只支持 `GET` 和 `POST`）
        * 参数二：`URL`
    * `send()`
        * 参数：需要传送的数据

5. 请求的返回值与事件处理程序：
    * 请求返回之后会触发 `load` 事件；
    * 如果请求失败（包括响应中缺少 `Access-Control-Allow-Origin` 头）会触发 `error` 事件；
    * 响应的数据保存在 `responseText` 属性中；
    * 请求返回前可以使用 `abort()` 方法终止请求；
    * `XDR` 对象也支持 `timeout` 对象以及 `ontimeout` 事件处理程序；
    * `XDR` 对象提供了 `contentType` 属性用来表示发送数据的格式（主要针对 POST 请求）。

## 其他浏览器对 CORS 的实现
1. 基本现代浏览器都通过 `XMLHttpRequest` 对象实现了对 `CORS` 原生支持；

2. 前提仍旧是目标域未对资源做限制（对来源域开放，能为其设置 `Access-Control-Allow-Origin` 头）；

3. 跨域 `XHR` 对象的限制
    * 不能使用 `setRequestHeader()` 设置自定义头部；
    * 不能发送和接收 `Cookie`（需特殊设置，见下放 `withCredentials`）；
    * 调用 `getAllResponseHeader()` 方法总是返回空字符串。

4. **最佳实践**：对于同源的本地资源，最好使用相对 `URL`，在访问远程资源时再使用绝对 `URL`
    > 这样做能消除歧义，避免出现限制访问头部或本地 `Cookie` 信息等问题

## Preflighted Requests
1. `CORS` 的 `Preflighted Requests` 机制会在使用以下高级选项时触发
    * **自定义的头部**
    * **`GET` 和 `POST` 之外的方法**
    * **不同类型的主体内容**

2. `Preflighted Requests` 只是在正式发起请求之前的一次预请求，用于确认服务器支持的
    * 域
    * 请求方法
    * 自定义头

3. `Preflighted Requests` 是触发之后由浏览器自动发起的，我们无需额外的操作；

4. `Preflighted Requests` 使用 `OPTIONS` 方法的请求头
    ```
    Origin: http://ce.sysu.edu.cn
    Access-Control-Request-Method: POST
    Access-Control-Request-Headers: DITHeader
    ```

5. 支持 `Preflighted Requests` 请求的服务器返回的响应头
    ```
    Access-Control-Allow-Origin: *
    Access-Control-Allow-Methods: POST, GET
    Access-Control-Allow-Headers: DIYHeader
    Access-Control-Max-Age: 1728000
    ```

6. 对一个目标域只需要一次 `Preflighted Requests`，预请求的响应会按照响应头的 `Access-Control-Max-Age` 规定的时间来缓存。

## 带凭据的请求
1. 默认情况下，跨域请求不支持提供凭据（`cookie`、`HTTP` 认证以及客户端 `SSL` 证明）；

2. 通过将 `XHR` 对象的 `widthCredentials` 属性设置为 `true`，可以指定 `XHR` 对象发送请求时携带凭据；

3. 要使请求携带凭据的前提：**服务器接受带凭据的请求，并且响应头包含下列名值对**
    ```
    Access-Control-Allow-Credentials: true
    ```

4. 如果发送的是带凭据的请求，但服务器的响应中没有包含上面的头部信息，那么浏览器就不会把响应交给 `JavaScript`，这样 `responseText` 就为空，`status` 的值为 0，并且会触发 `XHR` 对象的 `onerror()` 事件；

5. 服务器可以在响应 `Preflighted Requests` 的响应头中发送上面这个头部，表示允许源发送带凭据的请求。

## 跨浏览器的 CORS
即在版本的 IE 中（IE 8-）中需要跨域时，使用 `XDR` 对象。

见红书：P<sub>583</sub>

  [1]: http://stackoverflow.com/questions/3102819/disable-same-origin-policy-in-chrome
