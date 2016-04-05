# HTML 事件处理程序
1. 某个元素支持的所有事件都可以使用一个与相应事件处理程序同名的 `HTML` 特性来指定；
    ```HTML
    <input type="button" value="click me" onclick="alert('Clicked')" />
    ```

2. 在这个特性里，不能使用未经转义的 HTML 语法字符：`&` `"`、`<`、`>`；
    ```HTML
    <input type="button" value="click me" onclick="alert(&quot;Clicked&quot;)" />
    ```

3. 特性值也可以是调用页面中其他地方定义的脚本，HTML 事件处理程序在执行时 **有权访问全局作用域中的任何代码**；
    ```HTML
    <input type="button" value="click me" onclick="showMessage()" />
    ```

4. HTML 事件处理程序内部可以**通过 `event` 变量直接访问事件对象，无需自己定义， 也不用从函数的参数列表读取；**

5. **`HTML` 事件处理程序中的 `this` 等于事件的目标元素**；
    ```javascript
    <input type="button" value="click me" onclick="alert(this.value)">
    // 'click me'
    ```

6. HTML 事件处理程序有着独特的扩展作用域：处理函数内部可以直接访问 元素本身、 `document` 元素，类似使用 `with(this)`、`with(document)` 进行扩展一样；
    ```javascript
    <form method="post">
        <input type="text" name="username" value="">
        <input type="button" onclick="alert(username.value)">
        // === alert(document)
    </form>
    ```

7. 删除 HTML 事件处理程序的方法：设置事件处理程序对应的 HTML特性值为 `null`；

8. HTML 事件处理程序的三个缺点
    * 时差问题导致错误：当用户点击你的事件元素时你的处理函数可能还未准备好，**例如页面上部的处理程序调用了还未加载的脚本程序**；
    * 扩展事件处理程序的作用域链在不同浏览器中会有差异；
    * HTML 事件处理程序与 HTML 紧密耦合，不便于修改。
