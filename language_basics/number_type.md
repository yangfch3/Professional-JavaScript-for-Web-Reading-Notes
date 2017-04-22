# Number 类型
1. `JS` 中使用 `IEEE754` 格式来表示整数（**double,int**）和浮点数值（**float**）；
    > 使用基于 `IEEE754` 数值的浮点计算时不精确的，这不是 `JS` 的锅！
`0.1 + 0.2 === 0.3 // false`

2. 某些语言把整数称为 **双精度数值** -- `double`，JavaScript 中的数值是 64 位双精度数值

3. 八进制、十六进制表示与 `C` 风格一致：
八进制（**oct**）：字面量第一位为 `0`，其他位为小于 7 的**自然数**
十六进制（**hex**）：识别头 `0x`，各位 `0-F` 字母可大写，**推荐大写**

4. 八进制字面量在严格模式下无效；
    > ES6 做了补充

5. 进行算术计算时，八进制、十六进制表示的数值最终都将转换为十进制数值；

6. 浏览器 `JS` 引擎会不失时机地将如 `1.0` `1.` 之类的数字转换为整数如 `1`，以节省内存开销；

7. 默认情况下，`JS` 引擎会将小数点后带有 6 个 0 以上的 0~1 浮点数转换为 e 表示法表示：`0.00000003` ---> `3e-7`

8. 浮点数的最高精度是 17 位小数；

9. `Number` 常量：
    * `Number.MAX_VALUE` = 1.7976931348623157e+308
    * `Number.MIN_VALUE` = 5e-324
    * `Infinity` = +∞
    * `-Infinity` = -∞
    * `Number.POSITIVE_INFINITY` = `Infinity`
    * `Number.NEGATIVE_INFINITY` = `-Infinity`

10. `window` 的一个方法（函数）`isFinite()` 用于判断参数代表字面量有否有穷，类似方法还有 `isNaN`
    ```javascript
    var result = Number.MAX_VALUE + Number.MAX_VALUE; // Infinity
    alert(isFinite(result)); // false
    ```

11. `NaN`(Not a Number)，**是一个特殊的数值**，出现在**本来引擎预期返回数值的操作未返回数值**的情况；

12. `ES` 语言标准设立 `NaN` 是为了程序运行中防止一些愚蠢的运算抛出错误

13. `window` 的 `isNaN()` 方法（函数），接受一个参数，对其按需进行 `Number()` 转型，再判断是否为 `NaN`；
>当接受的参数为一个对象时，先调用对象的 `valueOf()` 方法，返回一个 `temp` 值，判断 `isNaN(temp)`，否则对 `temp` 继续调用对象的 `toString(temp)` 方法，对返回值进行 `isNaN` 判断。

    ```javascript
    var a = {foo:"123"};
    a.valueOf(); // Object {foo: "456"}
    a.toString(); // [Object object]
    isNaN(a); // true

    a.toString = function() {
        return 456;
    }
    isNaN(a); // false

    a.valueOf = function() {
        return 123;
    }
    isNaN(a); // false
    ```

14. 任何涉及 `NaN` 的操作都会返回 `NaN`；`NaN` 不与任何值相等，包括 `NaN` 本身；

15. `Number()` 可将任意非数值转换为数值（`NaN` 也是一个特殊的数值）；`parseInt()`、`parseFloat()` **只用于**将 **字符串** 转换为数值。
    > `Number()` 作用于对象时，先调用对象的 `valueOf()` 方法获得返回值 `temp`，若 `isNaN(temp)` 为 `true` 则使用 `temp` 继续执行对象的 `toString()` 方法，对返回值进行 `Number()` 转换。我叫这个为 **对象普用转型法**

16. `new Number(sth)` 也是可以创建一个 `Number` 对象，但是一般不需要 `Number` 对象实体，只需要 `number` 数据类型实体；

17. 易忘的转换规则：
    * `Number(null); // 0`
    * `Number(undefined); // NaN`
    * `Number()` 可以将八进制、十六进制数转换为十进制
    * `Number('123abc'); // NaN` <span style="color:red">不同于 `parseInt()` 和 `parseFloat()` 的处理方式，或者说 `Number` 更严格</span>

18. `Number()` 转换可以通过 **一元加** 操作符实现：
    ```javascript
    var a = +'123';
    a; // 123

    var b = +{};
    b; // NaN
    ```

19. **最佳实践**：字符串转换为数值用 `parseInt()` 或 `parseFloat()`，其他类型值转换为数值使用 `Number()`

20. `parseInt()` 转换的几个特点：
    * `parseInt('   123.2ab456c'); // 123` 忽略空格，智能解析 **打头的 连续的** 数字字符串
    * `parseInt(''); // NaN`
    * `parseInt()` 能解析十六进制，转为十进制；**它不能解析转换八进制的数值**
    > Tips：`ES5` 开始，`parseInt()` 不支持解析八进制数值，自动忽略特征 `0` 进行解析；但是可以通过下面 `#21` 来解析八进制！

21. `parseInt()` 接受除待转换数值外的另一个参数 -- 转换时使用的进制基数
    ```javascript
    var num1 = parseInt('0xAF', 16); // 175
    var num2 = parseInt('AF', 16); // 175

    var num3 = parseInt('010', 8); // 8
    ```

22. **最佳实践**：无论在什么情况下都明确指定基数，即使基数是默认的 10；

23. `parseFloat()` 特点：
    1. 字符串第一个小数点有效，之后的小数点无效；`var a = parseFloat(' 00123.456.789'); // 123.456`；
    2. `parseFloat()` 只将字符串作为十进制解析；`var b = parseFloat('0xAF'); // 0`；
    3. `parseFloat()` 没有第二个可选参数。

24. `Number.isInteger(arg);` 用于检测一个数（arg）是否为整数

---
JavaScript 中的数码系统剖析以及为何 `0.1 + 0.2` 不等于 0.3

![](https://uploads.disquscdn.com/images/53898e8706b9489ca0f66dbfc4f264714c8a738e7ac65823d3fd757558375976.jpg)
![](https://uploads.disquscdn.com/images/b0427133a3855b7e77b5ea54defaa1d13a35530e4b9c0ff62f3e802b28ea348a.jpg)
