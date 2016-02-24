# 调试技术
1. 大多数浏览器都有 `console` 对象，用于向 `JavaScript` 控制台写入消息；

2. `error(msg)` `info(msg)` `log(msg)` `warn(msg)`；

3. **冷知识**：部分浏览器支持使用 `LiveConnect` -- 在 `JavaScript` 中运行 `Java` 代码：
    ```javascript
    java.lang.System.out.println('Your message');
    // 效果等同 console.log() 或 opera.postError()
    ```

4. `log` 函数多浏览器兼容封装
    ```javascript
    function log(msg) {
        if (typeof console == 'object') {
            console.log(msg);
        } else if (typeof opera == 'object') {
            opera.postError(msg);
        } else if (typeof java == 'object' && typeof java.lang == 'object'){
            java.lang.System.out.println(msg);
        }
    }
    ```

5. 另一输出调试的方法：在页面开辟一小块区域，用于显示调试消息（方便用于不支持 `JavaScript` 控制台的浏览器）
    ```javascript
    function log(msg) {
        var console = document.getElementById('debuginfo');
        if (console === null) {
            console = document.createElement('div');
            console.id = 'debuginfo';
            console.style.background = '#dedede';
            console.style.border = '1px solid sliver';
            console.style.padding = '5px';
            console.style.width = '400px';
            console.style.position = 'absolute';
            console.style.right = '0px';
            console.style.top = '0px';

            document.body.appendChild(console);
        }

        console.innerHTML += '<p>' + msg + '</p>';
    }
    ```

6. 在运行函数之前，为了达到理想的效果要对传入的参数进行检查，如不符合要求时要主动抛出错误信息；

7. 抛出自定义错误的 `assert()` 函数封装（大型库比较常见）：
    ```javascript
    function assert(condition, message) {
        if (!condition) {
            throw new Error(message);
        }
    }

    function divide(num1, num2) {
        assert(typeof num1 == 'number' && typeof num2 == 'number', 'divide(): Both arguments must be numbers.');

        return num1/num2;
    }
    ```
