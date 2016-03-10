# HTML5 事件
HTML5 规范定义了一系列新事件。

## 1. contextmenu 事件
1. 单击鼠标右键调出上下文菜单；

2. `contextmenu` 事件是冒泡的，可以为 `document` （当然也可以为某些具体的元素）指定一个事件处理程序以处理页面中所有的 `contextmenu` 事件；

3. `contextmenu` 事件的目标是发生用于右键点击（Mac 下是 Ctrl + 单击）的操作的元素；

4. 在浏览器端实现自定义上下文菜单：先阻止右键的默认事件（`preventDefault`），再显示自定义的上下文菜单

5. ![demo.html-6.3kB][1]

6. 实现自定义上下文菜单不止这些，还可以使用鼠标事件的 `mousedown` 事件 + 检测点击按钮值 实现

## beforeunload 事件
1. `beforeunload` 事件发生在 `window` 对象上，是为了让开发人员有可能在页面卸载前阻止这一操作；

2. 触发事件时浏览器会询问用户是否要关闭（或离开）页面还是希望继续留下来；

3. 设置对话框的内容
    * 方式一：将要显示给用户的字符串设置为 `event.returnValue` 的值
    * 方式二：`return` 要显示给用户的字符串

4. 实例
    ```javascript
    eventUtil.addEventHandler(window, 'beforeunload', function(event) {
        event = eventUtil.getEvent(event);
        var message = '卸载此页面可能造成您输入的信息丢失，请确认是否真的要离开此页面！';
        event.returnValue = message;
        // return message;
    })
    ```

## DOMContentLoaded 事件
1. `DOMContentLoaded` 事件在形成完整的 DOM 树之后就会在 `document` 对象上触发，并且会冒泡到 `window` 对象；

2. 可以为 `window`，`document` 添加这个事件处理程序；

3. 事件目标（`event.target`）是 `document`；

4. 始终在 `load` 事件之前触发；

5. 在不支持 `DOMContentLoaded` 事件的浏览器上可以使用一个超时调用来粗略模拟 `DOMContentLoaded` 事件。

## readystatechange 事件
1. 文档中的部分元素可以添加这个事件处理程序，但是一般只在 `document` 对象添加；

2. 用于提供与文档或元素的加载状态相关的信息；

3. 五个 `readystate` 值
    * `uninitialized`
    * `loading`
    * `loaded`
    * `interactive`
    * `complete`

4. 并非所有对象和文档都会经过这几个阶段，有些阶段会跳过；

5. 与 `load` 事件触发的顺序无法简单地确定，视外部资源的大小和数量；

6. `script` 和 `link` 元素也会触发 `readystatechange` 事件，可以利用这点来确定外部的 `JavaScript` 和 `CSS` 是否已经加载完成，对于外部资源，一般 `readystate` 为 `loaded` 或 `complete` 即表示资源可用；

7. 为外部资源元素添加 `readystatechange` 事件，可以 **解决资源加载的依赖先后问题**
    ```javascript
    eventUtil.addEventHandler(window, 'load', function() {
        var script = document.createElement('script');

        eventUtil.addEventHandler(script, 'readystatechange', function() {
            event = eventUtil.getEvent(event);
            var target = eventUtil.getTarget(event);

            if (target.readystate == 'loaded' || target.readystate == 'complete') {
                eventUtil.removeHandler(target, 'readystatechange', arguments.callee);
                alert("script loaded!");
            }
        })
    })
    ```

8. 文档内用得比较少，一般用在 `ajax` 对象上。

## pageshow 和 pagehide 事件
1. 往返缓存 -- `back-forward cache` 用于在用户使用浏览器的“前进”和“后退”按钮时加快页面的转换速度；

2. 这个缓存中封存着页面数据、`DOM` 和 `JavaScript` 状态，即整个页面都保存在内存中；

3. 现代浏览器都能自动封存能够前进、后退的页面状态，自动将页面加入 `bf-cache`；

4. 从 `bf-cache` 中调取的页面不会触发 `load` 事件；

5. `pageshow` 事件触发时机
    * 载入页面时，`pageshow` 事件再 `load` 事件触发后触发
    * 从 `bf-cache` 中重载页面时，在页面完全恢复的时刻触发

6. `pageshow` 事件处理程序一般添加到 `window` 对象上；

7. `pageshow` 事件的 `event` 对象有一个 `persisted` 属性（布尔值），用于指示页面是否存在于 `bf-cache` 内；

8. `pagehide` 事件触发时机：在 `unload` 事件触发之前触发；

9. 指定了 `unload` 事件处理程序的页面会被自动排除在 `bf-cache` 之外，即使事件处理函数是空的。

## hashchange 事件
1. 出现这个新事件的主要推动力是：在 `Ajax` 应用中，常常需要使用 `URL` 参数（`hash`）列表来保存状态或导航信息；

2. `hashchange` 事件处理函数需要添加给 `window` 对象，一旦 `URL` 参数（`hash`）列表变化就会触发；

3. `hashchange` 事件的 `event` 对象有两个属性（部分浏览器支持）：
    * oldURL：变化之前的完整 URL
    * newURL：变化之后的完整 URL

4. `hashchange` 事件 IE 8+ 支持；



  [1]: http://static.zybuluo.com/yangfch3/tjg794uc3qaeh359xboxeeib/demo.html
