# 表单基础知识
1. `form` 元素在 DOM 中的对应类型是：`HTMLFormElement`；

2. `HTMLFormElement` 继承自 `HTMLElement`，因此与其他 HTML 元素具有相同的默认属性和方法；

3. `HTMLFormElement` 扩展的独有属性
    * `acceptCharset`
    等价于表单元素的 `accept-charset` 特性
    * `action`：URL
    等价于 `action` 特性
    * `elements`：表单中所有 **控件** 的集合
    * `enctype`：请求的编码类型
    等价于 `enctype` 特性
    * `length`：表单控件的数量
    * `method`：`HTTP` 请求类型
    等价于 `method` 特性
    * `name`
    等价于 `name` 特性
    * `target`：取得目标窗口或框架的名称
    等价于 `target` 特性

4. `HTMLFormElement` 扩展的独有方法
    * `reset()`
    * `submit()`
    * `checkValidity()`

5. 获取（引用）表单元素的方式
    1. `getElementById()`
    2. `document.forms` 取得页面中的所有表单，再通过索引或 `name` 特性来取得特定的表单
    ```javascript
    // 取得页面中的表单集合
    var forms = document.forms;

    // 取得页面中的第一个表单元素
    var firstForm = forms[0];

    // 取得页面中 name 为 form2 的表单元素
    var secondForm = forms['form2'];
    ```

## 提交表单
提交表单可能出现的最大问题是：**重复提交表单**。

* 第一次提交表单时禁用表单按钮（为提交按钮添加 `disabled` 特性）
* 利用 `onsubmit` 事件处理程序取消后续的表单提交操作

不要通过检测 `submit` 按钮的点击事件来处理表单提交
> 不同浏览器的 `submit` 和 `click` 事件触发的先后顺序存在差异

### 通过 HTML 进行提交

* `input` 元素，`type` 为 `submit`
* `input` 元素，`type` 为 `image`（图像按钮）
* `button` 元素，`type` 为 `submit`

1. 注意：只要表单（`form`）上存在以上任何一种按钮，那么在相应表单控件拥有焦点（`textarea` 例外）的情况下，按下回车键就会进行表单提交；

2. 以 HTML 的方式提交表单时，浏览器会在将请求发送给服务器之前在对应的表单对象上触发 `submit` 事件；

3. 由于第 2 点提到的特性，我们可以有机会在浏览器发送表单之前验证表单数据，并决定是否阻止表单（`return false`）的提交。

```javascript
var form = document.getElementById('myForm');

eventUtil.addEventHandler(form, 'submit', function(event) {
    event = eventUtil.getEvent(event);

    if (...) {
        event.preventDefault(event);
    }
})
```

### 通过 JS 进行提交。
通过调用表单对象的 `submit()` 方法进行提交。

1. 在以调用 `submit()` 方法的形式提交表单时，不会触发 `submit` 事件；
    > 因此记得在调用此方法之前验证表单数据

## 重置表单
1. 使用 `HTML` 来重置表单
    * `input` 元素 -- `type` 为 `reset`
    * `button` 元素 -- `type` 为 `reset`

2. 使用 JS 来重置表单：利用表单对象的 `reset()` 方法；

3. 使用 HTML 或 JS 重置表单都会触发 `reset` 事件；

4. 慎用表单重置功能。

## 表单字段
访问表单元素的方法

* DOM 方法获取
* 表单对象的 `elements` 元素集合的索引或 `name` 获取
    > 通过表单对象的 `elements` 序列的 `name` 特性来获取到的表单元素可能是 `NodeList`。
    > 因为同一个表单中可能存在多个 `name` 相同元素


### 1. 共有的表单字段属性
>* `disabled`：布尔值，可写，是否禁用表单元素
* `form`：指向字段所属的表单对象
* `name`
* `readOnly`：是否只读
* `tabIndex`：`tab` 序号
* `type`
* `value`

1. 可以通过 JS 动态修改以上除 `form` 外的所有属性；

2. 特殊的注意点
    * 文件控件的 `Value` 是只读的
    * `select` 空间的 `type` 属性是只读的

### 2. 共有的表单字段方法
> * `focus()`：设置焦点，激活该控件
* `blur()`：取消焦点

1. 表单元素在以下情况使用 `focus()` 方法会出错
    * `input` 的 `type` 为 `hidden`
    * 表单元素的样式 `display` 为 `none`
    * 表单元素的样式 `visibility` 为 `hidden`

2. HTML 5 为表单字段新增了 `autofocus` 特性，用于页面载入时表单元素的自动聚焦。

### 3. 共有的表单字符方法
> * `blur`
* `change`
* `focus`

1. `change` 事件触发的时间
    > `input` 或者 `textarea` 元素 **失去焦点** 且 **`value` 改变** 时触发
    > `select` 元素变更了选择项时立即触发

2. `change` 事件处理程序常用于验证用户在字段中输入的数据；

3. `focus` 和 `blur` 事件变更输入框的背景颜色可以提升用户体验

4. `blur` 和 `change` 事件在不同的浏览器下触发的顺序不同。
