# 变动事件
DOM 2 级的变动事件能在 DOM 的某一部分发生变化时触发。

## 1. 删除节点
`DOMNodeRemoved` `DOMNodeRemoveFormDocument` `DOMSubtreeModified`

1. 触发条件：使用 `removeChild()` 或 `replaceChild()` 删除节点时；

2. 首先触发 `DOMNodeRemove` 事件
    * 事件目标（`event.target`）：被删除的节点
    * 关联节点（`event.relatedNode`）：被删除节点的父节点

3. 然后在被删除的节点及其子节点上（如果包含）上触发 `DOMNodeRemovedFormDocument` 事件
    * 事件目标（`event.target`）：被删除的节点或子节点

4. 紧随其后在被删除节点的父节点上触发的是 `DOMSubtreeModified` 事件

```javascript
// HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Node Removal Events Example</title>
</head>
<body>
    <ul id="myList">
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
    </ul>
</body>
</html>

// JavaScript
eventUtil.addEventHandler(window, 'load', function(event) {
    var myList = getElement('#myList');

    eventUtil.addEventHandler(document, 'DOMSubtreeModified', function(event) {
        alert(event.type);
        alert(event.target);
    });

    eventUtil.addEventHandler(document, 'DOMNodeRemoved', function() {
        alert(event.type);
        alert(event.target);
        alert(event.relatedNode);
    });

    eventUtil.addEventHandler(list.firstChid, 'DOMNodeRemovedFromDocument', function(event) {
        alert(event.type);
        alert(event.target);
    });

    list.parentNode.removeChild(list);
})
```

## 2. 插入节点
`DOMNodeInserted` `DOMNodeInsertedIntoDocument` `DOMSubtreeModified`

1. 触发条件：使用 `appendChild()` `replaceChild()` `insertBefore()` 向 DOM 中插入节点时；

2. 首先触发 `DOMNodeInserted` 事件
    * 事件目标（`event.target`）：被插入的节点
    * 关联节点（`event.relatedNode`）：被插入节点的父节点

3. 然后在新插入的节点上触发 `DOMNodeInsertedIntoDocument` 事件
    * 事件目标（`event.target`）：被插入的节点

4. 最后在新插入节点的父节点上触发 `DOMSubtreeModified` 事件

```javascript
// HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Node Removal Events Example</title>
</head>
<body>
    <ul id="myList">
        <li>Item 1</li>
        <li>Item 2</li>
        <li>Item 3</li>
    </ul>
</body>
</html>

// JavaScript
eventUtil.addEventHandler(window, 'load', function(event) {
    var myList = getElement('#myList');
    var item = document.createElement('li');
    item.appendChild(document.createTextNode("Item 4"));

    eventUtil.addEventHandler(document, 'DOMSubtreeModified', function(event) {
        alert(event.type);
        alert(event.target);
    });

    eventUtil.addEventHandler(document, 'DOMNodeInserted', function() {
        alert(event.type);
        alert(event.target);
        alert(event.relatedNode);
    });

    eventUtil.addEventHandler(item, 'DOMNodeInsertedIntoDocument', function(event) {
        alert(event.type);
        alert(event.target);
    });

    myList.appendChild(list);
})
```
