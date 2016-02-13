# 单体内置对象
* **内置对象**：由 `EXMAScript` 实现提供的、不依赖于宿主环境的对象。例如之前的 `Object` `String` `Array` 都是内置对象

* **单体内置对象**：`Global` 和 `Math`

## Global 对象
1. 不属于任何其他对象的属性和方法（函数），最终都是 `Global` 对象的属性和方法；

2. 对于浏览器环境，`Global` 对象是 `window` 对象，不同的宿主环境 `Global` 对象不同；

3. 诸如 `isNaN()` `isFinite()` `parseInt()` `parseFloat()` 都是 `Global` 对象的方法；

### URI 编码方法
1. `encodeURI()`
    * 作用：对 `URI` 进行编码，用特殊的 `UTF-8` 编码替换所有无效的字符（例如空格等），使浏览器能接受和理解
    * 特点
        * 用于整个 URI
        * 不会对本身属于 URI 的特殊字符（`:/?#`）进行编码

    ```javascript
    var uri = 'http://ce.sysu.edu.cn/ hope/#index';

    encodeURI(uri); // "http://ce.sysu.edu.cn/%20hope/#index"
    ```

2. `encodeURIComponent()`
    * 作用：同上
    * 特点
        * 主要用于对 URI 中的某一段进行编码
        * `encodeURIComponent()` 会对他发现的任何非标准字符进行编码

    ```javascript
    var qstr = '查询内容';
    var uri = location.href + '?' + encodeURIComponent(qstr);
    // "http://ce.sysu.edu.cn/hope/?%E6%9F%A5%E8%AF%A2%E5%86%85%E5%AE%B9"
    ```

3. **最佳实践**：非特殊情况，仅使用 `encodeURIComponent()` 方法对查询字符串进行编码；

4. `decodeURI()` `decodeURICompenent()`
    * 区别
        * `decodeURI()` 方法只能对使用 `encodeURI()` 替换的字符进行解码：只能把 `%20` 替换为空格，不会对其他替换字符进行解码。
        * `decodeURICompenent()` 能够解码使用 `encodeURICompenent()` 编码的所有字符

5. **最佳实践**：使用 `encodeURICompenent()` 对查询字符串进行编码，使用 `decodeURICompenent()` 进行解码

6. **冷知识**：`ES 3` 中的 `escape()` 和 `unescape()` 现已废弃，二者只能编码解码 ASCII 字符

### eval() 方法
1. 被 `eval()` 执行的代码具有与该执行环境相同的作用域，在 `eval()` 执行的代码中可以包含环境中定义的变量；

2. 在 `eval()` 中创建的任何变量或函数都不会被提升，因为在解析代码时，他们被包含在一个字符串中，它们只在 `eval()` 执行的时候创建

3. 不要用 `eval()` 执行用户输入的数据！
    实现 `XSS` 常用的方法是：浏览器容错性 + `eval()`

### Global() 对象的属性
`undefined`、`NaN`、`Infinity` 以及 `Object` `Function` 都是 Global 对象的属性。

### window 对象
1. Web 浏览器将 `Global` 对象做为 `window` 对象的一部分加以实现的；`window` 除了实现了 `Global` 对象外，还承担着其他重要的作用

2. 取得 `Global` 对象的方法
    ```javascript
    var global = function() {
        return this;
    }();

    global; // window{...}
    ```

## Math 对象
`Math` 对象提供了完成高级计算的属性和方法

### Math 对象的属性
* 科学计算属性

```javascript
Math.E; // 2.718281828459045
Math.LN10; // ...
Math.LN2;
Math.LOG2E;
Math.LOG10E;
Math.PI;
Math.SQRT1_2;
Math.SQRT2;
```
* `min()` `max()`

```javascript
var max = Math.max(2, 7, 89, 53);
var min = Math.min(2, 7, 89, 53);

max; // 89
min; // 2
```
```javascript
// 寻找数组中的最大值 最小值
var values = [1, 2, 3, 4, 5, 6, 7];

// apply 的第二个数组参数会作为函数的参数传入
var max = Math.max.apply(Math, values); // 7
var min = Math.min.apply(Math, values); //  1
```
* 舍入方法
    `Math.ceil()` `Math.floor()` `Math.round()`

* `random()` 方法
    1. 返回的是介于 0~1 的随机数
    2. 随机数生成的公式：Math().floor(Math.random() * 可能值的总数 + 第一个可能的值)

    ```javascript
    // 一个专用于产生随机数的函数
    function selectFrom(lowerValue, upperValue) {
        var choices = upperValue - lowerValue + 1; // 随机区间
        return Math.floor(Math.random() * choices + lowerValue);
    }

    var num = selectFrom(2, 10);
    num; // 7

    // 使用 selectFrom 函数从数组中任意选出某一项
    var colors = ['red', 'yellow', 'blue', 'white', 'black'];

    var color = colors[selectFrom(0, colors.length-1)];
    color; // black
    ```

* 其他方法
`Math.abs(num)`
`Math.exp(num)`
`Math.log(num)`
`Math.pow(num, power)`
`Math.sqrt(num)`
`Math.cos(x)` -- 余弦
`Math.sin(x)` -- 正弦
`Math.tan(x)` -- 正切
`Math.acos(x)` -- 反余弦
`Math.asin(x)` -- 反正弦
`Math.atan(x)` -- 反正切
`Math.atan2(y, x)` -- y/x 的反正切
