# 其他类型
## Comment 类型
1. `nodeType` 为 8，`nodeName` 为 `#comment`，`nodeValue` 为注释内容；

2. `Comment` 类型拥有除 `splitText()` 之外的所有的属性和方法；

3. 创建注释节点只需要传入注释内容即可
    ```javascript
    document.createComment('Hi')
    ```

4. 一般浏览器不会识别位于 `</html>` 后面的注释；

5. 在 IE 8 中，注释节点被视作标签名为 `"!"` 的元素，注释节点可以使用 `getElementsByTagName('!')` 取得。

## CDATASection 类型
1. `nodeType` 为 4，`nodeName` 为 `#cdata-section`，`nodeValue` 为 `CDATA` 区域的内容；

2. `CDATASection` 类型针对的是 `XML` 文档，表示的是 `CDATA` 区域，在 HTML 文档中会错误的把 `CDATA` 区域解析为 `Comment` 或 `Element`；

3. 针对 `XML` 文档，可以使用 `document.createCDataSection()` 来创建 `CDATASection` 类型对象，接收一个参数：`CDATA` 区域的内容；

4. `CDATASection` 类型继承自 `Text` 类型，除了 `splitText()` 方法外所有的属性和方法都拥有。

## DocumentType 类型
1. `nodeType` 为 0；`ndoeName` 为 `doctype` 的名称，如 `html`；`nodeValue` 为 `null`；

2. 不能动态的创建，只能由 `html` 文档规定；

3. `name` `entities` `notations` 属性；

4. 使用 `document.doctype` 访问 `DocumentType` 对象

## DocumentFragment 类型
1. 文档碎片；

2. `DocumentFragment` 节点类型在文档中没有对应标记，它是游离于文档之外的；

3. `DocumentFragment` 类型是一种 **轻量级** 的文档（类似完整的 HTML 文档），可以包含和控制节点，但不会像完整的文档那样占用额外的资源；

4. `nodeType` 为 11；`nodeName` 为 `#document-fragment`；`nodeValue` 为 `null`；可以包含任意类型的子节点；

5. `DocumentFragment` 类型一般作为一个节点 `仓库` 来使用，可以在里面保存将来可能添加到文档的节点，然后 `DocumentFragment` 整体插入到文档内；

6. `DocumentFragment` 类型继承了 `node` 类型的所有方法；

7. 创建：`document.createDocumentFragment()`

8. 文档碎片可以通过 `appendChild(DocFrag)` `insertBefore(DocFrag)` 来插入文档，此时文档碎片便成为了文档的一部分

## Attr 类型
1. 元素的特性在 DOM 中以 Attr 类型来表示；

2. 特性就是存在于元素的 `attributes` 属性对象中的节点；

3. 三个属性：`name` `value` `specified`；

4. 创建特性节点：`document.createAttribut(attrName)`；
    ```javascript
    var attr = document.createAttribute('align');
    attr.nodeValue = 'left'; // or use .name

    element.setAttribute(attr);
    ```

5. 新创建的特性节点需要添加到元素上，使用元素的 `setAttribute(attrName)` 方法；

6. **不建议直接访问元素的特性节点（`getAttributeNode()` 等），而使用 `getAttribute()` `setAttribute()` `removeAttribute()` 来访问和操作元素的特性。**
