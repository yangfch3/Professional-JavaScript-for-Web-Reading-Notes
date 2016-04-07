# 拖放
1. 拖放的基本原理：绝对定位，鼠标点击、移动拖尾，释放时停止拖尾；

2. 最简单的拖放实现
    ```javascript
    var DragDrop = function() {
        var dragging = null;
        function handleEvent(event) {
            // 获取事件和目标
            event = eventUtil.getEvent(event);
            var target = eventUtil.getTarget(event);

            // 确定事件类型
            switch(event.type) {
                case 'mousedown':
                    if(target.calssName.indexOf('draggable') > -1) {
                        draging = target;
                    }
                    break;
                case 'mousemove':
                    if(dragging !== null) {
                        // 指定位置
                        dragging.style.left = event.clientX + 'px';
                        dragging.style.top = event.clientY + 'px';
                    }
                    break;
                case 'mouseup':
                    dragging = null;
                    break;
            }
        }
    }

    // 公共接口
    return {
        enable: function() {
            eventUtil.addEventHandler(document, 'mousedown', handleEvent);
            eventUtil.addEventHandler(document, 'mousemove', handleEvent);
            eventUtil.addEventHandler(document, 'mouseup', handleEvent);
        },
        disable: function() {
            eventUtil.removeEventHandler(document, 'mousedown', handleEvent);
            eventUtil.removeEventHandler(document, 'mousemove', handleEvent);
            eventUtil.removeEventHandler(document, 'mouseup', handleEvent);
        }
    }();
    ```

3. 使用
    ```javascript
    // HTML
    <div class="draggable" style="position: absolute; background-color: red"></div>

    // JS
    DragDrop.enable();
    ```

4. 使用 `draggable` 类名识别是否需要为元素开启拖放，上述代码会自动针对所有包含 `draggable` 类名的元素开启；

5. 简单 `DragDrop` 的缺点
    * 初始点击元素会发生一次闪跳
    * 每次拖放，鼠标的位置全部会重定位到元素左上角

## 修缮拖放功能
1. 解决上述两个问题的思路是：在 `mousemove` 的过程中对元素的定位进行精确的计算，不简单地使用 `clientX` 和 `clientY`；

2. 定位元素的机制：`offsetLeft` `offsetTop` `clientY` `clientX`；

3. 改进后的代码
    ```javascript
    var DragDrop = function() {
        var dragging = null,
            diffX = 0, // !
            diffY = 0; // !

        function handleEvent(event) {
            // 获取事件和目标
            event = eventUtil.getEvent(event);
            var target = eventUtil.getTarget(event);

            // 确定事件类型
            switch(event.type) {
                case 'mousedown':
                    if(target.className.indexOf('draggable') > -1) {
                        dragging = traget;
                        diffX = event.cliextX - target.offsetLeft;
                        diffY = event.cliextY - target.offsetTop;
                    }
                    break;
                case 'mousemove':
                    if(dragging !== null) {
                        // 指定位置
                        dragging.style.left = (event.clientX - diffX) + 'px';
                        dragging.style.top = (event.clientY - diffY) + 'px';
                    }
                    break;
                case 'mouseup':
                    dragging = null;
                    break;
            }
        }

        // 公共接口
        return {
            enable: function() {
                eventUtil.addEventHandler(document, 'mousedown', handleEvent);
                eventUtil.addEventHandler(document, 'mousemove', handleEvent);
                eventUtil.addEventHandler(document, 'mouseup', handleEvent);
            },
            disable: function() {
                eventUtil.removeEventHandler(document, 'mousedown', handleEvent);
                eventUtil.removeEventHandler(document, 'mousemove', handleEvent);
                eventUtil.removeEventHandler(document, 'mouseup', handleEvent);
            }
        }
    }();
    ```

4. 使用修缮后的代码可以得到一个更加平滑的拖放体验。

## 封装拖放组件：使用自定义事件
1. 上面修缮拖放效果之后仍存在的缺陷：**没有提供任何方法表示拖动开始、正在拖动或者已经结束**；

2. 使用自定义事件来制定我们自己的 **拖动开始、正在拖动、拖动结束** 事件；

3. 代码
    ```javascript
    function EventTarget() {
        this.handlers = {};
    }

    EventTarget.prototype = {
        constructor: EventTarget,
        addHandler: function(type, handler) {
            if(typeof this.handlers[type] == undefined) {
                this.handlers[type] = [];
            }
            this.handlers[type].push(handler);
        },
        fire: function(event) {
            if(!event.target) {
                event.target = this;
            }
            if(this.handlers[event.type] instanceof Array) {
                var handlers = this.handlers[event.type];
                for(var i = 0, len = handlers.length; i < len; i++) {
                    handlers[i](event);
                }
            }
        },
        removeHandler: function(type, handler) {
            if(this.handlers[type] instanceof Array) {
                var handlers = this.handlers[type];
                for(var i = 0, len = handlers.length; i < len; i++;) {
                    if(handlers[i] === handler) {
                        break;
                    }
                    handlers.splice(i, 1);
                }
            }
        }
    };

    var DragDrop = function() {
        var dragdrop = new EventTarget(),
            dragging = null,
            diffX = 0,
            diffY = 0;

        function handleEvent(event) {
            // 获取事件和对象
            event = eventUtil.getEvent(event);
            var target = eventUtil.getTarget(event);

            // 确定事件类型
            switch(event.type) {
                case 'mousedown':
                    if(target.className.indexOf('draggable') > -1) {
                        dragging = traget;
                        diffX = event.clientX - target.offsetLeft;
                        diffY = event.clientY - traget.offsetTop;

                        dragdrop.fire({type: 'dragstart', target: dragging, x: event.clientX, y: event.clientY});
                    }
                    break;

                case 'mousemove':
                    if(dragging !== null) {
                        // 指定位置
                        dragging.style.left = (event.clientX - diffX) + 'px';
                        dragging.style.top = (event.clientY - diffY) + 'px';

                        // 触发自定义事件
                        dragdrop.fire({type: 'drag', target: dragging, x: event.clientX, y: event.clientY});
                    }
                    break;

                case 'mouseup':
                    dragdrop.fire({type: 'dragend', traget: dragging, x: event.clientX, y: event.clientY});
                    dragging = null;
                    break;
            }
        }

        dragdrop.enable = function() {
            eventUtil.addEventHandler(document, 'mousedown', handleEvent);
            eventUtil.addEventHandler(document, 'mousemove', handleEvent);
            eventUtil.addEventHandler(document, 'mouseup', handleEvent);
        };

        dragdrop.disable = function() {
            eventUtil.removeEventHandler(document, 'mousedown', handleEvent);
            eventUtil.removeEventHandler(document, 'mousemove', handleEvent);
            eventUtil.removeEventHandler(document, 'mouseup', handleEvent);
        };

        return dragdrop;

    }();
    ```

4. 我们在 3 中创建了三个事件：`dragstart` `drag` `dragend`，受到启发，我们很容易地能够使用浏览器原生事件进行再加工指定出一系列自定义事件；

5. 三个自定义事件调用的方法
    ```javascript
    DragDrop.addHandler('dragstart', function(event) {
        var status = document.getElementById('status');
        status.innerHTML = 'Started dragging ' + event.target.id;
    })
    ```
