# DOM 0 级事件处理程序
```javascript
var btn = document.getElementById('myBtn');
btn.onclick = function () {
    alert(this.value);
}
```

1. DOM 0 级事件处理程序将之前 HTML 事件处理程序的处理权交还给了 `JavaScript`，现在为所有浏览器支持。

2. 每个元素（包括 `window` 和 `document`）都有自己的事件处理程序属性；

3. DOM 0 级方法指定的事件处理程序被认为是元素的方法；

4. DOM 0 级事件处理程序的 **作用域是元素本身**，**处理程序内的 `this` 指向元素**；

5. DOM 0 级事件处理程序会在事件流的 **冒泡阶段** 被处理；

6. 缺点：
    * 使用 DOM 0 级事件处理程序无法为元素绑定多个事件，后面的同名事件处理程序会覆盖前面的；
    * 定制化不强。

7. 删除 DOM 0 级事件处理程序的方法：`ele.onclick = null;`。
