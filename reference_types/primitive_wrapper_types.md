# 基本包装类型
1. 3 个特殊的（非严格的）引用类型：`Boolean` `Number` `String`；

2. 引入基本包装类型的目的：**便于操作基本类型值**；

3. <span style="color:red">JavaScript 解析器每读取一个基本类型的时候，后台便会创建一个对应的 **基本包装类型** 的对象，从而让我们能够调用一些方法来操作这个简单类型数据；</span>
    ```javascript
    var s1 = 'some text';
    var s2 = s1.substring(2);

    s2; // 'me text'
    ```

4. 我们知道：基本类型值不是对象，从逻辑上讲不应该有方法（但实际上如我们确实能调用方法进行处理）；为了方便我们对简答数据类型的操作，后台会自动完成一系列处理：
    1. 创建 `String` 类型的一个实例；
    2. **在实例上** 调用指定的方法；
    3. 销毁这个实例。

    ```javascript
    // 改写 3 中的实例代码，使更直观

    /*
     * 使用 `new String('str')` 操作创建基本包装类型对象实例
     * 也可以使用 `new Object('str')` 操作创建基本包装类型对象实例
     */
    var s1 = new String('some text');
    // typeof s1; // object

    var s2 = s1.substring(2);
    s1 = null; // 销毁
    ```

5. 引用类型与基本包装类型的区别：对象的生存周期
    > 自动创建的基本包装类型的对象，只存在于一行代码的执行瞬间，然后立即被销毁
    >由于是立即销毁，**我们不能在运行时为基本类型值添加属性和方法（无效操作）！**

6. 使用 `new` 调用基本包装类型的构造函数，与直接调用同名的转型函数是不一样的，一个返回对象，一个返回基本类型值：
    ```javascript
    var num1 = '10';
    var number = Number(num1);
    number; // '10'

    var obj = new Number(num1);
    obj; // Number {[[PrimitiveValue]]: 10}
    typeof obj; // 'object'
    ```

7. **最佳实践**：拒绝显式地调用 `Boolean` `Number` `String` 来创建基本包装类型的对象；
也不要为 `Object` 构造函数传入字符串、布尔、数值类型值来构建基本包装类型

## Boolean 类型
1. 创建 `Boolean` 对象
    ```javascript
    var booleanObject = new Boolean(true);

    booleanObject; // Boolean {[[PrimitiveValue]]: true}
    typeof booleanObject; // 'object'
    ```

2. `Boolean` 对象实例方法
    * `valueOf()` 返回基本类型值：`true` `false`
    * `toString()` 返回字符串：`'true'` `'false'`

3. **易错点**：
    ```javascript
    var booleanObject = new Boolean(false);
    var result = booleanObject && true;

    typeof booleanObject; // 'object'
    booleanObject instanceof Boolean; // true
    result; // ♣ true
    ```

5. **最佳实践**：永远不要使用 `Boolean` 对象

## Number 类型
* `valueOf()` 方法返回数值；
`toString()` `toLocaleString()` 方法返回字符串；

* `toFixed()` 方法
    1. 作用：按照指定的小数位（0-20个小数位）返回数值的 **字符串**
    2. 参数：零个或一个，指定返回 **字符串** 的小数位
    3. **注意**：使用 `toFixed()` 可以对小数数值进行自动舍入，不同浏览器表现存在差异。

    ```javascript
    var num0 = 10;
    num0.toFixed(2); // '10.00'

    // 使用 toFixed() 进行自动舍入
    var num1 = 10.005;
    num1.toFixed(2); // '10.01'
    ```

* `toExponential()` 方法
    1. 作用：返回指数表示法表示的数值的字符串形式
    2. 参数：零个或一个，指定输出结果字符串的小数位数

    ```javascript
    var num = 1234567890;
    num.toExponential(4); // "1.2346e+9"
    ```

* `toPrecision()` 方法
    1. 作用：依据参数返回最适合的格式的数值字符串，相当于 `toFixed()`、`toExponential()` 的综合智能加强版；
    2. 参数：零个或一个，指定返回数值字符串的所有数字的位数（不包括指数部分）
    3. `toPrecision()` 也有自动向上向下舍入的作用，见下面示例

    ```javascript
    var num = 99;
    num.toPrecision(1); // "1e+2"
    num.toPrecision(2); // "99"
    num.toPrecision(3); // "99.0"
    ```

* **最佳实践**：永远不要使用 `Number` 对象

## String 类型
`toString()` `toLocaleString()` `valueOf()` 方法都返回对象所表示的基本字符串值；

* **字符方法**
    1. `charAt()`
        * 作用：返回参数指定的位置的字符
        * 参数：一个，基于 0 的字符位置

        ```javascript
        var stringValue = 'hello world!';
        stringValue.charAt(1); // 'h'
        ```

    2. `charCodeAt()`
        * 作用：返回参数指定位置字符的字符编码
        * 参数：一个，基于 0 的字符位置

        ```javascript
        var stringValue = 'hello world!';
        stringValue.charCodeAt(1); // 101
        ```

    3. `[]` 访问法（`ES5`，部分支持）
        * 作用：简化地访问指定位置的字符
        * 参数：一个，字符串内字符索引

        ```javascript
        var stringValue = 'hello world!';
        stringValue[2]; // 'l'
        ```
