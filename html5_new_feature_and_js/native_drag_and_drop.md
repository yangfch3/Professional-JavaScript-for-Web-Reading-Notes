# 原生拖放
1. 早期的能拖放只有两种对象：图像和某些文本，有效的放置目标是文本框；

2. HTML 5 制定了拖放的规范，**现在的拖放对象已经拓展到了任意对象，放置目标也扩展到了任意对象，甚至是跨框架、跨窗口**。

---
## 拖放事件
`dragstart` `drag` `dragend`

1. 按下鼠标键并开始移动鼠标时，会在被拖放的元素上触发 `dragstart` 事件，通过 `dragstart` 事件处理程序来运行 `JavaScript` 代码；

2. 在 `dragstart` 之后，随即会触发 `drag` 事件，并在拖放的过程中会持续触发；

3. 释放鼠标，触发 `dragend` 事件；

4. 以上三个事件的目标都是被拖动的元素；

5. 当一个元素被拖放到一个有效的放置目标上时，下列事件会依次发生
    * `dragenter`
    * `dragover`
    * `dragleave` 或 `drop`

6. `dragover` 会在鼠标移动时被连续不断的触发；

7. 5 中的三个事件的目标都是 **作为放置目标的元素**。

## 自定义放置目标
1. 有些元素默认是不允许作为拖放的放置目标的（圆环中有一条反斜线），无法进行 `drop`；

2. 人为的将一个元素变为可放置目标：即 **阻止不允许拖放的默认事件**
    ```javascript
    var droptarget = document.getElementById('droptarget');

    eventUtil.addEventLisrener(droptarget, 'dragover', function(event) {
        event = eventUtil.getEvent(event);
        // 阻止默认的禁止
        eventUtil.preventDefault(event);
    })

    eventUtil.addEventLisrener(droptarget, 'dragenter', function(event) {
        event = eventUtil.getEvent(event);
        // 阻止默认的禁止
        eventUtil.preventDefault(event);
    })
    ```

3. 部分浏览器下放置事件的默认行为是：**打开被放置到目标上的 URL，或者转向图片链接资源**

## dataTransfer 对象
1. `dataTransfer` 对象是拖放事件对象的一个属性，只能在拖放事件触发的处理程序中访问 `dataTransfer` 对象；

2. 用于 从 **被拖放元素** 向 **目标** 传递字符串格式的数据；

3. `dataTransfer` 对象属性和方法
    * `getData()`
        * 参数：数据类型
    * `setData()`
        * 参数一：数据类型
        * 参数二：数据值

4. 关于上面两个数据类型参数
    > IE 只定义了 `text` 和 `URL` 两种有效的数据类型
    > HTML 5 规范指出：允许指定各种 `MIME` 类型，也支持向后兼容的 `text` 和 `URL`（本质是被自动映射为 `text/plain` `text/uri-list`）

5. 拖动文本时，触发 `dragstart` 事件，浏览器调用 `setData()` 方法，将拖动的文本、URL（图片 URL）保存到 `dataTransfer` 对象（当然，你也是可以手工调用 `setData()` 来自定数据）；
然后当元素被拖放到目标时，就可以通过 `getData()` 读取这些数据

```javascript
var dataTransfer = event.dataTransfer;

// 兼容不认 'url' 的浏览器
var url = dataTransfer.getData('url') || dataTransfer.getData('text/uri-list');
// 读取文本
var text = dataTransfer.getData('Text');
```

## dropEffect 和 effectAllowed
1. 利用 `dataTransfer` 对象还能确定 **被拖放元素** 以及 **作为放置目标的元素** 能够接受什么操作；

2. `dropEffect` 属性可以知道 **被拖动的元素** 能够执行哪种放置行为；
    `none` `move` `copy` `link`

3. 在 `ondragenter` 事件处理程序中针对放置目标来设置 `dropEffect` 属性；

4. `dropEffect` 属性需要搭配 `effectAllowed` 属性才有用，`effectAllowed` 属性表示允许拖动元素的哪种 `dropEffect`；
    `uninitialized` `none` `copy` `link` `move` `copylink` `copyMove` `linkMove`

5. 在 `ondragstart` 事件处理程序中设置 `effectAllowed` 属性

## 可拖动
1. HTML 5 为所有元素规定一个新特性：`draggable`，表示元素是否可以被拖动；

2. 图像、链接元素的 `draggable` 自动默认为 `true`，其他元素的 `draggable` 自动默认为 `false`；

```javascript
<div draggable="true">..</div>
```

## 其他属性和方法
* `addElement(element)`
* `clearData(format)`
* `setDragImage(element, x, y)`
* `types`
