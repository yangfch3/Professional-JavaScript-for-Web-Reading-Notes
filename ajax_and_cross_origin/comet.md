# Comet
1. Comet 英文翻译叫
    * 彗星
    * 科梅（人名）

2. `Comet` 技术是一种 **服务器推送** 技术，是一种更高级的 `Ajax` 技术；

3. `Comet` 实现的两种方式：
    * **长轮询**（客户端发起）
    * **流**

4. 短轮询
    ![PubSub_poll.jpg-46.9kB][1]

5. **长轮询**
    ![819496-20151114110929165-1485612934.jpg-61.7kB][2]
    > 页面发起一个到服务器的请求，服务器一直保持连接打开，直到有数据可返还给客户端；客户端接收到数据，关闭连接，随即又发起一个到服务器的请求，如此往复。

6. **长轮询** 和 **短轮询** 的区别：在于服务器如何发送数据
    > 短轮询：服务器立即发送响应，无论数据是否有效
    > 长轮询：保持连接，等待有可用资源时，才返回响应

7. 轮询的优势是
    * 所有浏览器都支持
    * 实现简单：只需要使用 XHR 对象和定时器

8. 另一种流行的 `Comet` 实现是：**`HTTP` 流**（简单点理解就是马桶和便秘地关系\_(:зゝ∠)_）

9. **“流”的特点**
    * 在页面的整个生命周期只使用一个 `HTTP` 连接
    * 服务器一直保持连接打开，周期性地想浏览器发送数据

10. 服务器端需要写好相应地实现 HTTP 流的脚本或程序；

11. 利用一个 `XHR` 对象即可实现 `HTTP` 流
    * 随着不断的从服务器端接收数据，`readyState` 的值会周期性地变为 3；
    * 当 `readyState` 值变为 3 时，`responseText` 属性中就会保存着接收到的所有数据；
    * 比较此前接触到的数据，处理，使用。

12. **缺陷**：管理 `Comet` 连接是十分容易出错的（于是为了简化 `Comet` 的实现，又为 `Comet` 技术创建了两个新的接口）。

```javascript
reyfunction createStreamingClient(udl, progress, finished) {
    var xhr = new XMLHttpRequest(),
        received = 0;

    xhr.open('GET', url, true);
    xhr.onreadystatechange = function() {
        var result;
        if (xhr.readyState === 3) {
            // 只取得最新数据
            result = xhr.responseText.substring(received);
            received += result.length;

            // 调用回调函数来处理数据
            progress(result);
        } else if (xhr.readyState === 4) {
            finished(xhr.responseText);
        }
    };

    xhr.send(null);
    return xhr;
}

var progress = function(data) {
    alert('Received: ' + data)
};

var finished = function() {
    alert('Done!');
};

var client = createStreamingClient('streaming.php', progress, finished);
```

## 服务器发送事件（SSE）
1. `SSE` -- `Server-Sent Events`，服务器发送事件，是围绕只读 `Comet` 交互推出的新 `API`；

2. `SSE API` 用于创建到服务器的单向连接；

3. `SSE` 的特点
    * 服务器可以通过这个连接发送任意数量的数据；
    * 服务器响应的 `MIME` 类型必须是 `text/event-stream`，响应的数据必须是 `JavaScript` 引擎能解析输出的；
    * `SSE` 支持长轮询、短轮询和 HTTP 流；
    * 能在断开时自动确定何时重新连接

4. `SEE API` 使用方法：
    1. 实例化一个 `EventSource` 实例
        ```javascript
        var source = new EventSource('myEvents.php');

        ```
        * 接受一个 `URL` 参数，必须与创建对象的页面同源
    2. `EventSource` 实例的属性：`readyState`
        * 0：表示正在连接到服务器
        * 1：表示打开了连接
        * 2：表示关闭了连接

    3. `EventSource` 实例的事件
        * `open`
        * `message`
            * 触发 `message` 事件时会创建 `event` 对象，`event.data` 内保存着服务器返回的数据。
        * `error`

### 事件流
1. 服务器事件会通过一个持久的 `HTTP` 响应发送；

2. 响应的 `MIME` 类型为 `text/event-stream`；

3. 响应的格式是：纯文本（可能每个数据项都带有 `data:` 前缀）
    ```
    // 1
    data: foo
    id: 1

    // 2
    data: bar
    id: 2

    // 3
    data: foo
    data: bar
    id: 3

    // 4
    data: foo
    id: 4
    ```
    * 第三个 `message` 事件返回的 `event.data` 是 `foo\nbar`；
    * 每个返回值之间间隔一行
    * 一个多行的值返回方式见上 3

