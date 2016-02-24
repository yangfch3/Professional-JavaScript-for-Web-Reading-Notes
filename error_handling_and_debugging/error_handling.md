#错误处理
## try-catch 语句
1. 与 `Java` 中的 `try-catch` 语句完全相同；
    ```javascript
    try {
        // 可能会导致错误的代码
    } catch(err) {
        // 在错误发生时怎么处理，错误对象会作为参数传入
    }

    ```

2. `try` 块中的任何代码发生了错误，就会立即退出代码执行过程，转而传入错误对象，执行 `catch` 块；

3. 错误对象最常用最被浏览器广泛支持的属性：`message` 属性用于显示错误的信息，**不同浏览器下错误信息一般不同**；

4. 各浏览器对错误对象属性的扩展
    * IE 添加了 `description`、`number` 属性
    * FF 添加了 `fileName` `lineNumber` 和 `stack` 属性
    * Safari 添加了 `line` `sourceId` 和 `sourceURL` 属性

5. **最佳实践**：跨浏览器时，最好只使用 `message` 属性；

6. 当 `try-catch` 语句发生错误时，浏览器会默认错误已经被处理，因而浏览器不会抛出或记录错误；

7. **最佳实践**：调用大型函数库的函数时，无法修改源码且无法预料错误，使用 `try-catch` 包裹；

8. 勿滥用 `try-catch`。

## finally 子句
1. `finally` 子句在 `try-catch` 语句中是可选的；

2. `finally` 子句一经使用，其代码无论如何都会执行，即使 `try-catch` 语句中包含 `return` 语句；
    ```javascript
    try {
        // ...
    } catch (error) {
        // ...
    } finally {
        // ...
    }
    ```

3. 示例：`return` 的是什么
    ```javascript
    try {
        return 1;
    } catch (err) {
        return 2;
    } finally {
        return 3;
    }
    // > 3
    ```

4. **注意点**：不轻易使用 `finally` 子句，除非你很清楚你想让代码做什么。

## 错误类型
1. 七种错误类型
    * `Error`
    * `EvalErroe`
    * `RangeError`
    * `ReferenceError`
    * `SyntaxError`
    * `TypeError`
    * `URIError`

2. `Error` 类型是其他错误类型的基类型，其他错误类型都继承自这个类型；

3. `Error` 基类型错误很少见，一般的错误为其他的具体错误类型，这个基类型的主要目的是 **供开发人员抛出自定义错误**；

4. `EvalError` 错误：在使用 `eval()` 函数而发生异常（没有把 `eval()` 当成函数调用）时被抛出；

5. 由于平时极少使用 `eval()`，所以 `EvalError` 极少见；

6. `RangeError` 错误：数值超出范围时触发
    ```javascript
    var items = new Array(-20); // RangeError
    var items = new Array(Number.MAX_VALUE); // RangeError
    ```

7. `ReferenceError` 错误：找不到对象或引用时会抛出 `RangeError` 错误，十分常见；

8. `SyntaxError` 错误：语法错误；

9. `TypeError` 错误：变量中保存着意外的类型，访问不存在的方法时会发生，本质是执行特定于类型的操作时，变量的类型并不符合要求所导致
    ```javascript
    var o = new 10; // TypeError

    alert('name' in true); // TypeError

    Function.prototype.toString.call('name'); // TypeError
    ```

10. 最常发生 `TypeError` 错误的情况：传递给函数的参数事先未经过检查，传入类型与预期类型不相符；

11. `URIError` 错误：在使用 `encodeURI()` 或 `decodeURI()` 解析格式错误的 URI 时出现；

12. 根据错误类型执行特性的操作
    ```javascript
    try {
        // ...
    } catch (err) {
        if (err instanceof TypeError) {
            // 处理类型错误
        } else if (err instanceof ReferenceError) {
            // 处理引用错误
        } else {
            // 处理其他错误
        }
    }
    ```

## 抛出错误
1. `throw` 操作符用于抛出错误；

2. `throw` 操作符后可接任意值，但抛出 **错误实例** 才有意义；
    ```javascript
    throw new Error('Something bad happend.');
    throw new SyntaxError('I don\'t like your syntax.');
    throw new TypeError('Type error!');
    ...
    ```

3. `throw` 抛出的 **错误实例** 与浏览器本身抛出的 **错误类型** 没有本质上的区别；
    ```javascript
    try {
        throw new TypeError('Type Error!');
        // 构造函数传入的参数会作为错误对象的 message
    } catch (err) {
        console.log(err.message);
    }
    ```

