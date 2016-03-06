# DOM 2 级事件处理程序
```javascript
var btn = document.getElementById('myBtn');
var process = function () {
    alert(this.value);
}
btn.addEventListener('click', process, false)；

btn.removeEventListener('click', process, false);
```

1. 两个方法：`addEventListener()` `removeEventListener`；

2. `window`、`document` 和所有 DOM 节点都有这两个属性（方法）；

3. 三个参数：事件名、事件处理函数、是否捕获阶段即调用布尔值；

4. **最佳实践**：无特殊要求，第三个参数始终设为 `false`（见 11）；

5. **最佳实践**：事件处理函数最好预先定义，避免使用匿名函数（见 10）；

6. **事件处理程序在其依附的元素的作用域中运行，事件处理函数中的 `this` 指向元素本身**；

7. **主要优点**：可以为一个元素添加多个同名的事件处理程序；

8. 通过 `addEventListener()` 添加的事件处理程序只能使用 `removeEventListener()` 来移除；

9. `removeEventListener()` 移除事件处理程序时传入的参数必须与添加事件处理程序时传入的参数相同，且第二个参数：**事件处理函数** 的指向必须相同，否则无法移除事件处理程序；

10. 将第三个参数设置为 `false` 是为了最大限度地兼容各个浏览器，**除非需要在事件到达目标之前截获它的，才将第三个参数设置为 `true`**；

11. IE 9 及以上支持。
