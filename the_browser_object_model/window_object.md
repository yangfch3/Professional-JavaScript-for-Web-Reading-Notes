# window 对象
`window` 对象有双重角色

* 通过 `JavaScript` 访问浏览器窗口的一个接口
* `ECMAScript` 规定的 `Global` 对象

### 全局作用域
1. 定义全局变量与在 window 对象上直接定义属性的细微差别：
    * 全局变量不能通过 `delete` 操作删除（部分浏览器执行删除会报错）
    * window 对象上的定义属性可以

    ```javascript
    var name = 'yangfch3';
    window.color = 'white';

    delete window.name; // false 删除失败
    window.name; // undefined

    delete window.color; // true 删除成功
    window.color; // 'white'
    ```

2. 使用 `var` 语句添加的 window 属性有一个名为 `[[Configurable]]` 的特性，这个特性的值被设置为 `false`，导致无法删除；

### 窗口关系及框架
`window` `top` `self` `parent` `top.frames[..]`

1. 每个框架都拥有自己的 window 对象，保存在 frames 集合中；

2. 访问某个框架的 window 对象
    * `top.frames[i]`
    * `top.frames['name']`

3. **最佳实践**：不要使用类似 `window.frames[i]`  的方式来访问某个框架，请使用 `top.frames[i]`
    > `window`、`self` 对象总是指向那个框架的特定实例，`top` 对象始终指向最上层的浏览器窗口

4. 从子框架访问父框架与顶层框架（浏览器窗口）
    * `parent`
    * `self.parent`
    * `top`
    * `self.top`

5. `self` 与 window 可以互换使用；

6. 每个框架都有一套自己的构造函数，并不相等，注意避免跨框架的 `instanceof` 验证操作。

### 窗口位置
`screenLeft` `screenTop` `screenX` `screenY` `moveTo()` `moveBy()`


1. 获取窗口左上角在屏幕上的坐标
    * IE、Safari、Opera、Chrome
        `screenLeft` `screenTop`
    * Safari、Chrome、Firefox
        `screenX` `screenY`

    兼容地获取左上角坐标方法
    ```javascript
    var leftPos = (typeof window.screenLeft == 'number') ? window.screenLeft : window.screenX;
    var topPos = (typeof window.screenTop == 'number') ? window.screenTop : window.screenY;
    ```

2. 移动窗口位置

    `moveTo` `moveBy` 在绝大多数浏览器中只对 `window.open` 弹出的窗口有用，对于正常打开的窗口是默认禁用的
    ```javascript
    var newWin = window.open('', 'github', 'width:300,height=300');

    newWin.opener.location.href; // https://github.com
    newWin.moveTo(300, 300); // ...
    ```

### 窗口大小
1. window 有关属性

    `innerWidth` `innerHeight` `outerWidth` `outerHeight`
2. DOM-body（相当于 body 宽高）

    `document.body.clientWidth`
`document.body.clientHeight`
3. DOM-HTML（相当于 HTML 宽高）

    `document.documentElement.clientWidth` `document.documentElement.clientWidth`
`document.documentElement.clientTop`
`document.documentElement.clientLeft`
4. 调整窗口大小

    `resizeto(widthnum, heightnum)` `resizeby(widthnum, heightnum)`

