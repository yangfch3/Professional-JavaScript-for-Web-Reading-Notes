# DOM 中的事件对象

|属性/方法|类型|读写|说明|
|---|---|---|---|
|bubbles|Boolean|只读|表明事件是否支持冒泡|
|cancelable|Boolean|只读|表明是否可以取消默认行为|
|currentTarget|Elememt|只读|表明事件处理程序当前正在处理事件的那个元素|
|defaultPrevented|Boolean|只读|为true表示已经调用了下面的 `preventDefault()`|
|detail|Int|只读|与事件相关的细节信息|
|eventPhase|Int|只读|触发事件处理程序的阶段：1--捕获，2--处于目标，3--冒泡|
|preventDefault()|Funtion|/|取消事件的默认行为，前提条件是 cancelable 属性为 true|
|stopImmediatePropagation()|Function|/|取消事件的进一步捕获或冒泡，**同时阻止任何（其他的）事件处理程序被调用**|
|stopPropagation()|Function|/|立即停止事件在 DOM 中的传播，取消进一步的事件捕获或冒泡，前提条件是：bubbles 属性为 true|
|target|Element|只读|事件的目标元素|
|trusted|Boolean|只读|为 true 表示是浏览器生成的，为 false 表示是开发人员自定义的事件|
|type|String|只读|被触发的事件类型|
|view|AbstractView|只读|与事件相关的抽象视图，等于发生事件的 window 对象|


1. 在 HTML 事件处理程序时：变量 `event`（固定）中保存着 `event` 对象，无需主动声明，也无需已参数形式传入；
    ```HTML
    <input type="button" value="click me" onclick="alert(event.type)">
    > click
    ```

2. 在 DOM 0 级和 DOM 2 级事件处理程序中，`event` 对象会 **以参数的形式传入事件处理函数** 中；

3. `event` 对象的基本属性和方法见，各个不同类型的事件扩展的属性和方法各不相同；

4. 在事件处理程序内部，对象 `this` 的值始终等于 `currentTarget` 的值；
    > `target` 属性是（同心圆）靶心，`this` 和 `currentTarget` 是你击中的最大点数环（同心圆）

5. `target` 是事件在 DOM 同心圆蔓延的终点；

6. `stopPropagation()` 用法实例：
    ```javascript
    var btn = document.getElementById('myBtn');

    btn.onclick = function(event) {
        alert(''clicked!);
        event.stopPropagation();
    }

    document.body.onclick = function(event) {
        alert('body clicked');
    }

    // DOM 0 级事件是冒泡阶段触发，所以此例中 body 上的绑定事件无法由 btn 上的 click 冒泡触发
    ```

7. 只有在 `eventPhase` 为 2 （目标阶段时）this=cureentTarget 和 traget 才相等；

8. 只有在事件处理程序执行期间，event 对象才存在，一旦事件处理程序执行完毕，event 对象就会被销毁。
