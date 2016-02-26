# Element 类型
1. `Element` 节点是 `Element` 类型的实例；

2. `Element` 节点的 `nodeName` 与 `tagName` 属性返回元素的标签名 **大写**；`nodeValue` 为 `null`；

3. **最佳实践**：在比较标签名与字符串时，最好先将标签名转换为全小写。

### 1. HTML 元素
1. 所有 `HTML` 元素都由 `HTMLElement` 类型表示，**`HTMLElement` 类型继承自 `Element` 类型并添加了一些属性和方法**；
    > 所有 `HTML` 元素都是由 `HTMLElement` 或者其子类型表示的，每个 `HTML` 元素都由与之关联的类型，如：`HTMLAnchorElement` `HTMLBodyElement` `HTMLDivElement` ...，这些类型都继承 `HTMLElement` 类型的属性和方法，也有着自己的拓展

2. 大部分 `HTML` 标签的原生特性作为 `Element` 节点对象的属性都可以直接通过 `.` 或 `[]` 操作符 **读写**，但 **无法读写自定义的特性（见下面的取得特性一节）**
    ```javascript
    var div = document.getElementById('myDiv');

    div.id;
    div.className;
    div.name;
    ...
    div.DIYAttri; // undefined
    div['DIYAttri']; // undefined
    ```

3. `class` 为 `ES` 的保留字，使用 `.` 或 `[]` 访问元素类名需要使用 `className`。

### 2. 取得特性
`getAttrigute()` `setAttribute()` `removeAttribute()` 三个特性操作的方法

1. 传递给 `getAttribute()` 的特性名字符串与实际的特性名相同，如：要获取 `class` 的特性值，应该传入 `'class'`，而不是 `'className'`；

2. **通过 `getAttribute()` 方法可以取得自定义特性**
    ```javascript
    var div = document.getElementById('myDiv');

    div.getAttribute('DIYAttri');
    ```

3. **最佳实践**：根据 `HTML5` 的规范推荐，自定义特性应该加上 `data-` 前缀以便验证；

4. 补充上节 No.2
    >IE 会为定义特性也创建属性，可以直接使用 `.` 和 `[]` 直接访问自定义属性

5. 用 `getAttribute()` 方法与 `.` 法访问节点的 `style` 属性的返回值不同，`.` 法返回一个 `CSSStyleDeclaration` 对象， `getAttribute()` 方法返回字符串
    ```javascript
    var ele = $('#gs_st0');

    ele.style; // CSSStyleDeclaration {0: "line-height", ...}
    ele.getAttribute('style'); // "line-height: 38px;"
    ```

6. 用 `getAttribute()` 方法与 `.` 法访问节点的 `onclicke` 属性的返回值不同，使用 `getAttribute()` 方法返回字符串，使用 `.` 等直接访问返回一个函数（或 `null`）
    ```javascript
    var ele = $('#gs_st0');

    ele.onclick; // function () {alert("Hi!")}
    ele.getAttribute('onclick'); // "alert("Hi!")"
    ```

7. **最佳实践**：只在获取标签的自定义特性值的情况下才使用 `getAttribute()` 方法，其余情况直接访问属性。

### 3. 设置特性
1. `setAttribute()` 接受两个字符串参数：要设置（或新建）的特性名和值；

2. `setAttribute()` 方法可以操作自定义特性；

3. `setAttribute()` 设置的特性名会自动转换为小写；

4. **最佳实践**：只在设置标签的自定义特性值的情况下才使用 `setAttribute()` 方法，其余情况直接操作属性；

5. `removeAttribute()` 用于彻底删除元素的特性和值，包括自定义特性。

### 4. attributes 属性
1. `attributes` 属性是 `Element` 类型独有的；

2. `attributes` 属性是一个 `NamedNodeMap` 对象，类似 `NodeList`，是由特性节点组成对象；

3. `NamedNodeMap` 对象中的每一项又是一个单独的特性节点，有 `nodeName` `nodeValue` 等属性；

4. `NamedNodeMap` 对象拥有的方法
    ```javascript
    var ele = $('#gs_st0');

    // item()
    ele.attributes.item(1); // id="gs_st0"

    // getNamedItem()
    ele.attributes.getNamedItem('id'); // id="gs_st0"
    ele.attributes.getNamedItem('id').nodeValue; // "gs_st0"

    // removeNamedItem()
    ele.attributes.removeNamedItem('class'); // class="gsst_b_sbib_c"

    // setNamedItem()
    var newAttr = document.createAttribute('newAttr')
    newAttr.nodeValue = '123';
    ele.attributes.setNamedItem(newAttr);
    ```

5. 使用 `attributes` 对象来操作元素节点的特性与直接使用 `getAttribute` `setAttribute` `removeAttribute` 效果一致，`attributes` 的方法总能找到别的简单的替代者；

6. **注意点**
    * 针对 `attributes` 对象中的特性，不同浏览器返回的顺序不同；
    * `IE 7` 会将没有指定值的特性一并返回，需要使用 `NamedNodeMap` 中节点的 `specified` 属性检测是否为已定义特性。
    ```javascript
    ele.attributes[1].specified; // true or false
    ```

7. **最佳实践**：只使用 `attributes` 属性对象来遍历元素的特性。
    ```javascript
    function outputAttributes(element) {
        var attrs = new Array(),
            attrName,
            attrValue,
            i,
            len;

            for (i = 0, len=element.attributes.length; i < len; i++ ) {
                attrName = element.attributes[i].nodeName;
                attrValue = element.attributes[i].nodeValue;
                if (element.attributes[i].specified) {
                    attrs.push(attrName + '=\"' + attrValue + '\"');
                }
            }

            return attrs.join(" ");
    }

    var ele = $('#gs_st0');
    outputAttributes(ele);
    ```

### 5. 创建元素
1. `document.createElement()` 传入参数为标签名（小写）；
    ```javascript
    var div = document.createElement('div');
    div.className = 'div';
    div.innerHTML = 'Hi!';
    document.body.appendChild(div);
    ```

2. 可以创建自定义的非标准标签，例如：`document.createElement('abc');`；

3. 在 `IE` 8 及以下可以传入整个元素标签，包括属性、content 等，其他浏览器不支持；
    兼容性写法
    ```javascript
    // IE7-
    if (document.documentMode && document.documentMode <= 8) {
        var div = document.createElement('<div class=\"demo\">Content</div>');
    } else {
        var div = document.createElement();
        div.className = 'demo';
        div.innerHTML = 'Content';
    }
    ...
    ```

### 6. 元素的子节点
1. 不同的浏览器对待元素间的空白符的解析不同。低版本 IE 会忽略元素间的无用空白符，其他浏览器会将空白符视作一个文本节点；

2. 需要对元素的 `childNodes` 进行操作时，通常先检测一下 `nodeType` 属性：
    ```javascript
    for (var i = 0, len = element.childNodes.length; i++) {
        if (element.childNodes[i].nodeType == 1) {
            // do something
        }
    }
    ```

### 7. 元素尺寸与位置
<span style="display:block; text-align:center">
![client&outer.jpg-51.9kB][1]
</span>

<span style="display:block; text-align:center">
![scroll.JPG-41.1kB][2]
</span>

  [1]: http://static.zybuluo.com/yangfch3/0arkpkfnxsu06vk3akwxq5m0/client&outer.jpg
  [2]: http://static.zybuluo.com/yangfch3/gy5e83d28kvn6s4hg9jemvtb/scroll.JPG
