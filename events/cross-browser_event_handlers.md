# 跨浏览器的事件处理程序
1. 基本原则：使用能力检测；

2. 为保证处理时间的代码能在大多数浏览器下运行，只关注冒泡阶段是很好的选择；

3. 结合 DOM 0 级方法、DOM 2 级方法或 IE 方法来添加事件，使用能力检测向后兼容
    ```javascript
    var EventUtil = {
        addEventHandler: function(element, type, handler) {
            if (element.addEventListener) {
                element.addEventListener(type, handler, false);
            } else if (element.attachEvent) {
                element.attachEvent('on' + type, handler);
            } else {
                element['on' + type] = handler;
            }
        },

        removeEventHandler: function(element, type, handler) {
            if (element.removeEventListener) {
                element.removeEventListener(type, handler, false);
            } else if (element.detachEvent) {
                element.detachEvent('on' + type, handler);
            } else {
                element['on' + type] = null;
            }
        }
    }
    ```

4. 以上代码的局限性：没有考虑各个事件处理程序的作用域问题
    * `HTML` 事件处理程序关联作用域：`document`、(`this.form`)、`this element`
    * DOM 0 级事件处理程序关联作用域：`this element`
    * DOM 2 级事件处理程序关联作用与：`this element`
    * IE 事件处理程序关联作用域：`window`
