# Document 类型
区别： `Document` 类型、`HTMLDocument` 类型、`document` 对象
    > `Document` 表示文档，这里的文档可以是 HTML 文档，也可以是 `XML` 文档，换句话说 `Document` 类型能表示 `HTML` 和 `XML` 等文档；
    >
    > `HTMLDocument` 对象继承自 `Document` 对象，专用于表示 `HTML` 文档；
    >
    > `docuemnt` 对象是 `HTMLDocument` 对象的一个实例，表示整个 `HTML` 页面，又叫做页面的根节点。

`Document` 对象（根节点）特征
```javascript
var doc = document;

doc.nodeType; // 9
doc.nodeName; // '#document'
doc.nodeValue; // null
doc.parentNode; // null
doc.ownerDocument; // null
doc.childNodes; // [<!DOCTYPE html>, <html>......</html>]
```

### 1. 文档的子节点
1. 使用 `documentElement` 属性快捷访问页面的 `<html>` 元素；

2. `body` 属性直接指向 `<body>` 元素；

3. `doctype` 属性可在部分浏览器下访问文档类型声明；各个浏览器对于文档类型声明的支持方式存在较大差异：
> IE8-：文档类型声明被解释为一个注释，当做一个 `Comment` 节点，`document.doctype` 返回值为 `null`
    >
    >大部分现代浏览器：文档类型声明作为文档的第一个子节点，是一个 `DocumentType` 节点，可以通过 `docuemnt.doctype` `document.childNodes[0]` `document.firstChild` 访问

4. 各个浏览器对 `<html>` 元素外注释的解析方式的不同
    ```javascript
    <!-- 第一条注释 -->
    <html>
        <body>
        </body>
    </html>
    <!-- 第二条注释 -->
    ```
    部分浏览器会为两个注释都建立注释节点，部分浏览器会忽略第二条，只为第一条创建注释节点，大部分浏览器现在回完全忽略这 2 条注释；

5. 不要在 `document` 对象上调用 **增删替换** 节点的操作，请到 `<html>` 元素节点上调用这些方法。

### 2. 文档信息
1. `title` 属性用于便捷读写 `title` 标签；

2. `URL` 属性只可读，`referrer` 属性也只可读；

3. `domain` 属性可写，但不能设置为当前 URL 中不包含的域，通过设置 `domain` 属性相同可实现框架间不同子域页面的 `JavaScript` 通信
    ```javascript
    // example.com
    document.domain = 'example.com';

    // b.example.com as a frame in example.com
    document.domain = 'example.com';

    // 可以使用程序从 example.com 访问
    do sth Cross-domain
    ```

4. `document.domain` 只能往上级域名方向设置，设置为非上级域名会报错，不能设置为顶级域名
    ```JavaScript
    // a.example.com
    document.domain = 'example.com'; // 'example.com'
    document.domian = 'b.example.com'; // 'Uncaught DOMException: ...'
    ```

### 3. 查找元素
1. `getElementById()` 注意事项：
    * 传入参数对应的 ID 的元素不存在，返回 null
    * 传入参数大小写需与实际元素 id 特性严格匹配
    * 多个元素的 id 特性相同，方法返回文档中第一次出现的那个

2. `getElementsByTagName()` 方法返回的是 `HTMLCollection` 对象；

3. `HTMLCollection` 对象类似 `NodeList` 对象；

4. `HTMLCollection` 对象可以使用 `namedItem()` 方法通过元素的 `name` 取得特定的项，或者更简单的，直接 **方括号 + name** 访问特定元素
    ```javascript
    var images = document.getElementsByTagName('img');

    images[0].src;
    images.item(0).src;
    images.namedItem('myImage');
    images['myIname'];
    ```

5. `getElementsByTagName()` 传入“*”可以获得文档中的所有元素；
    > 旧版本 IE 将注释（Comment）实现为元素（Element），在 IE 中使用 `getElementsByTagName()` 查询会返回注释节点

6. `getElementsByName()` 常用于取得表单元素；

7. `getElementsByClassName()` 在 IE8 及以下不支持。

### 4. 特殊集合
其他作为 `document` 对象属性的 `HTMLCollection` 对象
```javascript
document.anchors;
document.forms;
document.images;
document.links;
```

### 5. DOM 一致性检测
浏览器实现 DOM 的范围有多有少，`document` 对象的属性 `document.implementation` 是提供相应的信息和功能的对象，该对象在 DOM 1 级的规范下规定了一个方法：`hasFeature()`，接受两个参数：要检测的 DOM 功能的名称和版本号，返回值为布尔值

```javascript
var hasXmlDom = document.implementaction.hasFeature('XML', '1.0');
```

限制：只是检测支不支持（部分） DOM 功能（并且是由浏览器产商自己说了算的），不一定遵守规范

### 6. 文档写入
`wirte(str)` `writeln(str)` `open()` `close()`

1. `writeln()` 与 `write()` 的不同是会在字符串的末尾添加一个换行符（`\n`）；

2. `write()` 和 `writeln()` 用于动态地包含外部脚本时，需要对 `</script>` 做转移处理，否则会导致字符串被解释为脚本块执行，后面的代码无法执行；正确写法如下：
    ```javascript
    <script type="text/javascript">
        document.write('<script type=\"text/javascript\" src=\"file.js\">' + '<\/script>')
    </script>
    ```

3. <span style="color:red">在文档加载结束后调用 `document.write()` 输出的内容会重写整个页面；</span>

4. `open()` `close()` 分别用于打开和关闭网页的输出流；

5. **最佳实践**：避免使用 `document.write()`
