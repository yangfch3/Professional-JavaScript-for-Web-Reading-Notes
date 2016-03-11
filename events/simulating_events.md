# 模拟事件
事件经常由用户操作触发或其他浏览器功能触发，我们也可以使用 JavaScript （的模拟事件）在任意时刻来触发指定事件，就如同触发浏览器创建的事件一样。

IE 有它自己模拟事件的方式。\_(:зゝ∠)_

## 1. DOM 中的事件模拟
1. 使用 `document` 对象上的 `createEvent()` 方法创建 `event` 对象；
    * 接收一个参数：事件类型字符串
    * DOM 2 级中规定这个可接收的事件类型字符串
        * `UIEvents`
        * `MouseEvents`
        * `MutationEvents`
        * `HTMLEvents`
    * DOM 3 级中以上事件类型字符串全部单数化
    * 通用事件类型字符串：`Events`
    * DOM 3 级规定了键盘事件：`KeyboardEvent`，但各个浏览器的实现存在较大的差异

2. 创建了 `event` 对象后，还需要使用与 `event` 对象信息对创建的事件进行初始化，每种类型的 `event` 对象都有特定的一个方法，接受传入的参数对自己进行初始化；
    * `initUIEvent` -- UIEevent
    * `initMouseEvent` -- MouseEvents
    * `initMutationEvent` -- MutationEvents
    * `initEvent` -- HTMLEvents 与通用事件（`Events`）
    * 初始化键盘事件在各个浏览器下存在差异

3. 模拟事件的最后一步是在 DOM 节点上触发该事件：使用 DOM 节点的 `dispatchEvent()` 方法，传入我们创建的 `Event` 对象。

### 1. 模拟鼠标事件
1. 步骤实例
    ```javascript
    var btn = document.getELementById('myBtn');

    // 预先为元素添加好事件处理程序
    eventUtil.addEventHandler(btn, 'click', function() {
        alert('Hi!')
    })

    // 新建鼠标事件对象
    var event = document.createEvent('MouseEvents');

    // 初始化事件对象
    event.initMouseEvent('click', true, true, document.defaultView, , 0, 0, 0, 0, 0, false, false, false, 0, null);

    // 在目标元素上触发事件
    btn.dispatch(event);
    ```

2. 鼠标事件 `event` 对象的 `initMouseEvent` 方法接收的参数
    * `type`：事件类型名
    * `bubbles`：是否应该冒泡，设为 `true`
    * `cancelable`：是否可以取消，设为 `true`
    * `view`：与事件关联的视图，一般都设为 `document.defaultView`
    * `detail`：如没有事件详细信息，设为 0
    * `screenX`：相对于屏幕的 X 坐标
    * `screenY`：相对于屏幕的 Y 坐标
    * `clientX`：相对于视口的 X 坐标
    * `clientY`：相对于视口的 Y 坐标
    * `ctrlkey`：是否同时按下了 `ctrl`，默认为 `false`
    * `altkey`
    * `shiftkey`
    * `metakey`
    * `button`：按下了那个鼠标按键，默认为 0
    * `relatedTarget`：与事件相关联的对象，一般为 `null`，只在模拟 `mouseover` 或 `mouseout` 时使用

### 2. 模拟键盘事件
1. DOM 2 级中没有就模拟键盘事件作出规定，导致各个浏览器实现键盘事件模拟的情况存在差异；

2. DOM 3 级标准中的模拟键盘事件：`KeyboardEvent` `initKeyboardEvent`
    * 实例
    ```javascript
    var textBox = document.getElementById('myTextBox'),
        event;

    // 检测浏览器是否支持 DOM 3 级的模拟键盘事件
    if (document.implementation.hasFeature('KeyboardEvents', '3.0')) {
        // 以 DOM 3 级的方式创建键盘事件对象
        event = document.createEvent('KeyboardEvent');

        // 初始化事件对象
        event.initKeyboardEvent('keydown', true, document.defaultView, 'a', 0, 'shift', 0);
    }

    // 触发事件
    textBox.dispatchEvent(event);
    ```
    * `initKeyboardEvent` 接受的参数列表
        * `type`
        * `bubbles`
        * `cancelable`
        * `view`
        * `key`：按下键的键码
        * `location`：按下了哪里的键，0--主键盘，1--左，2--右，3--数字键盘，4--移动设备，5--手柄
        * `modifiers`
        * `repeat`

3. Firefox 中的模拟键盘事件：现已与 DOM 3 级标准一致，旧版本与 DOM 3 级标准存在差异（见 P<sub>408</sub>）

4. 其他未标准化，又没有自己专有扩展的浏览器则需要创建一个通用的事件，再向事件对象添加键盘事件特有的信息
    ```javascript
    var textBox = document.getElementById('myTextBox');

    // 创建通用事件对象
    var event = document.createEvent('Events');

    // 初始化事件对象
    event.initEvent('keydown', true, true);
    event.view = document.defaultView;
    event.altKey = false;
    event.shiftKey = true;
    event.keyCode = 65;
    event.charCode = 65;

    // 触发事件
    textBox.dispatchEvent(event);
    ```

5. 使用通用事件模拟键盘事件：**多数浏览器使用模拟键盘事件虽然能触发键盘事件，但无法完成文本输入，因为无法精确模拟事件导致的。**

### 3. 模拟其他事件
1. `MutationEvents` `initMutationEvent()`
    `HTMLEvents` `initEvent()`

2. 一般 HTML 事件和变动事件较少使用，也很少需要模拟。

### 4. 自定义 DOM 事件
1. 使用 `CustomEvent` `initCuntomEvent()`；

2. `initCuntomEvent()` 接收以下四个参数
    * `type`
    * `bubbles`
    * `cancelable`
    * `detail`

3. 实例
    ```javascript
    var div = document.getElementById('myDiv'),
        event;

    eventUtil.addEventHandler(div, 'myEvent', function(event) {
        alert('div: ' + event.detail);
    });

    eventUtil.addEventHandler(document, 'myEvent', function(event) {
        alert('document: ' + event.detail);
    });

    if (document.implementation.hasFeature('CustomEvents', '3.0')) {
        event = document.createEvent('CunstomEvent');
        event.initCustomEvent('myEvent', true, false, 'Hello world!');
        div.dispatch(event);
    }
    ```

## 2. IE 中的事件模拟
1. IE 8 及以前版本没有标准的事件模拟，自己有一套。\_(:зゝ∠)_

2. `document` 对象的 `createEventObject()`
    `fireEvent()`

3. 见 P<sub>410, 411</sub>
