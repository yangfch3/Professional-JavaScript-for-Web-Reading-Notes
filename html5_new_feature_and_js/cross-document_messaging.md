# 跨文档信息传递
1. `XDM` -- `Cross-document messaging`，来自不同域的页面间传递消息；

2. `XDM` 实现了不同域的两个框架间（或者与弹出窗口间）的通信机制的规范化，主要限制它使用的是浏览器的现代化程度；

3. 主要的两个概念
    * `postMessage()` 方法
    * `message` 事件

4. `postMessage()` 方法
    * **注意点**：调用这个方法的对象是 **目标窗口或框架**，会向调用这个方法的框架或窗口发送消息
    * 参数
        * 参数一：消息字符串
        * 参数二：指定消息接收方法（也就是调用这个方法的目标窗口或框架）的域名，**可以通配**，用于核对校验

    ```javascript
    var iframeWindow = document.getElementById('myFrame').contentWindow;

    iframeWindow.postMessage('Hello', 'http://www.example.com');
    ```

5. `message` 事件 -- 目标框架或窗口接收到 `XDM` 消息时，会触发其 `window` 对象上的 `message` 事件
    * 支持 DOM 0 级绑定与 DOM 2 级绑定
    * `event` 对象属性
        * `data`：消息字符串
        * `origin`：发送域
        * `source`：发送消息的文档的 `window` 对象代理（并非实际的 `window` 对象）

6. 务必在 `message` 事件处理程序上检测消息来源（`event.origin`）的正确性；

7. `event.source` 可以取得消息发送文档的 `window` 对象引用，但不能对于进行其他操作，只用于调用 `postMessage()` 发送回执
    ```javascript
    // 目标窗口或框架的程序
    eventUtil.addEventHandler(window, 'message', function(event) {
        // 确保发送消息的域是符合规则的域
        if (event.origin == 'http://ce.sysu.edu.cn') {
            // 调用预先准备好的数据处理函数
            processMessage(event.data);

            // 使用来源窗口代理发送回执
            event.source.postMessage('Yes, Received!', 'http://ce.sysu.edu.cn/hope');
        }
    })
    ```

8. 部分现代浏览器对于 `postMessage()` 方法第一个参数的数据格式支持情况存在差异
    * 一般只传入 **字符串数据**
    * 部分浏览器其他结构化的数据

9. `XDM` 对同域窗口或框架也是适用的

10. `XDM` 现在已经做为一个规范独立出来，这是以后框架间、窗口间通信的趋势
