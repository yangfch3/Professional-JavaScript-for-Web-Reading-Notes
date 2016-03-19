# 其他跨域技术
在 `CORS` 出现之前，要实现跨域的 `Ajax` 需要颇费周折。

聪明的劳动人民想到了一些其他的实现跨域请求的方法：**利用 `DOM` 能执行跨域请求的功能，在不依赖 `XHR` 对象的情况下也能发送某种请求**。

使用 `CORS` 需要浏览器端做出相应的配置，所以这些开发人员发明的方法一直延续并被广泛使用着。

在不依赖 `XHR` 对象和其他新技术（`SSE`、`WS`）的情况下一般使用 `DOM` 中的 `img` 和 `script` 元素进行跨域资源的请求。

## 图像 ping
1. `JavaScript` 中的 `Image` 构造函数用于新建 `Image` 对象实例（`img` 元素）；

2. `Image` 实例的链接不需要为真实的图片资源 `URL`（如果不是当做图片元素使用的话），可以为服务器上的任意 `URL`（指向服务器上的处理程序资源）；

3. `Image` 实例（`img` 元素）的 `onload` `onerror` 事件；

4. 需要传给服务器的数据则通过 `URL` 的查询字符串来保存传输；

5. 图像 Ping 一般只用于 **浏览器向服务器的单向通信**：
    > 1. 只能发送 GET 请求
    > 2. 无法访问服务器的响应文本（因为你用的是 `Image` 对象）

6. 图像 Ping 最常用于跟踪用户点击页面或动态广告的曝光次数。

```javascript
var img = new Image();
img.onload = img.onerror = function() {
    alert('Done!');
};

img.src = 'http://ce.sysu.edu.cn/processExec?name=yangfch3&age=21';
```

## JSONP
1. `JSONP`，全称 `JSON with Padding`，填充式 `JSON` 或叫做参数式 `JSON`；

2. JSONP 由两部分组成：回调函数和数据，回调函数需要预先创建定义；

3. 使用动态 `script` 元素；

4. 因为 `script` 能不受限制地从其他域加载资源；

5. `URL` 特征：查询字符串指定回调函数名
    ```
    http://www.example.com/json?callback=foo
    ```

6. `JSONP` 也需要服务器端支持；

7. `JSONP` 支持服务器端、客户端双向通信；

8. 使用不是自己运维的服务器时，一定得保证它安全可靠；

9. 要确认 `JSONP` 请求成功与否并不容易。
