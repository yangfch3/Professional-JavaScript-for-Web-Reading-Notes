# IE 事件处理程序
```javascript
var btn = document.getElementById('myDiv')；
var process = function () {
    alert(this);
};

btn.attachEvent('onclick', process);

btn.detachEvent('onclick', process);
```

1. IE 8 不支持 DOM 2 级事件；

2. **注意点**：`attachEvent()` `detachEvent()` 独特点
    * 接收的第一个参数为 `on + 'event'`，而非 DOM 2 级事件的 `'event'`；
    * 接收无需指定是否捕获阶段截获的布尔值参数，默认是冒泡阶段截获事件。

3. **牢记：`attachEvent()` 事件处理程序的事件处理函数的作用域是全局作用域，`this` 指向 `window`**，与 HTML 事件处理程序、DOM 0 级事件处理程序、DOM 2级事件处理程序有所出入；

4. `attachEvent()` 也可以为一个元素添加多个同名的事件处理程序；

5. `detachEvent()` 移除事件处理程序接收的参数必须与 `attachEvent()` 接收的参数一致，且事件处理函数的指向必须一致（匿名函数无法被移除）。
