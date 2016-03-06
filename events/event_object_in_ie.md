# IE 中的事件对象
|属性/方法|类型|读/写|说明|
|---|---|---|---|
|cancelBubble|Boolean|读/写|表示是否（默认否）取消事件冒泡（类似 DOM 中的 `stopPropagation()` 方法）|
|returnValue|Boolean|读/写|设为 false 表示取消事件的默认行为（类似 DOM 中的 `preventDefault()`）|
|srcElemnt|Element|只读|事件的目标元素（类似 DOM 中的 target 属性）|
|type|String|只读|被触发的事件类型|


1. `IE` 中访问 event 对象的方式与现代浏览器 DOM 中访问 event 对象的方式不同；

2. IE 下的 DOM 0 级事件：event 对象作为 window 对象的一个属性存在，而不用将 event 作为处理函数的参数传入：
    ```javascript
    var btn = getElement('#myBtn');

    btn.onclick = function() {
        var event = window.event;
        alert(event.type); // 'click'
    }
    ```

3. HTML 特性指定事件处理程序时，在处理函数中直接通过一个 `event` 变量访问 `event` 对象；

4. `attachEvent()` 指定事件处理程序时，访问 event 对象有两种方法
    * `event` 对象做为 `window` 对象的属性被访问
    * `event` 对象作为参数传入事件处理函数

5. 低版本 IE 不支持事件捕获，只支持事件冒泡；使用 `cancelBubble = true` 可以阻止事件继续冒泡：
    ```javascript
    var btn = document.getElementById('myBtn');

    btn.onclick = function(event) {
        alert(''clicked!);
        window.event.cancelBubble = true;
    }

    document.body.onclick = function(event) {
        alert('body clicked');
    }

    // DOM 0 级事件是冒泡阶段触发，所以此例中 body 上的绑定事件无法由 btn 上的 click 冒泡触发
    ```
