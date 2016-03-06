# 鼠标与滚轮事件

## 鼠标事件
1. DOM 3 级事件定义了 9 个鼠标事件
    * `click`：**按下回车键或左键**触发
    * `dbclick`：一般是连续点击两次 **左键**
    * `mousedown`：**按下任意鼠标按键** 时触发
    * `mouseup`：**按下后释放** 任意鼠标时触发
    * `mouseenter`：指针移入元素范围
    * `mouseleave`：指针移出元素范围
    * `mousemove`：指针在元素内部移动时重复触发
    * `mouseover`
    * `mouseout`

2. `mouseover` `mouseenter` 之间的关系
> `mouseover` 是逻辑关系上的事件，即一旦某 DOM  元素的子元素被 `mouseover` 了，它自己也就被 `mouseover` 了，而 `mouseenter` 却是物理上的，即只有你眼睛看到光标进入了的元素，才会触发 `mouseenter` 事件，`mouseleave` 和 `mouseout` 的区别同理。
<br>
链接：[mouseover 与 mouseenter的区别][1]

3. 除了 `mouseenter`、`mouseleave`，其他鼠标事件都会冒泡；

4. 只有在同一个元素上相继触发 `mousedown` 和 `mouseup` 事件，才会触发 `click` 事件；

5. 检测浏览器是否支持 DOM 2 级和 DOM 3 级的鼠标事件
    ```javascript
    var isSupported = document.implementation.hasFeature('MouseEvents', '2.0');

    var isSupported = document.implementation.hasFeature('MouseEvent', '3.0');
    ```

6. 一个滚轮事件：`mouseWheel`；

7. 滚轮事件可以跟踪鼠标滚轮，也可以跟踪触摸板。

---
### 1. 客户区坐标
鼠标事件对象的 `clientX` 和 `clientY` 属性，用于获取鼠标事件发生时指针在 **当前视口** 中的水平坐标和垂直坐标；

---
### 2. 页面位置坐标
1. 鼠标事件对象的 `pageX` 和 `pageY` 属性，用于获取鼠标光标在 **文档页面** 中的位置

2. 在没有滚动的情况下，`pageX` 和 `pageY` 的值和 `clientX` 和 `clientY` 的值相等

---
### 3. 屏幕坐标位置
1. 鼠标事件对象的 `screenX` 和 `screenY` 属性，用于确定鼠标事件发生时鼠标指针相对于整个屏幕的坐标信息；

2. `window` 对象也有 `screenX` 和 `screenY` 属性，用于获取浏览器左上角的屏幕坐标。

---
### 4. 修改键
1. 按下鼠标时键盘上的某些键的状态也会影响到所有采取的操作；

2. 鼠标事件的 `event` 对象有四个属性（布尔值）用于检测是否在触发鼠标事件同时按下了以下功能键
    * event.shiftKey
    * event.ctrlKey
    * event.altKey
    * event.metaKey

3. IE 8 及之前不支持 `metaKey` 键。

---
### 5. 相关元素
1. 在发生 mouseover 和 mouseout 事件时，还会涉及到其他的相关元素；
    > 这两个事件涉及到把元素指针从一个元素之内移动到另一个元素边界之内，牵涉到两个元素，所有 `event` 事件需要有两个属性用于表征 **事件发生的对象** 和 **事件的相关对象**

2. `event` 对象的 `relatedTarget` 属性提供了相关元素的信息，这个属性只在 `mouseover` 和 `mouseout` 事件中存在（其他时间这个属性的值为 `null`）；

3. IE 8 及以下不支持事件的 `relatedTarget` 属性，但有同类型的两个代替属性：
    * `mouseover` 事件的 `fromElement`
    * `mouseout` 事件的 `toElement`

4. 获取相关元素的兼容性写法
    ```javascript
    var eventUtil= {
        // ...

        getRelatedTarget: function(event) {
            if (event.relatedTarget) {
                return event.relatedTarget;
            } else if (event.toElement) {
                return event.toElement;
            } else if (event.fromElement) {
                return event.fromElement;
            } else {
                return null;
            }
        },

        // ...
    }

    // 使用 `getRelatedElement()`
    var div = getElement('#myDiv');
    eventUtil.addEventHandler(div, 'mouseover', function(event) {
        event = eventUtil.getEvent(event);

        var target = eventUtil.getTarget(event),
            relatedTarget = eventUtil.getRelatedTarget(event);

        alert('Mouse out of ' + target.tagName + 'to ' + 'relatedTarget.tagName');
    });
    ```

