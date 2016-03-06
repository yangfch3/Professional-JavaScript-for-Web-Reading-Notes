# 焦点事件
1. 常用焦点事件：
    * `blur`：元素失去焦点时触发，**注意：该事件不会冒泡**
    * `focus`：元素获得焦点时触发，**注意：该事件不会冒泡**
<br>

2. 不常用焦点事件：
    * `focusin`：冒泡版 `focus` 事件
    * `focusout`：`blur` 事件通用版
    * `DOMFocusIn`：已废弃版 `focusin`
    * `DOMFocusOut`：已废弃版 `focusout`
<br>

3. 常被制定焦点事件的元素：**input(文本框)、链接、body**；

4. 焦点事件可以使用 `JavaScript` 元素的 `focus()` `blur()` 触发
    ```javascript
    document.body.focus
    ```