4. **创建自定义错误的构造函数**：
    ```javascript
    function CustomError(message) {
        this.name = 'CuntomError';
        this.message = message;
    }

    CuntomError.prototype = new Error();

    throw new CuntomError('My cuntom error message');
    // Uncaught CustomError {name: "CuntomError", message: "My cuntom error message"}
    ```

5. 重点关注函数，反复考虑可能导致函数执行失败的因素：
    ```javascript
    function process(values) {
        if (!(values instanceof Array)) {
            throw new Error('Process(): Argument must be an array.');
        }

        values.sort();
        // 先在 try-catch 块中检验了数组，再调用数组方法

        for (var i = 0, len = values.length; i < len; i++) {
            if (values[i] > 100) {
                return values[i];
            }
        }

        return -1;
    }

    process('str');
    // Uncaught Error: Process(): Argument must be an array.(…)
    ```

6. **最佳实践**：只应该捕获那些你确切知道该如何处理的错误。

## 错误事件
1. **任何没有通过 `try-catch` 处理的错误都会触发 `window` 对象的 `error` 事件**；

2. 要指定 `noerror` 事件处理程序，必须**使用 “DOM 0 级”**技术，`onerror` 事件没有遵循 “`DOM 2` 级事件”的标准格式，**接受三个参数：错误信息、错误 url、错误行数**，但 **不会传入错误对象本身**，写法如下：
    ```javascript
    window.onerror = function (message, url, line) {
        alert(message);
    }
    ```

3. 避免脚本出错时代码被迫中断无法继续往下执行的无奈的方法，只要可能就不应该使用下面的方法：
    ```javascript
    window.onerror = function (message, url, line) {
        console.log(message);
        return false;
    }
    ```

4. 3 中的方法相当于充当了整个文档的 `try-catch` 语句，部分浏览器在使用 3 中的方法使得脚本继续往下执行时会将 `onerror` 事件发生之前的变量和数据全部销毁；

5. 图片支持 `load` 和 `error` 事件，分别在图片成功加载和加载失败的情况下触发；
    ```javascript
    var img = new Image();

    // EventUtil 为一个我们自定义的对象
    EventUtil.addHandler(image, 'load', function (e) {
       alert('Image loaded!')
    });

    EventUtil.addHandler(image, 'error', function (e) {
       alert('Image not loaded!')
    });

    image.src = 'smilex.gif';
    ```

## 常见错误类型
    * 类型转换错误
    * 数据类型错误
    * 通信错误

1. 类型转换发生的情景
    * 操作符转换
    * 等性运算符
    * 流程控制语句

2. **最佳实践**：使用（非）全等运算符代替（非）等于运算符进行比较；

3. **最佳实践**：**虽然流控制语句的判断可以自动将条件转换为布尔值，但是最好的是在条件中切实地传入布尔值**；
    ```javascript
    // not good
    if (values) {
        // ...
    }

    // good
    if (typeof values == 'number') {
        // ...
    }
    ```

4. **最佳实践**：对于执行函数时传入的参数，函数内需要对传入参数的数据类型做相应的检查；

5. **最佳实践**：不推荐将一个值与 `null` 和 `undefined` 比较；

6. **最佳实践**：基本类型检测使用 `typeof`，对象类型检测使用 `instanceof`；

7. **最佳实践**：对于查询字符串，必须使用 `encodeURIComponent()` 方法来转码；

## 把前端的错误记录到服务器
1. 在服务器上建立一个专用的错误记录处理程序：`log.php`；

2. 前端将错误信息作为请求链接的查询参数，向服务器的对应记录程序 URI 发起请求，**小技巧：使用 `Image` 对象来发送请求**
    ```javascript
    function logError(level, msg) { // 错误等级与错误信息
        var img = new Image();
        img.src = 'log.php?sev=' + encodeURIComponent(sev) + '&msg=' + encodeURIComponent(msg);
    }

    try {
        // ...
    } catch (err) {
        logError(1, 'Init failed: ' + err.message)
    }
    ```
    > 使用 `Image` 对象来发送请求的优点：
    1. 兼容：所有浏览器兼容，兼容性胜过 `XHR`；
    2. 可以避免跨域限制；

3. 服务器收到请求，解析查询参数，记录归档分析