4. 通过 `id:` 前缀可以为返回值指定一个关联的 ID；

5. `EventSource` 对象会跟踪上一次触发的时间，`ID` 在连接断开，重新建立 `SSE` 连接时发挥作用
    > 如果连接断开，会向服务器发送一个包含名为 Last-Event-ID 的特殊 `HTTP` 头部，以便服务器知道下一次要触发 `id:` 为多少的事件

## Web Sockets
1. WS 的目标是在一个 **单独的**、**持久的** 连接上提供 **全双工**、**双向** 通道；

2. 在 JS 中创建了 WS 后，会有一个 `HTTP` 请求发送到服务器以建立连接，取得服务器响应后，建立的连接会使用 `HTTP` 升级：**从 `HTTP` 协议交换为 `Web Socket` 协议**；

3. WS 使用了自定义的协议，URL 模式与我们熟悉的 `HTTP` URL 不尽相同
    * 未加密连接：`ws://...`
    * 加密连接：`wss://...`

4. 使用新的协议的好处
    * 可以发送非常少量的数据，没有 `HTTP` 那样的**字节级开销**；
    * 传递的数据包很小，非常适合移动应用
    * 根本**没有同源的限制**，**唯一的需要是：服务器支持 WS 链接与协议，允许来源域的链接**

### Web Sockets 使用方法
1. 实例化一个 `WebSocket` 对象：传入 `ws` 协议格式的绝对 `URL`
    ```javascript
    var socket = new WebSocket('ws://www.example.com/server.php');
    ```

2. WS 对象属性：`readyState`
    * 0：WebSocket.OPENING -- 正在建立连接
    * 1：WebSocket.OPEN -- 已经建立连接
    * 2：WebSocket.CLOSING -- 正在关闭连接
    * 3：WebSocket。CLOSE -- 已经关闭连接

3. 关闭 ws 连接调用 `close()` 方法
    ```javascript
    socket.close();
    ```

4. 使用 `send()` 方法向服务器发送任意字符串数据
    ```javascript
    socket.send('Hi!');
    ```
    > 只能发送纯文本数据，复杂的数据结构在发送之前需要先进行序列化处理

5. 服务器向客户端发来消息时，`WebSocket` 对象会触发 `message` 事件，返回的数据保存在 `event` 对象的 `event.data` 属性中；
    ```javascript
    socket.onmessage = function() {
        var data = event.data;

        // 处理数据
    }
    ```

6. 其他事件：
    * `open`
    * `error`
    * `close`

7. WebSocket 对象不支持 DOM 2 级事件侦听器，请使用 DOM 0 级语法定义事件处理程序；

8. 以上三个事件中只有 `close()` 事件的 `event` 对象有以下扩展属性
    * `wasClean`：是否已经明确地关闭，布尔值
    * `code`：服务器返回的数值状态码，数值
    * `reason`：服务器返回的关闭原因，字符串
    ```javascript
    // 连接关闭时将信息展示给用户或记录到服务器端作为分析日志
    socket.onclose = function(event) {
        var img = new Image();
        img.src = 'http://www.example.com/log.php?' + 'wasClean=' + event.wasClean + '&Code=' + event.code + '&Reason=' + event.reason;

        alert('Was clean: ' + event.wasClean + ' Code: ' + event.code + ' Reason: ' + event.reason);
    }
    ```

## 使用 SSE 还是 WS
1. 是否有自由度建立和维护 Web Socket 服务器
    * `Web Socket` 协议不同于 `HTTP`，现有浏览器在没有经过特殊配置不能用于 `Web Socket` 通信
    * `SSE` 使用的是常规的 `HTTP` 流，现有服务器便可满足需求

2. 到底是否需要双向通信
    * 如果只需要读取服务器端数据，而不需要频繁地发送，`SSE` 比较容易实现
    * 如果必须双向频繁地通信，那么使用 Web Socket 显然更好

> 可以使用 XHR 与 SEE 搭配也可以实现双向通信，但是比 Web Socket 实现起来复杂



  [1]: http://static.zybuluo.com/yangfch3/ip9iqna6jmalua9fs5qmtl6n/PubSub_poll.jpg
  [2]: http://static.zybuluo.com/yangfch3/6s2k25rqhfewrix9e244re8j/819496-20151114110929165-1485612934.jpg
