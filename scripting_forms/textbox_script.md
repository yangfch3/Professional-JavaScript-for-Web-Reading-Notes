# 文本框脚本
1. 两种文本框
    * `input type="text"` 单行文本框
    * `textarea` 多行文本框

2. 单行文本框特征
    * 设置 `size` 特性，可以指定文本框能显示的字符数
    * `maxlength` 特性，指定文本框接收的最大字符数

3. 多行文本框特征
    * `rows` `cols` 指定文本框的字符行数和列数
    * 初始值放在 `<textarea>` `</textarea>` 之间
    * 用户输入的内容也会保存在 `value` 属性中
    ```HTML
    <textarea rows="5" cols="3">default value!<textarea>
    ```
    <textarea>default value!</textarea>

## 选择文本
1. 单行、多行文本框都支持 `select()` 方法；

2. [demo：在文本框获得焦点时选择其所有文本，方便用户的操作（复制或删除）](http://static.zybuluo.com/yangfch3/7dnoazqz3hrgl4rsdz1df1gl/demo.html)

### 1. 选择（select）事件
1. 与 `select()` 方法对应的时间，当 **文本框** 中的文本被选中时触发；

2. 各个浏览器对已 `select` 事件的触发事件处理不同
    * 现代浏览器在用户释放鼠标时触发
    * 老版 IE 在用户选择一个字母（不需要放开鼠标）即触发

3. 另外，当调用 `select()` 方法时也会触发 `select` 方法。

### 2. 取得选择的文本
1. 通过监听 `select` 事件我们得知用户什么时候选择了文本；

2. HTML 5 扩展为文本框元素添加了两个属性，用以表示用户选择的文本的索引开始与结束值
    * `textBox.selectionStart`：默认为 0
    * `textBox.selectionEnd`：默认为 0

3. IE 8 及以下版本不支持 `textBox.selectionStart` 和 `textBox.selectionEnd` 两个属性，但支持一个 **针对整个文档的 `selection` 属性**
    ```javascript
    // Run under or eq IE 8
    console.log(document.selection.createRange().text;);
    ```
    > IE 8 及以下 `document` 对象有一个 `selection` 属性，这个属性是一个对象，其中保存着用户在整个文档范围内选择的文本信息

4. [demo：兼容各个浏览器地获取选择的文本][1]

### 3. 选择部分文本
1. 文本框对象的 `select()` 方法可以帮助我们选择文本框内的所有文本；

2. 所有文本框元素都有一个 `setSelectionRange()` 方法，可以实现使用脚本任意选择部分文本
    * 参数一：开始字符串索引
    * 参数二：结束字符串索引

    ![2016-03-20_212238.jpg-57.5kB][2]

3. IE 8 及以下版本选择部分文本的方法
    * 使用 IE 在所有文本框上提供的 `createTextRange()` 方法创建一个范围
    * 对 **该范围** 使用 `collapse()` 方法将范围折叠到文本框的开始位置
    * 使用 `moveStart()` 和 `moveEnd()` 这两个范围方法将范围移动到位
    * 最后，使用范围的 `select()` 方法选择文本

4. 兼容性的选择文本框的部分文本的封装函数，以及 [demo.html][3]
    ```javascript
    function selectText(textBox, startIndex, stopIndex) {
        if (textBox.setSelectionRange) {
            textBox.setSelectionRange(startIndex, stopIndex);
        } else if(textBox.createTextRange) {
            var range = textBox.createTextRange();
            range.collapse(true);
            range.moveStart('character', startIndex);
            range.moveEnd('character', stopIndex - startIndex);
            range.select();
        }

        textBox.focus();
    }
    ```

## 过滤输入
JavaScript 可以完成许多过滤输入的操作，综合运用事件和 DOM 手段，可以将普通的文本框转变为智能型的功能控件。

### 1. 屏蔽字符
实例：运用 **正则表达式、事件** 完成对文本框输入内容的限定 -- 只需输入数字
```javascript
eventUtil.addEventHandler(textBox, 'keypress', function(event) {
    event = eventUtil.getEvent(event);
    var target = event.getTarget(event);
    var charCode = event.getCharCode(event);

    if (!/\d/.test(String.fromCharCode(charCode)) && charCode > 9 && !event.ctrlKey) {
        eventUtil.preventDefault(event);
    }
})
```

## 操作剪切板
1. HTML 5 把剪贴板事件纳入了规范；

2. 6 个剪贴板事件
    * `beforecopy`
    * `copy`
    * `beforecut`
    * `cut`
    * `beforepaste`
    * `paste`

3. 要访问剪贴板中的数据，使用 `clipboardData` 对象
    * 在 IE 中 `clipboardData` 对象是 `window` 对象的属性，可以 **随时访问剪贴板**
    * 在其他浏览器中 `clipboardData` 对象是剪贴板事件触发时 `event` 对象的属性，只有 **在剪贴板事件发生时才能访问**

4. 在实际的 `copy` `cut` `paste` 事件发生前可以通过 `beforecopy` `beforecut` `beforepaste` 事件向剪贴板读写数据；

5. 在 `copy` `cut` `paste` 发生时可以 **检测剪贴板数据是否符合表单输入的要求**，然后**使用 `event.preventDefault()` 来阻止剪贴操作**；

6. `clipboardData` 对象有三个方法
    * `getData()`
        * 参数：取得的数据的格式
    * `setData()`
        * 参数一：数据类型
        * 参数二：设置的文本
    * `clearData()`


## 自动切换焦点
为文本框设置 `keyup` 事件监听程序，搭配文本框的 `max-length` 特性，通过 `DOM` 寻找下一个可输入的文本框元素，自动切换焦点

## HTML 5 约束 API
HTML 5 规范为表单元素新增了许多特性，这些特性用于约束表单元素的行为，使得我们一些以前需要使用 JavaScript 的 **校验、功能** 操作已经完全由浏览器自动化了。

移动端对 HTML 5 的规范实现较好，现代浏览器也基本实现了这些 HTML 5 规定的新特性。

### 1. 必填字段
* `required` 属性
* 有该属性的表单元素不能留空，浏览器不会提交
* 适用于 `input` `textarea` `select` 字段

### 2. 其他输入类型（type）
1. 字符串类
    * `email`
    * `url`
2. 数字类
    * `number`
    * `range`
    * `datatime` `datatime-local`
    * `time` `date` `month` `week`

设置了特殊输入类型的 `input` 元素浏览器会自动检测其 `value` 看是否符合格式要求。

不支持特殊输入类型的浏览器遇到新特性会自动退化为 `type="text"`。

### 3. 数值范围
以上数字类的输入类型，可以指定以下特性对其做限定

* `min` 属性 -- 最小值
* `max` 属性 -- 最大值
* `step` 属性 -- 单次增减值

伴随着 `step` 出现的是向上向下调节的两个箭头。

### 4. 输入模式
1. HTML 5 为 **文本字段** 新增了 `pattern` 特性，特性值是一个正则表达式，用于检查匹配文本框中的值；

2. **注意点**：这里的正则表达式不需要 `^` `$`，默认已有；

3. 通过表单元素对象的 `pattern` 属性可以访问输入模式特性。

### 5. 检查有效性
1. 检查某个字段是否有效：在具体的表单元素对象上调用 `checkValidity()` 方法
    > 有效的标准：
    > * 必填字段不能为空
    > * 符合 `pattern` 模式匹配
    > * 是否满足约束条件限制

2. 检查整个表单是否有效：对表单对象调用  `checkValidity()` 方法
    > 所有表单字段都有效时返回 `true`

3. 更具体的获取有效性信息：访问表单元素的 `validity` 属性（对象）
    * `customError`
    * `patternMismatch`
    * `rangeOverflow`
    * `rangeUnderflow`
    * `stepMisMatch`
    * `tooLong`
    * `typeMismatch`
    * `valueMissing`

### 6. 禁用验证
1. 对表单添加 `novalidate` 特性，禁用表单的原生验证；

2. `JavaScript` 中表单对象的 `noValidate` 属性对应表单的这个特性，可读写；

3. 对提交按钮添加 `formnovalidate` 属性也可以禁用表单的原生验证；

4. `JavaScript` 中表单提交元素的 `formNoValidate` 属性对应这个特性，可读写。





  [1]: http://static.zybuluo.com/yangfch3/rec7vnmi1h0g5g1u95av1dhf/demo.html
  [2]: http://static.zybuluo.com/yangfch3/56edryxqebn7d1hv9l0kczmv/2016-03-20_212238.jpg
  [3]: http://static.zybuluo.com/yangfch3/jb8u94jqgumkaf1ihb6iqcgt/demo.html
