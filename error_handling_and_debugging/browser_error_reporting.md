# 一、浏览器报告的错误
1. （低版本）`IE` 是唯一一个在浏览器的界面窗体中显示 `JavaScript` 错误信息的浏览器；

2. 低版本 IE （<=7）不自带脚本调试器，需要另行安装；

3. 低版本 IE（<=7）中，如果错误位于外部文件的脚本中，行号通常会与错误所在的行号差 1，嵌入脚本没有这样的偏差。

## 常见 （低版本） IE 错误
### 操作终止（IE 7-）
* 出现情景：页面尚未加载完成，`JavaScript` 即对页面内容进行修改；
* 错误反应：弹出模态对话窗，单击 OK 后，卸载整个页面，屏幕空白；
* 避免方法：等到页面加载完毕后再进行操作

### 无效字符
出现情景：`JavaScript` 文件中存在无效字符 -- `invalid character`

`FF` 下报告 `illegal character` 错误，`Safari` 报告发生了语法错误，`Opera` 报告 `ReferenceError`。

### 未找到成员
**低版本 IE 所有 DOM 对象都是以 COM 对象，而非原生 `JavaScript` 实现**，由此导致的**垃圾收集**例程配合错误会引起 `Member not found` 错误。

对象在销毁之后，又给该对象赋值。

```javascript
document.onclick = function() {
    var event = window.event;
    setTimeout(function() {
        event.returnValue = false;
        alert(event.returnValue);
    }, 1000);
};
```
低版本 IE 在触发点击事件时，创建 `event` 对象，代码执行完毕，`event` 对象被销毁；在闭包中再次调用这个 `event` 对象时便会导致出现 **未找到成员** 错误。

### 未知运行时错误
1. 把块元素插入行内元素时，低版本 IE 会出现 **未知运行时** 错误；

2. 访问表格任意部分（`<table>` `<tbody>`）的任意属性时。

### 语法错误
1. 代码少了分号；

2. 花括号前后不对应；

3. `<script>` 标签 `src` 指向的资源的 `MIME` 类型不是可用的 `JavaScript`

### 系统无法找到指定资源
1. `JavaScript` 代码中请求的 URL 长度超过了长度限制：低版本 IE 限制是 2083 个字符；

2. IE 对页面本身的 URL 长度也有限制：2048 个字符。
