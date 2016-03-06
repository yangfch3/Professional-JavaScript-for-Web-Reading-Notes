# UI 事件
* `load`
当页面、框架、图片、嵌入资源分别在 **window、frames 框架、`img` 元素、`object`** 元素上加载完毕时触发

* `unload`
页面、框架、嵌入资源 完全卸载后在 **window、frames 框架、`object` 元素** 上面触发

* `abort`
用户停止下载，而嵌入的内容仍没有加载完时在 **`object` 元素** 上触发

* `error`
发生 JavaScript 错误时在 **window 对象** 上触发，无法加载图片、无法加载资源、无法加载某些框架时分别在 **img 元素、object 元素、框架集** 上触发

* `select`
用户选择文本输入框（`input type="text"` `textarea`）中的一个或多个字符时在 **`input` 元素**、**`texterea` 元素** 上触发

* `resize`
窗口或框架大小变化时在 **window 对象** 或 **框架** 上面触发

* `scroll`
用户滚动带滚动条的元素中的内容时，在该 **元素** 上触发，滚动网页主体时在 **`body` 元素** 上触发

---
需要注意的点：

* 以上事件触发的对象需要注意的地方
    1. `select` 事件触发的元素时表单域的 **输入元素**；
    2. 大部分时间都能在 **`window` 对象** 上触发。

---
检测浏览器是否支持 **DOM 2 级事件**、**DOM 3 级事件** 规定的以上 HTML、UI 事件：
```javascript
var isSupported = document.implementation.hasFeature('HTMLEvents', '2.0');

```
```javascript
var isSupported = document.implementation.hasFeature('UIEvents', '3.0');

```

---
## 1. load 事件
1. 当页面完全包括图像、JS 文件、CSS 文件等 **外部资源完全加载**，就会在 **`window` 对象** 上触发 `load` 事件；
    > 所以遇到资源请求多、大的页面，不做特殊处理的话，`load` 事件发生会比较迟滞

2. 也可以为 `body` 元素添加 `onload` 特性；
    > 不推荐，这只是浏览器为了向后兼容采用的措施

3. 现代浏览器中：`load` 事件的 `target` 是 `document`；

4. 一般来说，在 `window` 上面发生的任何事件都能在 `body` 元素中通过相应的特性来指定；

5. 规范指定：应该在 `document` 上触发 `load` 事件，而不是在 `window` 对象上，但是，**所有浏览器都在 `window` 上面实现 `load` 事件**；

6. 也可以为 `img` 元素指定 `onload` 事件处理程序，此时 `target` 为 `img` 元素本身：
    ```javascript
    var image = getElement('#myImg');

    eventUtil.addEventHandler(image, 'load', function(event) {
        event = eventUtil.getEvent(event);
        target = eventUtil.getTarget(event);

        alert(target.src);
    })
    ```

7. 创建新的 `img` 元素并为其指定 `load` 事件处理程序：**在指定 `src` 属性之前先指定事件处理程序**
    ```javascript
    eventUtil.addEventHandler(window, 'load', function() {
        var image = document.createElement('img');

        eventUtil.addEventHandler(image, 'load', function() {
            var event = eventUtil.getEvent(event);
            var target = eventUtil.getTarget(event);
            alert(target.src);
        });

        document.body.appendChild(image);
        image.src = '...'
    })
    ```

8. 通过 DOM 操作新建的图像元素的 `src` 新图像资源， 一经设置马上下载；

9. DOM 0 级的 `Image` 对象也可以用于新建 `img` 元素（在错误处理一章，我们用到了使用 `image` 对象的 `src` 请求服务器端的错误记录程序，以实现客户端错误服务器端记录的效果）
    ```javascript
    var image = new Image();
    image.src = '...';
    ```

10. **兼容**：在不属于 DOM 文档的图像上触发 `load` 事件时，IE 8 及以下版本不会生成 `event` 对象；

11. 大部分现代浏览器（IE 9+）还支持 `script` 元素的 `load` 事件；但是动态加载 `script` 与 `img` 元素有所不同
    > 使用 DOM 生成 `img` 元素，一经设置 `src` 马上下载图片资源；使用 DOM 方法动态创建 `script` 元素，设置了 `src` 后还需要将 script 元素插入文档脚本才会开始下载；

12. 部分浏览器还支持 `link` 元素上的 `load` 事件，但各个浏览器对待这个问题差异很大。

---
## 2. unload、beforeunload 事件
1. 发生时机：用户关闭窗口、跳转离开或卸载框架时在 `window` 或 **框架对象** 上触发；

2. 指定 `unload` 和 `beforeunload` 事件
    ```javascript
    eventUtil.addEventHandler(window, 'unload', function(event) {
        return '此页面的数据会在关闭或重载后丢失，确定离开或重载页面？';
    })

    eventUtil.addEventHandler(window, 'beforeunload', function(event) {
        return '此页面的数据会在关闭或重载后丢失，确定离开或重载页面？';
    })
    ```

3. 此事件一般用于一些关闭或重载后数据会丢失的页面，经测试，需要有 `return` 值才能使浏览器触发离开或重载前的询问对话框。

---
## resize 事件
1. 触发条件：浏览器窗口被调整到一个新的高度或宽度；
触发对象：`window`

2. 指定 `resize` 事件的两种形式：
    * JavaScript 指定：`addEventHandler ---> window`
    * HTML 属性指定：`body onresize=""`

3. 传入事件处理程序的 `event` 对象的唯一属性：`target`，指向 `document`；
    > IE 8 及之前版本则未对 `resize` 的 `event` 提供任何属性

4. 各个浏览器处理 `resize` 事件的方式不尽相同
    * 大部分浏览器只要窗口一旦变化就会立即触发；
    * FF 则会等到调整停止（稳定）下来才会触发。

---
## scroll 事件
1. `scroll` 事件发生在 `window` 对象；

2. 可以使用 `body` 元素的 `scrollLeft` 和 `scrollTop` 来监控这一变化；

3. scroll 事件会在页面滚动时被重复触发，所以尽可能保持事件处理程序的代码简洁。
