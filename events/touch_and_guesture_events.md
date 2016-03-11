# 触摸和手势事件
随着移动设备的出现，常规的鼠标和键盘事件根本不够用，W3C 开始制定 Touch Events 规范。

要区分 **触摸事件、触摸事件 event 对象的属性、Touch 对象、Touch 对象的属性** 数个概念之间的联系与区别！
> 触摸事件触发时，会向事件处理函数传入一个 event 对象，event 对象有数个属性指向的是由 Touch 对象组成的数组，同时 Touch 对象又有着一些特定的属性

## 触摸事件
`touchstart` `touchmove` `touchend` `touchcancel`

1. 各个事件触发时机
    * `touchstart`：手指触摸屏幕时触发
    * `touchmove`：手指在屏幕上滑动时 **连续地** 触发
    * `touchend`：手指从屏幕上移开时触发
    * `touchcancel`：系统停止跟踪触摸时触发，文档没有明确说明事件的触发时机

2. 以上几个事件都会冒泡；

3. 这些触摸事件不是在 DOM 规范中定义的，但它们却是以兼容 `DOM` 的方式实现的；

4. 触摸事件 `event` 对象有以下通用属性（即 **鼠标事件中常用的属性**）
    * bubbles
    * cancelable
    * view
    * clientX
    * clientY
    * screenX
    * screenY
    * detail
    * altKey
    * shiftKey
    * ctrlKey
    * metaKey

5. 触摸事件 `event` 对象还有三个用于 **跟踪触摸的属性**
    * `touches`：表示 **当前跟踪的触摸操作** 的 **`Touch` 对象** 的数组
    * `tragetTouches`：特定于事件目标的 **`Touch` 对象** 的数组
    * `changeTouches`：表示自动上次触摸以来发生了什么改变的 **`Touch` 对象** 的数组

6. 每个 Touch 对象包含的属性
    * clientX：触摸目标在视口中的 X 坐标
    * clientY
    * identifier：标识触摸的唯一 ID
    * pageX
    * pageY
    * screenX
    * screenY
    * target：触摸的 DOM 节点目标

7. 实例：[touch_events.html][1]
    ```javascript
    function handlerTouchEvent(event) {

        // 只跟踪一次触摸
        if (event.touches.length == 1) {
            var output = document.getElementById('output');
            switch (event.type) {
                case 'touchstart':
                    output.innerHTML = 'Touch started (' + event.touches[0].clientX + ', ' + event.touches[0].clientY + ')';
                    break;
                case 'touchend':
                    output.innerHTML = '<br>Touch ended (' + event.changedTouches[0].clientX + ', ' + event.changedTouches[0].clientY + ')';
                    break;
                case 'touchmove':
                    output.innerHTML = '<br>Touch moved (' + event.changedTouches[0].clientX + ', ' + event.changedTouches[0].clientY + ')';
            }
        }
    }

    eventUtil.addEventHandler(document, 'touchstart', handleTouchEvent);
    eventUtil.addEventHandler(document, 'touchend', handleTouchEvent);
    eventUtil.addEventHandler(document, 'touchmove', handleTouchEvent);
    ```

8. 手机端事件（也会触发鼠标事件）触发先后顺序
    * touchstart
    * mouseover
    * mousemove
    * mouseup
    * click
    * touchend

## 手势事件
1. iOS 首先引入手势事件，手势事件一般 **两个手指** 在屏幕上时触发；

2. 三个手势事件
    * `gesturestart`：当一个手指已经在屏幕上，而另一个手指又触摸屏幕时触发；
    * `gesturechange`：当触摸屏幕的任何一个手指的位置发生变化时触发
    * `gestureend`：当任何一个手指从屏幕上移开时触发

3. 以上事件是冒泡的；

4. 在一个元素上设置手势事件处理程序，意味着两个手指都必须同时位于该元素的范围内；

5. 手势事件的 `event` 对象的 `target` 属性指向 **两个手指都位于其范围内的元素**；

6. 两个手指先后放上屏幕时触发事件的顺序
    * 第一指的 `touchstart` 事件
    * `gesturestart` 事件
    * 第二指的 `touchstart` 事件
    * `genturechange` 事件
    * （单指移开）`gestureend` 事件
    * `touchend` 事件

7. 手势事件的 `event` 对象的两个额外属性
    * `rotation`：手指变化引起的旋转角度，正负号分别表示顺、逆时针
    * `scale`：两个手指间距离的变化情况（以手势开始时距离为 1 计算 scale）

8. 实例：[gesture_events.html][2]
    ```javascript
    function handlerGestureEvent(event) {
        var output = document.getElementById('output');

        switch (event.type) {
            case 'gesturestart':
                output.innerHTML = 'Gesture started (rotation=' + event.rotation + ', scale=' + event.scale + ')';
                break;
            case 'gestureend':
                output.innerHTML = 'Gesture ended (rotation=' + event.rotation + ', scale=' + event.scale + ')';
                break;
            case 'gesturechange':
                output.innerHTML = 'Gesture changed (rotation=' + event.rotation + ', scale=' + event.scale + ')';
                break;
        }
    }

    eventUtil.addEventHandler(document, 'gesturestart', handlerGestureEvent);
    eventUtil.addEventHandler(document, 'gestureend', handlerGestureEvent);
    eventUtil.addEventHandler(document, 'gesturechange', handlerGestureEvent);
    ```


  [1]: http://static.zybuluo.com/yangfch3/i0yfer7fze82zz0sg30a7b0v/touch_events.html
  [2]: http://static.zybuluo.com/yangfch3/tyg345xp4zu3wyzo8z299ki8/gesture_events.html
