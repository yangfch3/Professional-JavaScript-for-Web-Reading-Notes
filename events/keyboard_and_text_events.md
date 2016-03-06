## 键盘与文本事件
1. DOM 3 级事件为键盘事件制定了规范；

2. **三个键盘事件**：
    * keydown：按住不放会重复触发
    * keyup
    * keypress：按住不放会重复触发，按下 `esc` 键也会触发

3. 所有元素都支持以上三个事件，**但只有在通过文本框输入文本时才最常用到**；

4. **一个文本事件**：`textInput`
    >文本事件是对 `keypress` 事件的补充，在文本插入文本框之前会触发，更便于我们拦截插入文本；

5. 一次按键触发三个键盘事件的先后顺序：`keydown` ---> `keypress` ---> `keyup`

### 1. 修改键属性
键盘事件对象也有修改键属性：`shiftkey` `ctrlkey` `altkey` `metakey`。

各个属性用于保存事件发生时是否按下了四个修改键（布尔值），IE 不支持 `metakey` 属性。

### 2. 键码（keycode）
常用键码键码表：P<sub>380</sub>

|键|键码|
|:---:|:---:|
|Enter|13|
|Tab|9|
|Shift|16|
|Ctrl|17|
|Esc|27|


1. 发生 `keydown` 和 `keyup` 事件时，`event` 对象有一个 `keycode` 属性中会包含一个代码，与键盘上一个特定的键对应；

2. 通过键码属性，可以得知用户按下的是键盘上的那个键；

3. `keycode` 中的数字、字母、基础符号的键码与 ASCII 码中的编号相同；
    > 不管输入的是大写还是小写字母，键码都为小写字母在 ASCII 中的编码。

4. 兼容性问题：分号 `;`
    FF、Opera 下键码为 59，即分号在 ASCII 中的编码；IE、Safari、Chrome 下返回 186（键码）。

---
### 3. 字符编码（charCode）
1. IE 9+ `event` 对象有 `charCode` 属性，数值，保存着按下那个键所代表的 ASCII 字符；

2. 只有在触发 `keypress` 事件时 `event` 对象的 `charCode` 属性才包含值；

3. IE 8 及以前则是直接在 keycode 中保存字符的 ASCII 编码；

4. 跨浏览器获取按键的 ASCII 编码
    ```javascript
    var eventUtil = {
        // ...

        getCharCode: function(event) {
            if (typeof event.charCode) {
                return event.charCode;
            } else {
                return event.keycode;
            }
        }

        // ...
    }
    ```

5. 使用：
    ```javascript
    var textBox = document.getElement('#myText');

    eventUtil.addEventHandler(textBox, 'keypress', function(event) {
    event = eventUtil.getEvent(event);

    var charCode = eventUtil.getCharCode(event);
    alert(charCode);
    })
    ```

6. `keycode` 与 `charCode` 的区别
    > `keycode` 一般用于获取用户按下的所有键的键码，`charCode` 用于获取输入的字符的编码。

7. 获取到的 `charCode` 可以使用 `String.fromCharCode(charCode)` 将其转换为实际的字符。

### 4. DOM 3 级变化
1. DOM 3 级事件取消了 `charCode` 属性，并为键盘事件新加了四个属性：

* `key`：按下字符键为相应的文本字符，按下非字符键为键名（Shift, ctrl, Alt...）
* `char`：按下字符键为相应的文本字符，按下非字符键为 `null`
* `keyIdentifier`：按下字符键时为相应文本的 `Unicode` 码，按下非字符键为键名（Shift, ctrl, Alt...）
* `location`：按下了什么位置的键，0 -- 默认位置，1 -- 左侧位置，2 -- 右侧位置，3 -- 数字小键盘，4 -- 移动设备键盘，5 -- 手柄

2. DOM 3 级事件为键盘事件 event 对象新增的方法：`getModifierState(modifierKey)`，用于获取修改键的状态（被按下为 true，否则为 false）

3. 以上新增的方法没有得到很好的支持，存在兼容性问题，并且很容易使用现有属性和方法代替，所以平时开发不要使用。

### 5. textInput 事件
1. 只有可编辑区域才能触发 `textInput` 事件；

2. 只有在用户按下能够输入实际字符的按键时才能被触发；

3. `textInput` 事件的 `event` 对象有一个 `data` 属性，保存着用户实际输入的字符串（不是字符编码）；

4. `textInput` 事件的 `event` 对象还有一个 `inputMethod` 属性，表示把文本输入到文本框中的方式（只有 IE 支持）
    * 0：浏览器也不知道是咋输入的
    * 1：键盘输入
    * 2：粘贴输入
    * 3：拖放进来
    * 4：使用 IME 输入
    * 5：表单选择输入
    * 6：手写输入
    * 7：语音输入
    * 8：以上方式组合输入
    * 9：脚本输入
