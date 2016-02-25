# 元素遍历
使用循环语句进行常规遍历的不方便性：
>元素间的空格（空行）的对待方式不一致，除 **IE 9-** 外的浏览器都视之为文本节点，所以未使用新 API 之前遍历元素都需要检测节点的类型，跳开空格（空行）代表文本节点。

`W3C` 的 `Element Traversal` 规范新定义了一组属性：**IE 9+** 支持

* `childElementCount`：子元素节点的个数
* `firstElementChild`：第一个子元素节点
* `lastElementChild`：最后一个子元素节点
* `previousElementSibling`：前一个元素节点
* `nextElementChildSibling`：后一个元素节点

旧时做法：
```javascript
var i,
    len,
    child = element.firstChild;

while (child != element.lastChild) {
    if (child.nodeType == 1) {
        processChild(child);
    }
    child = child.nexSibling
}
```

新 API 下的做法
```javascript
var i,
    len,
    child = element.firstElementChild;

while (child != element.lastElementChild) {
    processChild(child);
    child = child.nextElementSibling;
}
```
