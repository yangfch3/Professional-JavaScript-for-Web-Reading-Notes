# JSON

## 语法
1. 支持以下三种类型的值
    * 简单值
    * 对象
    * 数组

2. 不支持以下类型值
    * `undefined`
    * 变量
    * 函数
    * 对象实例（但支持对象表达式）

3. **凡是符合 `JSON` 语法要求的数据都是有效的 `JSON` 数据；**

4. `JavaScript` 字符串的引号可单可双，`JSON` 数据中必须为双引号；

5. `JSON` 中的对象要求为属性添加双引号；

6. 常犯错误：
    * 属性名必须加双引号；
    * 出现了 `undefined`（应该用 `null` 代替）。

7. `JSON` 数据还可以是一个数组；

8. 可以使用对象与数组搭配组合成更复杂的数据。

## 解析与序列化
1. 早期使用强大的 `eval()`，因为 `JSON` 是 `JavaScript` 语法的子句；

2. ES 5 定义了全局的 `JSON` 对象，并为其设置了特性的解析与序列化的方法；

3. 两个主要方法
    * `stringify()`：把对象或值序列化为 `JSON` 字符串
    * `parse()`：把 `JSON` 字符串解析为原生 `JavaScript`

### JSON.stringify()
1. 接收三个参数
    * 待序列化为 JSON 字符串的对象
    * 过滤器（可选）：数组或函数
    * 格式选项（可选）

2. 默认情况（无可选参数）下，`JSON.stringify()` 输出的 `JSON` 字符串不包含任何空格字符或缩进；

3. 第二可选参数（过滤器）为数组时：
    ```javascript
    var book = {
        "title": "HBS",
        "year": 2011,
        "edition": 3
    }

    var jsonText = JSON.stringify(book, ["title"]);
    // "{"title":"HBS"}"
    ```

4. 第二个可选参数（过滤器）为函数时
    * 接收两个参数
        1. 属性键
        2. 属性值
    * 返回值：相应键的值
        1. 返回 `undefined` 则删除该属性
        2. 函数会遍历对象的 **属性键-属性值**，对属性值进行过滤或进行相应的处理

    ```javascript
    var book = {
        "title": "HBS",
        "year": 2011,
        "edition": 3,
        "reader": ["yangfch3", "liuhq3"]
    }

    var jsonText = JSON.stringify(book, function(key, value) {
        switch (key) {
            case "edition":
                return undefined; // to be delete
            case "reader":
                return value.join(", ");
            default:
                return value;
        }
    })

    jsonText;
    // "{"title":"HBS","year":2011,"reader":"yangfch3, liuhq3"}"
    ```

5. 第三个可选参数实现缩进
    * **数值**：表示每个级别缩进的空格数（最大为10，一般为 2 或 4）
    * **字符串**：将缩进的空格换成相应的短划线

    ```javascript
    var book = {
        "title": "HBS",
        "year": 2011,
        "edition": 3,
        "reader": [{"name": "yangfch3"}, {"name": "liuhq3"}]
    }

    var jsonText1 = JSON.stringify(book, null, 4);
    var jsonText2 = JSON.stringify(book, null, "--");

    jsonText1
    // "{
    //     "title": "HBS",
    //     "year": 2011,
    //     "edition": 3,
    //     "reader": [
    //         {
    //             "name": "yangfch3"
    //         },
    //         {
    //             "name": "liuhq3"
    //         }
    //     ]
    // }"

    jsonText2;
    // "{
    // --"title": "HBS",
    // --"year": 2011,
    // --"edition": 3,
    // --"reader": [
    // ----{
    // ------"name": "yangfch3"
    // ----},
    // ----{
    // ------"name": "liuhq3"
    // ----}
    // --]
    // }"
    ```

6. 对象的 `toJSON()` 方法
    * 原生 `Date` 对象有一个 `toJSON()` 方法
    * 可以为对象定义 `toJSON()` 方法

    ```javascript
    var book = {
        "title": "HBS",
        "year": 2011,
        "edition": 3,
        "reader": [{"name": "yangfch3"}, {"name": "liuhq3"}],
        toJSON: function() {
            return this.title;
        }
    }
    ```

7. 对一个有 `toJSON` 方法的对象进行 `JSON.stringify()` 序列化时的详细过程
    1. 如果存在 `toJSON` 方法而且能通过对象的该方法取得有效的值，则调用该方法。否则，返回对象本身；
    2. 如果提供了第二个参数，应用这个函数过滤器。传入函数过滤器的值时第一步返回的值；
    3. 对第（2）步返回的值进行序列化
    4. 如果提供第三个参数，执行相应的格式化

### JSON.parse()
1. 接收两个参数
    * 待解析的 JSON 字符串
    * 还原函数（可选）

2. 还原函数：与 `JSON.stringify()` 的过滤函数刚好相反
    * 接收两个参数
        1. 属性键
        2. 属性值
    * 返回值：相应键的值
        1. 返回 `undefined` 则删除该属性
        2. 函数会遍历对象的 **属性键-属性值**，对属性值进行过滤或进行相应的处理

3. 实例
    ```javascript
    var book = {
        "title": "HBS",
        "year": 2011,
        "edition": 3,
        "reader": [{"name": "yangfch3"}, {"name": "liuhq3"}]
    }

    var jsonText = JSON.stringify(book);

    var bookCopy = JSON.parse(jsonText, function(key, value) {
        if (key == 'reader') {
            return undefined;
        } else {
            return value;
        }
    });

    bookCopy;
    // Object {title: "HBS", year: 2011, edition: 3}
    ```
