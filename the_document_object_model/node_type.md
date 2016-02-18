# Node 基类型
1. **DOM1级** 定义了一个 **Node 接口**，该接口作为 **Node 类型**实现；

    ```javascript
    // 节点类型（nodeType） 1~12
    Node.ELEMENT_NODE; // 1
    Node.ATTRIBUTE_NODE; // 2
    Node.TEXT_NODE; // 3
    ...
    Node.COMMENT_NODE; // 8
    Node.DOCUMENT_NODE; // 9
    ...
    ```
2. JavaScript 中的所有节点类型都继承自 `Node` 类型，所有的节点类型都共享着相同的基本属性和方法（如下面1-[TODO]的属性和方法是所有节点所共有的）；

3. **最佳实践**：对节点进行操作时，最好先使用 `nodeType` 检测一下节点的类型；

### 1. nodeName nodeValue
对于元素节点（`nodeType === 1`），`nodeName` 中保存的是元素标签名的大写，`nodeValue` 的值始终是 `null`

### 2. 节点关系
1. 节点关系可以用传统的家族关系来表示，文档树可以视为家谱；

2. 节点的 `chileNodes` 属性保存的是一个 `NodeList` 对象；
    > `Nodelist` 是一种类数组对象，但不是 `Array` 的实例，用于保存一组有序的节点，因为是基于 `DOM` 结构动态执行查询的结果，所以 `NodeList` 对象是动态变化的（例如 `length` 属性）。

3. `NodeList` 中节点访问方法
    * `[index]`
    * `item(index)`

4. 函数内的 `arguments` 对象使用 `Array.prototype.slice()` 方法可以将其转换为数组，采用同样的方法，也可以将 `NodeList` 对象转换为数组（非 IE 下）。

5. `NodeList` 对象转数组 -- 兼容 IE
    ```javascript
    function convertToArray(nodes) {
        var arr = null;
        try {
            array = Array.prototype.slice.call(nodes, 0); // 针对非 IE
        } catch (e) {
            array = new Array();
            for (var i = 0, len = nodes.length; i < len; i++) {
                array.push(nodes[i]); // 手动枚举，针对 IE 的 NodeList 是 COM 对象
            }
        }
    }
    ```

6. `previousSibling` `nextSibling` `previousElementSibling` `nextElementSibling` `firstChild` `lastChild` `firstElemntChild` `lastElemntChild`
    如果以上属性没有指向，则返回 `null`

7. `cildren` 能很方便的过滤其他节点类型，返回所有元素类型的子节点；

8. `hasChildNodes()` 方法用于检测是否有子节点；

9. `ownerDocument` 属性指向于整个文档的文档节点，每个节点都有这个属性

### 3. 操作节点
1. `appendChild()` 返回的是**新增的节点**
    ```javascript
    var returnedNode = someNode.appendChild(newNode);
    alert(returnedNode === newNode); // true
    alert(someNode.lastChild === newNode); // true
    ```

2. `insertBefore()` 接受两个参数的顺序：要插入的节点和作为参照的节点，返回返回被插入的节点

3. `replaceChild()` `removeChild()` **返回的是被替换、移除的节点**，被替换和移除的节点**仍然为文档所有**，只不过在文档中已经没有了自己的位置；

4. 在不支持子节点的节点上调用上面牵涉到子节点的方法，会导致错误的发生。

### 其他方法
1. `cloneNode()` 创建调用这个方法的节点的一个完全相同的副本，当传入可选布尔参数为 `true` 时，将执行 **深复制**；

2. `cloneNode()` 不会复制添加到 DOM 节点中的 JavaScript 属性，例如事件处理程序；IE 浏览器器会连带事件处理程序一起复制；

3. **最佳实践**：在 `cloneNode()` 之前先移除事件处理程序

4. `normalize()`
    * 作用：处理文档树中的文本节点，它会删除空文本节点，合并相邻的两个文本节点
