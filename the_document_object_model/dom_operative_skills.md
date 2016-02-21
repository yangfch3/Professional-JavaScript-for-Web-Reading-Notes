# DOM 操作技术
DOM 操作最大的两个麻烦是：操作效率问题、浏览器兼容问题

## 动态脚本
1. 通过修改 DOM 动态添加脚本的两种模式
    * 插入外部文件
    * 插入 `JavaScript` 代码

2. 插入外部文件
    ```javascript
    var script = document.createElement('script');
    script.src = 'client.js';
    document.body.appendChild(script);
    ```

3. 封装为 **外部脚本插入函数**
    ```javascript
    function loadScript(url) {
        var script = document.createElement('script');
        script.src = url;

        document.body.appendChild(script);
    }

    loadScript('script.js')
    ```

4. 插入行内代码：不兼容 IE 8-
    IE 8- 将 `script` 视为一个特殊的元素，不允许 `DOM` 访问其子节点
    ```javascript
    var script = document.createElement('script');
    var textNode = document.createTextNode('function sayHi() {alert("Hi!");}');
    script.appendChild(textNode);
    // or script.innerHTML = '...'

    document.body.appendChild(script);
    ```

5. 插入行内代码的兼容性解决方案：**行内脚本插入封装函数**
    ```javascript
    function loadScript(code) {
        var script = document.createElement('script');

        try {
            script.appendChild(document.createTextNode(code));
        } catch (err) {
            script.text = code;
        }

        document.body.appendChild(script);
    }

    loadScript('function sayHi() {alert("Hi!");}');
    ```

6. 以以上两种模式加载的脚本代码会立刻执行；

7. 虽然可以，但不推荐使用 `eval()` 执行字符串代码。

## 动态样式
1. 通过 DOM 动态添加样式的两种模式
    * `<link>` 引入外部样式
    * `<style>` 嵌入样式

2. **插入外部样式文件封装函数**
    ```javascript
    function loadStyles(url) {
        var head = document.getElementsByTagName('head')[0];
        var link = document.createElement('link');
        link.rel = 'stylesheet';
        link.href = url;
        head.appendChild(link);
    }

    loadStyles('styles.css');
    ```

3. 插入行内样式的兼容性解决方案：**行内样式插入封装函数**
    ```javascript
    function loadInlineStyle(css) {
        var head = document.getElementsByTagName('head')[0];
        var style = document.createElement('style');

        try {
            style.appendChild(document.createTextNode(css));
        } catch (err) {
            style.styleSheet.cssText = css;
        }

        head.appendChild(style);
    }

    loadInlineStyle('body{background-color: red;}');
    ```

4. IE 8- 将 `<style>` 视为一个特殊元素的、与 `<script>` 类似的节点，不允许访问其子节点，所以需要写入内联的样式代码，需要使用 IE 8- 下 `style` 节点的特征属性：`style.styleSheet.cssText`。

## 操作表格
1. `HTML DOM` 为了方便操作表格，为 `<table>` `<stbody>` `<tr>` 元素添加了特殊的属性和方法；

2. **为 `<table>` 元素添加的属性和方法**
    * `caption`：返回表格的 `caption` 元素节点，没有则返回 `null`；

    * `tHead` `tBodies`、`tFoot`：返回表格 `<tHead>` `<tbody>` `<tfoot>` 元素（一表一个）；

    * `rows`：返回元素所有行 `<tr>` 元素的 `HTMLCollection`；

    * `createTHead()` `<createTFoot>` `<createCaption>`：创建 `<thead>` `<tfoot>` `<caption>` 空元素，将其放到表格中，返回创建的 `<thead>` `<tfoot>` `<caption>` 元素节点；

    * `deleteTHead()` `<deleteTFoot>` `<deleteCaption>`：删除 `<thead>` `<tfoot>` `<caption>` 空元素，无返回值（或返回值为 `undefined`）；

    * `deleteRow(_pos_)`：删除指定位置（注意参数不是索引，而是从 0 开始的位置）的行，返回 `undefined`；

    * `insertRow(_pos_)`：向 rows 集合中的指定位置（不是索引）插入一行。

3. **为 `<tbody>` 元素添加的属性和方法**
    * `rows`：返回 `<tbody>` 元素下所有行 `<tr>` 元素的 `HTMLCollection`；

    * `deleteRow(_pos_)`：删除指定位置（注意参数不是索引，而是从 0 开始的位置）的行，返回 `undefined`；

    * `insertRow(_pos_)`：向 rows 集合中的指定位置（不是索引）插入一行。

4. **为 `<tr>` 元素添加的属性和方法**
    * `cells`：返回 `<tr>` 元素中单元格的 `HTMLCollection`；

    * `deleteCell(_pos_)`：删除指定位置（不是索引）的单元格；

    * `insertCell(_pos_)`：向 `cells` 集合中的指定位置（不是索引）插入一个单元格，返回对新插入单元格的引用。

5. 表格便捷属性和方法创建 `table` 实例：
    ```javascript
    // 创建 table
    var table = document.createElement('table');
    table.boder = 1;
    table.width = '100%';

    // 创建表格标题
    var caption = table.createCaption();
    caption.innerHTML = 'My Table';

    // 创建 tbody
    var tbody = document.createElement('tbody');
    table.appendChild(tbody);

    // 创建第一行
    var row01 = tbody.insertRow(0);
    var cell0101 = row01.insertCell(0),
        cell0102 = row01.insertCell(1),
        cell0103 = row01.insertCell(2);

    cell0101.appendChild(document.createTextNode('cell 1,1'));
    cell0102.appendChild(document.createTextNode('cell 1,2'));
    cell0103.appendChild(document.createTextNode('cell 1,3'));

    // 创建第二行
    var row02 = tbody.insertRow(1);
    var cell0201 = row01.insertCell(0),
        cell0202 = row01.insertCell(1),
        cell0203 = row01.insertCell(2);

    cell0201.appendChild(document.createTextNode('cell 2,1'));
    cell0202.appendChild(document.createTextNode('cell 2,2'));
    cell0203.appendChild(document.createTextNode('cell 2,3'));

    // 将表格插入到文档主题中
    document.body.appendChild(table);
    ```

## 使用 NodeList
理解并区别三个概念：`NodeList` `NamedNodeMap` `HTMLCollection`

1. 上面三个集合都是动态的，**所有以上集合都是在访问 `DOM` 文档时实时运行的查询**；

2. 动态性实例：**每一次访问的都是更新后的集合**
    ```javascript
    var divs = document.getElementsByTagName('div'),
        i,
        div;

    for (var i = 0; i < divs.length; i++) {
        div = document.createElement('div');
        document.body.appendChild(div);
    }

    // 页面挂掉了...
    ```

3. **最佳实践**：防止出现 2 中情形的方案是，将 `NodeList` 的 `length` 缓存到一个变量中
    ```javascript
    var divs = document.getElementsByTagName('div'),
        i,
        len,
        div;

    for (var i = 0, len = divs.length; i < len; i++) {
        div = document.createElement('div');
        document.body.appendChild(div);
    }

    // 页面不会挂掉了...因为 len 是不会变的
    ```

4. **最佳实践**：减少访问 `NodeList` 的次数，每次访问都会重新查询一次。
