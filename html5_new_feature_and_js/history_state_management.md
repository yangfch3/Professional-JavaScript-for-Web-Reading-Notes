# 历史状态管理
历史状态管理一直是现代 `Web` 应用中的难点，用户的每次操作不一定会打开一个新的页面（尤其是 `Ajax` 型单页面应用），导致用户很难在 **不同的状态间** 切换。

如何解决以上痛点？

* 状态管理 `API`，在 **不重载页面** 的情况下改变浏览器的 `URL`
    * 这里的不重载指的是：**不刷新、也不 `scrollIntoView`**
* `hashchange` 事件处理程序监听 `URL` 的 `hash` 值 的变化，执行处理程序
    * `hashchange` 的事件对象有两个额外属性：`oldURL` 和 `newURL`

## history.pushState()
1. 三个参数
    * 参数一：状态对象
    * 参数二：状态标题
    * 参数三（可选）：相对 `URL`（**可真可假**）

    ```javascript
    // e.g. 1
    history.pushState({init: 'Good Afternoon!'}, '', '#2');

    // e.g. 2
    var data = {
        name: 'yangfch3',
        age: 21,
        job: 'UI Dev'
    };
    history.pushState(data, '', 'yangfch3.html')
    ```

2. 执行 `pushState` 后
    * 新的状态信息被加入 **历史状态栈**
    * 浏览器地址（`location.href`）更新为相对地址
    * **浏览器不会刷新，也不会向服务器发送新的请求**
    * **不会触发 `hashchange` 事件**

3. 参数一：状态对象
    这是个很重要的参数，这个对象一般存储初始化页面状态所需的各种信息

4. 参数二：状态标题
    无太大用途，部分浏览器不支持

5. 参数三：相对 URL
    可以是服务器真实存在的资源地址，也可以完全是一个假的 URL
    > 避免使用假 URL，请确保使用 `pushState()` 创造的每一个 假  `URL`，在 `Web` 服务器上都有一个真的、实际存在的 `URL` 与之对应，否则用户刷新页面（假 `URL` 页面）时会出现 `404`

6. 用户按下“后退”按钮时，会触发 `window` 对象上的 `popstate` 事件，事件对象的 `state` 属性即是使用 `pushState()` 封存状态时传入的状态对象
    ```javascript
    eventUtil.addEventHandler(window, 'popstate', function(event) {
        var state = event.state;
        if (state) {
            processState(state);
        }
    });
    ```

7. 浏览器加载的第一个页面没有状态，因此单击后退按钮返回到了第一个页面时，`event.state` 为 `null`

8. 更新当前状态，使用 `history.replaceState()`，不会再历史状态栈中创建新状态，只会重写当前状态
