# HTML5
HTML5 规范定义了一系列与 DOM 规范重叠的 API。

HTML5 的规范涉及面非常广，新增的 DOM API 只是其一部分。

## getElementsByClassName()
1. 可以通过 `document` 和所有 `HTML` 元素对象调用此方法；

2. 返回带有指定类的 `NodeList`；

3. 传入的一个字符串参数 **可以包含多个类**；

## classList 属性
1. 所有元素都拥有，是新集合类型 `DOMTokenList` 的对象实例；

2. 访问 `classList` 的项：`[]` `item()`
    `classList` 对象属性：`length`

3. 删除一个元素多个类名中的某个类名的旧版方法：
    ```javascript
    function removeClass(element, toDelClass) {
        var classNames = element.className.split(/\s+/);

        var pos = -1,
            i,
            len = classNames.length;

        for (i = 0; i < len; i++ ) {
            if (classNames[i] == toDelClass) {
                pos = i;
                break;
            }
        }

        classNmaes.splice(i, 1);

        element.className = classNames.join(' ');
    }

    removeClass(document.body, 'any-class');
    ```

4. 使用 `classList` 对象的方法操作元素的 `className`：
    ```javascript
    var element = document.getElementById('myDiv');

    // add(value) -- If already exists do nothing
    element.classList.add('newClassName'); // >>undefined

    // contains(value)
    element.classList.contains('newClassName'); // >>true

    // remove(value)
    element.classList.remove('newClassName'); // >> undefined

    // toggle(value) -- add it when exist otherwise delete it
    element.classList.toggle('newClassName'); // >> undefined
    ```

## 焦点管理
### document.activeElement
1. `document.activeElement` 指向页面中当前获得了焦点的元素；

2. 元素获得焦点的方式：
    * 页面加载
    * Tab 切换
    * 元素的 `focus()` 方法

3. 文档刚加载完，`document.activeElement` 指向 `document.body`，文档加载期间，`document.activeElement` 指向为 `null`；

### document.hasFocus()
1. 该方法用于确定文档是否获得了焦点；

2. 可以通过检测文档是否获得了焦点得知用户是否正在与页面交互.

## HTMLDocument 的变化
### readyState
1. 属性可用于实现一个文档的加载完成指示器；

2. `loading`：正在加载文档；
`complete`：已经加载完成；

### 兼容模式
1. `document.compatMode` 可以侦测渲染页面的是标准模式还是混杂模式；

2. 两个取值
    `CSS1Compat`：标准模式
    `BackCompat`：混杂模式

### head 属性
1. 类似 `document.body`，`document.head` 用于快捷访问文档的 `head` 标签；

2. 兼容性写法
    ```javascript
    var head = document.head || document.getElementsByTagName('head')[0]
    ```

## 字符集属性
1. `document.charset` 属性表示文档实际使用的字符集；

2. `document.charset` 是 **可写** 的；

3. `document.defaultCharset` 表示操作系统和浏览器默认的字符集，中文操作系统一般是 `gb2312`；

## 自定义数据属性
1. HTML5 规定可以为元素添加非标准的属性，但需要添加前缀 `data-`；

2. 自定义数据属性提供的是与渲染无关，但用于存储不可见的重要数据的属性；

3. 元素相应地新增了 `dataset` 属性来 **访问和编辑** 自定义的属性值
    ```javascript
    var div = document.getElementById('myDiv');

    // 写
    div.dataset.Id = 123;
    div.dataset.name = 'yangfch3';

    // 读
    var Id = div.dataset.Id
    ```

4. **思路**：可以为文档不同位置的元素添加 `data-` 属性，设置加载的百分比，然后该数据可以用于制作加载进度条。

## 插入标记
`innerHTML` `outerHTML` `insertAdjacentHTML()`

### innerHTML
1. 各浏览器返回的 HTML 内容的格式不太一致：部分浏览器会将所有标签转换为大写；不要期望所以浏览器返回的格式相同；

2. 限制（出于安全等方面的考虑）：
    * 通过 innerHTML 直接插入 `<script>` 元素在大多数中并不会执行其中的脚本；
    * 通过 innerHTML 直接插入 `<style>` 在一些浏览器中无效。
    > 在这些浏览器中，这两个元素被认为是“无作用域的元素”（不会在页面上显示的元素），如果通过 innerHTML 插入的字符串开头就是一个“无作用域的元素”，那么这些浏览器就会在解析这个字符串前先删除该元素。**解决方法是：在这些“无作用域的元素”前临时增加一个有作用域的元素，之后再将临时元素删除，同时为脚本元素设置 `defer` 特性**。

3. 部分元素不支持 `innerHTML`

4. **冷知识**：FF 浏览器在 `XHTML` 文档中设置 `innerHTML` 时要求 `XHTML` 必须严格符合要求；

5. **最佳实践**：使用 `innerHTML` 插入代码片段时，请通过特定的方法过滤掉 `script` 标签（可能引入恶意脚本）。

### outerHTML
与 `innerHTML` 的不同点：返回和写入调用它的元素及其所有子节点的 `HTML` 标签

### insertAdjacentHTML() 方法
1. 所有元素新增的方法；

2. 两个参数：插入位置、插入的 HTML 字符串文本

3. 位置参数的取值：
    * `beforebegin`：调用元素之前插入；
    * `afterend`：调用元素之后插入；
    * `afterbegin`：调用元素的第一个子元素之前；
    * `beforeend`：调用元素最后一个子元素之后。

4. 与此同时，还有 `insertAdjacentElement()` 用法与 `insertAdjacentHTML()` 一致，但第二个参数需传入元素节点。

### 内存与性能问题
1. `innerHTML` 等操作会在浏览器底层运行，效率高，但过度使用会使页面性能表现下降；

2. 不要频繁使用，最好将要插入的 HTML 先保存在一个字符串内，再一次性插入。

## scrollIntoView()
1. HTML5 为所有的元素节点新增的方法；

2. 接收一个参数：true|false
    * true：元素顶部与视口顶部平齐；
    * false：元素底部与视口底部平齐。

## 专有扩展
1. IE 文档模式属性：`document.documentMode`；
    > 可以通过 HTML 文档头部的 `meta` `X-UACompatible` 设置

2. `children` 属性
    1. `HTMLCollection` 实例；
    2. 与 `childeNodes` 的不同点是：只包含元素的子节点。

3. `contains()` 方法
    元素的新增方法：一个元素节点参数，检测是否被调用方法的元素包含

4. `DOM Level3` 为元素节点新增方法：`compareDocumentPosition()`
    同样接收一个元素节点作为参数，返回掩码的加和：**1-无关，2-居前，4-居后，8-包含，16-被包含**
    ```javascript
    var result = document.documentElement.compareDocumentPosition(document.body);

    result; // 4+16
    ```

5. `contains()` 兼容代码
    ```javascript
    function contains(refNode, otherNode) {
        if (typeof refNode.contains == 'function') {
            return refNode.contains(otherNode);
        } else if (typeof refNode.compareDocumentPosition == 'function') {
            return !!(refNode.compareDocumentPosition(otherNode) & 16)
        } else {
            var node = otherNode.parentNode;
            do {
                if (node === refNode) {
                    return true;
                } else {
                    node = node.parentNode;
                }
            } while (node !== null);

            return false;
        }
    }
    ```