---
### 6. 鼠标按钮
1. 对于 `mousedown` 和 `mouseup` 事件来说，`event` 对象存在一个 `button` 属性；

2. 标准的 DOM 规范指定 `event` 对象的 `button` 属性有以下值：
    * 0：鼠标主按钮
    * 1：鼠标中键（滚轮）
    * 2：鼠标右键

3. IE 8 及以下对 `event` 对象提供的 `button` 属性值：虽然详尽但并不实用
    * 0：没有按下按钮
    * 1：主鼠标按钮
    * 2：次鼠标按钮
    * 3：同时安歇主、次鼠标按钮
    * 4：鼠标中键
    * 5：同时按下鼠标中键、主鼠标按钮
    * 6：同时按下鼠标中键、次鼠标按钮
    * 7：同时按下 3 个鼠标按钮

3. 兼容性写法
    ```javascript
    var eventUtil = {
        // ...

        getButton: function(event) {
            if (document.implementation.hasFeature('MouseEvents', '2.0')) {
                return event.button;
            } else {
                switch (event.button) {
                    case 0:
                    case 1:
                    case 3:
                    case 5:
                    case 7:
                        return 0;
                    case 2:
                    case 6:
                        return 2;
                    case 4:
                        return 1;
                }
            }
        },

        // ...
    }
    ```

---
### 7. 鼠标事件其他信息
1. DOM 2 级规范在 `event` 对象上提供了 `detail` 属性；

2. 对于鼠标事件来说，`detail` 属性存储的是在给定位置单击的次数，一旦移位，点击数重置为 0；

3. IE 还为鼠标事件 `event` 对象提供了其他属性
    * altLeft
    * ctrlLeft
    * shiftLeft
    * offsetX
    * offsetY

4. IE 的 `event` 对象的这些属性用处不大，或者可以通过其他方式获取。

---
### 8. 鼠标滚轮事件
1. `mousewheel` 事件；

2. 可以在任何元素上触发，最终冒泡到 `document` 或 `window` 对象；

3. `mousewheel` 事件 `event` 对象的属性：`wheelDelta`
    * 向前滚时：`wheelDelta` = `+ n120`
    * 向后滚时：`wheelDelta` = `+ n120`

4. `Firefox` 支持 `DOMMouseScroll` 事件与 `mouseWheel` 事件相似，鼠标滚动信息保存在 `event` 对象的 `detail` 属性上
    * 向前滚动时：`deatil` 的值 `-3`；
    * 向后滚动时：`detail` 的值 `+3`。

5. 处理鼠标滚轮事件获取滚动信息的跨浏览器解决方案：
    ```javascript
    var eventUtil = {
        // ...

        getWheelDelta: function(event) {
            if (event.wheelDelta) {
                return event.wheelDelta;
            } else {
                return -event.detail * 40;
            }
        },

        // ...
    }
    ```

6. 绑定滚轮事件
    ```javascript
    function handleMouseWheel(event) {
        event = eventUtil.getEvent(event);
        var delta = eventUtil.getWheelDelta(event);
        alert(delta);
    }

    eventUtil.addEventHandler(document, 'mouseWheel', handleMouseWheel);
    eventUtil.addEventHandler(element, 'DOMMouseScroll', handleMouseWheel);
    ```

7. 如果指定的事件不存在，则为该事件指定处理程序的代码会静默失败。
---
### 9. 触摸设备上的鼠标事件
1. 触摸设备没有鼠标设备，但是因为 **桌面站点也需要在手机上正常浏览**，所以这些设备也会支持桌面站点上的鼠标事件；

2. 注意事项
    * 没有 `dbclick` 事件，移动设备默认双击放大；
    * 轻击可单击元素会触发 `mousemove` 事件，并在之后会依次触发 `mousedown` `mouseup` `click` 事件
    * `mousemove` 事件也会触发 `mouseover` `mouseout` 事件
    * 两个手指放在屏幕上且页面随手指移动而滚动时会触发 `mousewheel` 和 `scroll` 事件

3. 无障碍性浏览
    * 盲人可以使用回车键代替鼠标中键触发 `click` 事件；
    * 其他时间都无法使用键盘代替鼠标触发，这会为盲人浏览带来不便。


  [1]: http://www.cnblogs.com/libmw/articles/2600747.html