* **字符串操作方法** -- [链接：字符串方法一览][1]
    1. `concat()` -- 拼接
        * 作用：拼接多个字符串，生成并返回一个新的字符串，不影响原字符串
        * 参数：任意多个，变量、字符串或可以 `String` 化的值
        * **最佳实践**：放弃 `concat`，直接使用 `+` 进行拼接操作

        ```javascript
        var a = 'hello',
            b = ' world!',
            o = {
                toString: function () {
                    return ' hello yangfch3!';
                }
            };

        var str1 = a.concat(a, b, o); // "hellohello world! hello yangfch3!"
        var str2 = a + b + o; // "hellohello world! hello yangfch3!"
        ```

    2. `substr()` `substring()` `slice()`
        * 作用：基于原字符串创建新字符串副本，不影响原字符串
        * 参数
            * 参数一：子字符串开始位置
            * 参数二（可选）：`slice()` `substring()` 方法是字符串的**结束位置**，`substr()` 是指定的**字符长度**
        * **注意点**：当参数是负数时，几个方法的处理不同

        ```javascript
        var stringValue = 'hello world!';

        stringValue.slice(3, 7); // 'lo w'
        stringValue.substr(3, 7); // 'lo worl'
        stringValue.substring(3, 7); // 'lo w'
        ```

* **字符串位置方法**
    `indexOf()` `lastIndexOf()`
    > 两个方法的区别：一个从头向后搜，一个从后往前搜
    >
    > 第二个可选参数：搜索的起始位置

* `trim()` `trimLeft()` `trimRight()`

* `toLowerCase` `toUpperCase` `toLocaleLowerCase` `toLocaleUpperCase`
    前两个方法借鉴自 `java.lang.String` 中的同名方法

* **字符串模式匹配方法**
    `match()` `search()`
    > 两个方法接受的唯一参数形式：
    > * 正则表达式
    > * `RegExp` 对象
    >
    >返回值的不同:
    * `match()` 返回字符串内所有符合正则表达式的字符或字符串组成的数组
    * `search()` 返回第一个匹配项的索引或者 `-1`



* `replace()`
    1. 参数
        * 参数一：正则、RegExp 对象、字符串
        * 参数二：字符串、正则字符序列（本质也是字符串）、函数

    2. 注意点
        * 替换不对原字符串造成影响，返回的是新的字符串类型值
        * 如果第一个参数是字符串，则只会替换第一个匹配的子字符串
        * 字符匹配序列见 [链接：RegExp 类型][2]

    3. `replace()` 实例

        ```javascrip
        var stringValue = 'Hello world! Hello yangfch3!';
        var result1 = stringValue.replace('Hello', 'hi');
        var result2 = stringValue.replace(/hello/gmi, )
        result1; // "hi world! hello yangfch3!"
        ```
        函数作为 `replace()` 参数的实例：**`HTML` 转义**
        ```javascript
        function htmlEscape(text) {
            /*
             * 参数 match：模式的匹配项
             * 参数 pos：模式的匹配项在字符串中的位置
             * 参数 originalText：原始字符串
             */
            return text.replace(/[<>"&]/g, function (match, pos, originalText) {
                switch (match) {
                    case '<':
                        return '&lt;';
                    case '>':
                        return '&gt;';
                    case '&':
                        return '&amp;';
                    case '\"':
                        return '&quot';
                }
            })
        }

        htmlEscape('<p class="greeting">Hello world!</p>');
        ```


* `split()`
    * 作用：基于指定的分隔符将一个字符串分割成多个子字符串，返回字符串数组
    * 参数
        1. 参数一：分隔符可以是字符串，也可以是 `RegExp()` 对象
        2. 参数二（可选）：指定数组的大小

        ```javascript
        var colorText = 'red, blue, green, yellow';

        var colorList1 = colorText.split(', '); // ["red", "blue", "green", "yellow"]
        var colorList2 = colorText.split(', ', 2); // ["red","blue"]
        var colorList3 = colorText.split(/[^\,]+/); // ["", ",", ",", ",", ""]
        ```
    * **注意点**：使用正则表达式时一定要在各个浏览器下多做测试，确保兼容

* `localeCompare()` 方法
    * 作用：比较两个字符串，返回一个数字

        ```javascript
        var stringValue = 'yellow';

        stringValue.localeCompare('brick'); // 1
        stringValue.localeCompare('yellow'); // 0
        stringValue.localeCompare('zoo'); // -1
        ```

* `fromCharCode()` 方法
    * 作用：接收任意多个字符编码，将它们转换成一个字符串

    ```javascript
    String.fromCharCode(104, 101, 108, 108, 111); // 'hello'
    ```

* HTML 方法

    `anchor(name)`、`big()`、`bold()`、`fixed()`、`fontcolor(color)`、`fontsize(size)`、`italics()`、`link(url)`、`small()`、`strike()`、`sub()`、`sup()`
    ```javascript
    'str'.anchor('name'); // "<a name="name">str</a>"
    ```




  [1]: https://www.zybuluo.com/yangfch3/note/284313
  [2]: https://yangfch3.gitbooks.io/professional-javascript-for-web-reading-notes/content/reference_types/regexp_type.html
