# 进度事件
* `loadstart`：接收到响应数据第一个字节时触发
* `progress`：接收响应期间持续不断地触发
* `error`：请求发生错误时触发
* `abort`：调用 `abort()` 方法时触发
* `load`：接收到完整的响应数据（包括失败的响应）时触发
* `loadend`：通信完成（不管响应式成功或失败）后触发 -- 大部分浏览器不支持

## load 事件
1. 引入 `load` 事件的目的是替代 `onreadystatechange` 事件，让开发人员无需去检测 `readystate` 变化；

2. `onload` 事件处理程序接收一个 `event` 对象，`target` 属性指向 `XHR` 对象实例（但部分浏览器不支持 `traget` 属性）；

3. **局限性**：只要浏览器接收到了服务器的响应，不管状态如何，都会触发 `load` 事件；因此我们仍需要检测 `status` 属性才能确定数据是否真的可用。

## progress 事件
1. `progress` 事件会在浏览器接收新数据期间周期性地触发；

2. `progress` 事件处理程序会接收到一个 `event` 对象，`target` 属性指向 `XHR` 对象，三个额外的属性
    * `lengthComputable`：进度信息是否可用布尔值
    * `loaded`：已接收的字节数
    * `total`：`Content-Length` 响应头确定的预期字节数
    > 旧版本浏览器的后两个属性是：position 和 totalSize

3. 可以使用 `progress` 事件创建一个 `Ajax` 请求进度指示器。
