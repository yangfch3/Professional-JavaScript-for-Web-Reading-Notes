# 复合事件
1. 复合事件（`composition event`）是 DOM 3 级事件中新增的；

2. 复合事件用于处理 `IME` 输入序列；

3. `IME`（`Input Method Editor`, 输入法编辑器） 可以让用户输入在物理键盘上找不到的字符；

4. 在使用 `IME` 打开时，输入时，关闭时都会触发相应的事件，一般为 **文本框** 等能够输入的元素绑定事件处理程序；
    * compositionstart
    * compositionupdate
    * compositionend

5. 缺少浏览器支持
