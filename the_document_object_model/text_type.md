# Text 类型
1. `nodeType` 为 3，`nodeName` 为 `#text`；

2. 不支持子节点；

3. 可以包含转义后的 HTML 字符；

4. 文本节点的值可以通过 `nodeValue` 属性或 `data` 属性访问；
    ```javascript
    var text = document.createTextNode('Hello yangfch3!');

    text.nodeType; // 3
    text.nodeName; // #text
    text.nodeValue; // 'Hello yangfch3!'
    text.data; // 'Hello yangfch3!'
    text.nodeValue.length; // 15
    text.data.length; // 15
    ```

5. 操作文本节点的方法
    * `appendText(text)`
    * `deleteDate(offset, count)`
    * `insertData(offset, count)`
    * `replaceData(offset, count, text)`
    * `splitText(offset)`
    * `substringData(offset, count)`

6. 默认情况下，每个包含内容的元素最多只能有一个文本节点，而且必须有内容存在；

7. 创建和修改文本节点时，字符串会经过 `HTML`（或`XML`）编码转义，防止对原文档造成解析错误；
    ```javascript
    div.firstChild.nodeValue = "<p>Hello world!</p>"; // "&lt;p&gt;Hello world!&lt;/p&gt;"
    ```

### 创建文本节点
1. `document.createTextNode()`；

2. 可以人为使一个元素包含多个文本节点，虽然多个文本节点的表现形式与单个节点的表现形式没什么两样；
    ```javascript
    var element = document.createElement('div');
    element.className = 'message';

    var textNode = document.createTextNode('Hello world!');
    element.appendChild(textNode);

    var anotherTextNode = document.createElement('Yippee!');
    element.appendChild(anotherTextNode);

    document.body.appendChild(element);
    ```

### 规范化文本节点
元素节点的方法：`element.normalize()` 方法用于合并元素内的多个文本节点；

### 分隔文本节点
Text 类型的 `splitText()` 方法接收一个参数：分割的位置索引，将一个文本节点分为两个，**同时返回后一个新文本节点**
```javascript
var textNode = document.createTextNode('Hello world!');
var newNode = textNode.splitText(5);
textNode; // 'Hello'
newNode; // ' world!'
```

**一般用于从文本节点提取数据！**