点击查看大图：
[![2016-02-14_124751.jpg-244.8kB][1]](http://static.zybuluo.com/yangfch3/pd662u71rdtrw676zxugyebj/2016-02-14_124751.jpg)

* `outerWidth` `outerHeight` 返回浏览器窗口本身的尺寸（无论从哪个框架访问）
* `innerWidth` `innerHeight` 返回浏览器页面视图区（包含滚动条）大小
* `resizeby()` `resizeto()` 默认禁用只能在 `window.open()` 弹出的新窗口使用

### 导航和打开窗口
`window.open()`：导航到一个特定的 URL，或者打开一个新的浏览器窗口，接收四个参数：

　　参数一：URL（或 URL 的部分） 字符串

　　参数二（可选）：目标窗口名，可以为已有框架的 `name`

　　参数三（可选）：新窗口特性字符串

　　参数四（可选）：布尔值，是否取代当前页面的浏览器历史记录

1. 参数
    * 参数一：为普通字符串时，默认连接为 `location.href + 'str'`
    ```javascript
    // in window: http://ce.sysu.edu.cn
    window.open('hope', '_blank'); //
    ```

    * 参数二：当该参数为已有窗口或框架的名称时，就会在具有该 `name` 的窗口或框架中加载第一个参数指定的 `URL`
    ```javascript
    // HTML
    <iframe name="embedWin" width="200" height="300"></iframe>

    // JS
    window.open('http://ce.sysu.edu.cn/', 'embedWin');
    ```

    * 参数二：还可以是 `_self` `_parent` `_top` `_blank`
    ```javascript
    // 相当于在当前标签页或新标签页打开连接
    window.open('http://ce.sysu.edu.cn/', '_blank');
    ```

    * 参数三：逗号分隔，描述弹出窗口特性[^1]；没有第三个参数则不会“弹出”新窗口

    * 参数四：只在不打开新窗口，只打开新标签页的情况下使用
        * true，覆盖历史记录
        * false，不覆盖
<br>

2. 安全限制

    弹出窗口被滥用、恶意使用，影响用户体验，所有现代浏览器都对弹窗做了，**安全限制**

3. 检测弹出窗口是否被屏蔽
    * 原理：浏览器内置的屏蔽程序阻止弹出窗口会 `window.open()` 返回 `null`，浏览器插件等程序阻止的弹出窗口 `window.open()` 会返回一个 **错误**，通过检测返回值，`try-catch` 来捕捉错误
    ```javascript
    var blocked = false;

    try {
        var newWin = window.open('http://ce.sysu.edu.cn', '_blank');
        if (newWin == null) {
            blocked = true;
        }
    } catch (e) {
        blocked = true
    }

    if (blocked) {
        alert('The popup was blocked!');
    }
    ```

4. 间歇调用和超时调用
    1. `JavaScript` 是单线程语言；

    2. `setInterval()` `setTimeOut()` 是 window 的方法，随处可以调用，随处可以清除（使用 `clearInterval()`、`clearTimeOut()`）；

    3. `setInterval()` `setTimeOut()` 第一个参数可以是包含 `JavaScript` 代码（语句）的字符串，也可以是一个函数；

    4. 调用 `setTimeOut()` `setInterval()` 的返回值是一个 **超时调用的 ID**，可以用一个变量来存储，并可以通过这个 ID 来取消间歇调用或超时调用（clear...）；

    5. 超时调用的代码都是在全局作用域中执行，函数中的 `this` 都指向 `window` 对象，严格模式下指向 `undefined`

    6. **最佳实践**：第一个参数采用函数的形式，杜绝采用代码字符串的形式

        ```javascript
        // not good
        setInterval('alert("Hello world!")', 1000);

        // good
        setInterval(function() {
            alert('Hello world!');
        }, 1000);
        ```

    7. **最佳实践**：尽量少地使用间歇调用，真的要使用则需要避免间歇调用的叠加，可以使用重复的超时调用来模拟间歇调用。

        ```javascript
        // Interval exec
        var num = 0,
            max = 10,
            intervalId;

        function incrementNumber() {
            num++;

            if (num === max) {
                clearInterval(intervalId);
                alert('done');
            }
        }

        intervalId = setInterval(incrementNumber, 500);

        // timeOut exec
        var num = 0,
            max = 10;

        function incrementNumber() {
            num++;

            if (num < max) {
                setTimeout(incrementNumber, 500);
            } else {
                alert('Done');
            }
        }

        setTimeout(incrementNumber, 500);
        ```

5. 系统对话框
    1. `alert()` `confirm()` `prompt()` 对话框外观由操作系统和浏览器决定；

    2. 都接收一个字符串参数，`prompt()` 可接收两个字符串参数，一个提问，一个默认值；

    3. `confirm()` 返回布尔值；`prompt()` 返回用户提交的输入字符串

6. 查找与打印功能
    1. `windo.print()` -- 查找

    2. `window.find(strArg)` -- 打印

    3. 这两个操作都是异步显示的，不影响脚本的继续执行

<br>
<br>
<br>
<br>
<br>


  [1]: http://static.zybuluo.com/yangfch3/pd662u71rdtrw676zxugyebj/2016-02-14_124751.jpg


[^1]: 新窗口 Feature 参数列表.
|设置|值|说明|
|::|::|:--|
|fullscreen|yes 或 no|仅IE，是否最大化|
|height、width|数值|新窗口宽高|
|top、left|数值|新窗口的上、左坐标|
|location|yes 或 no|是否在新窗口显示地址栏|
|menubar|yes 或 no|是否显示菜单栏，默认 no|
|resizable|yes 或 no|是否可以拖动改变窗口大小，默认 no|
|scrollbars|yes 或 no|是否允许滚动，默认 no|
|status|yes 或 no|是否显示状态栏，默认 no|
|toolbar|yes 或 no|是否娴熟工具栏，默认 no|
