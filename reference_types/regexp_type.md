# RegExp 类型
1. 正则表达式：`Regular Expression`

2. `ES` 的正则表达式类似 `Perl` 语法，基本格式
    ```javascript
    var expression = /pattern/flasg
    ```
    其中：`pattern`：字符串模式，简单或复杂的正则表达式；
    `flags`：是匹配模式标志

3. 三个标志：
    * `g`：全局（global）匹配模式，默认首次匹配模式
    * `i`：不区分大小写（case-insensitive）模式，默认大小写敏感
    * `m`：多行（multiline）模式，默认单行模式

4. 简单实例
    ```javascript
    var somestr = 'Hello World! Hello yangfch3!';
    var pattern0 = /hello/g;
    var pattern1 = /yangfch3/gi;

    var index0 = somestr.search(pattern0);
    index0; // -1 大小写敏感，无法匹配

    var index1 = somestr.search(pattern1);
    index1; // 19
    ```

5. **转义**：表达式中使用的所有元字符都需要转义，使用 `\`
    >`(` `)` `[` `]` `{` `}` `^` `*` `$` `|` `\` `+` `.`
这些元字符在正则表达式中都有一种或多种特殊用途，如果想要匹配的字符串中包含这些字符，就需要转义
    ```javascript
    var pattern0 = /[bc]at/i; // 匹配第一个 'bat' 或 'cat'

    var pattern1 = /\[bc\]at/i; // 匹配第一个 '[bc]at'
    ```

6. 使用 `RegExp()` 构造函数来创建正则表达式
    * 参数
        * （必选）要匹配的字符串模式
        * （可选）标志 -- `g` `i` `m`

    ```javascript
    var pattern0 = /[bc]at/i;
    // 等价于
    var pattern1 = new RegExp('[bc]at', 'i')
    ```

7. **易错点**：使用字面量形式创建正则表达式时 **不用引号**；使用构造函数传参传入的是 **字符串**，**参数需要引号**；

8. 由于 `RegExp()` 构造函数的模式参数是 **字符串**，`\` 在字符串内本身便具有转义的功能；所以在某些情况下要对字符进行双重转义；
    ```javascript
    var pattern0 = /\[bc\]at/i;
    // 下面需要使用双重转义
    var pattern1 = new RegExp('\\[bc\\]at', 'i');
    ```

9. 使用字面量与构造函数创建的表达式相同的正则表达式不是同一个实例，使用字面量或构造函数每次都是创建新的实例
    ```javascript
    var pattern0 = /\[bc\]at/i;
    var pattern1 = new RegExp('\\[bc\\]at', 'i');
    var pattern2 = /\[bc\]at/i;
    var pattern3 = new RegExp('\\[bc\\]at', 'i');

    pattern0; // /\[bc\]at/i
    pattern1; // /\[bc\]at/i
    pattern0 == pattern1; // false

    pattern0 == pattern2; // false

    pattern1 == pattern3; // false
    ```
    > 在 `ES3` 中，使用字面量法创建的表达式相同的正则表达式会共享同一个 `RegExp` 实例

10. **最佳实践**：采用字面量法来创建正则表达式

## RegExp 实例属性
* `global`：布尔值，是否设置了 `g` 标志
* `ignoreCase`：布尔值，是否设置了 `i` 标志
* `lastIndex`：整数，表示开始搜索下一个匹配项的字符位置，从 0 起
* `multiline`：布尔值，是否设置了 `m` 标志
* `source`：正则表达式的字符串表示

```javascript
var pattern0 = /[bc]at/gim;

pattern0.global; // true
pattern0.ignoreCase; // true
pattern0.multiline; // true
pattern0.lastIndex; // 0
pattern0.source; // '/[bc]at/gim'

```

## RegExp 实例方法
1. `exec()` -- `RegExp` 对象的主要方法
    * **参数**
    接收一个参数，即要应用模式（`pattern`）的字符串
    * **返回值**
        * 成功匹配时，返回包含第一个匹配项信息的数组；
        * 不成功匹配时，返回 `null`
    * **返回数组的额外属性**
    返回的是 `Array` 的实例，但是该数组包含两个额外的属性
        * `index`：表示匹配项在字符串中的位置
        * `input`：表示应用正则表达式的字符串
    * **返回的数组项**
        * 第一项是与整个模式匹配的字符串；
        * 其他项是与模式中的 **捕获组** 匹配的字符串（如果没有其他捕获组，则该数组只包含一项）

    ```javascript
    var text = 'grandmom and grandpa and mom and dad and baby';
    /*
     * 下面的正则包含两个捕获组，最内部的 ' and baby'
     * 以及包含她的捕获组：'and dad and baby'
     */
    var pattern = / mom( and dad( and baby)?)?/gi
    var matches = pattern.exec(text);

    matches; // [" mom and dad and baby", " and dad and baby", " and baby"]
    alert(matches.index); // 24
    alert(matches.input); // "grandmom and grandpa and mom and dad and baby"
    alert(matches[0]); // " mom and dad and baby"
    ```

2. 捕获组的概念见专业的正则表达式书籍或标准；

3. `exec()` 方法返回 **包含第一个匹配项信息的数组**
**未设置全局标志**，多次调用 `exec()` 每次只会返回第一个匹配项
**设置全局标志**、多次调用 `exec()` 都会在字符串中继续查找下一个新的匹配项，直至搜索结束
    ```javascript
    var text = 'Hello World! Hello yangfch3!';
    var pattern1 = /hello/i;
    var pattern2 = /hello/gi;

    // 未使用全局标志的第 1 次查找
    var matches = pattern1.exec(text);
    matches;                            // ["Hello"]
    matches.index;                      // 0
    matches[0];                         // "Hello"
    pattern1.lastIndex;                 // 0

    // 未使用全局标志的第 2 次查找
    matches = pattern1.exec(text);
    matches;                            // ["Hello"]
    matches.index;                      // 0
    matches[0];                         // "Hello"
    pattern1.lastIndex;                 // 0

    // 使用了全局标志的第 1 次查找
    var matches = pattern2.exec(text);
    matches;                            // ["Hello"]
    matches.index;                      // 0
    matches[0];                         // "Hello"
    pattern2.lastIndex;                 // 0

    // 使用了全局标志的第 2 次查找
    matches = pattern2.exec(text);
    matches;                            // ["Hello"]
    matches.index;                      // 13
    matches[0];                         // "Hello"
    pattern2.lastIndex;                 // 18

    // 使用了全局标志的第 3 次查找：没有了
    matches = pattern2.exec(text);
    matches;                            // null
    matches.index;                      // error
    matches[0];                         // error
    pattern2.lastIndex;                 // error

    ```

4. `test()` 方法
    * 参数
    被正则表达式作用的一个字符串
    * 返回值
    布尔值；匹配时返回 `true`，不匹配返回 `false`

5. `test()` 方法常用于 `if` 语句作为判断方法 ：只需要知道目标字符串与某个模式是否匹配，但不需要知道其文本内容的情况
    ```javascript
    var text = '000-00-0000';
    var pattern = /\d{3}-\d{2}-\d{4}/;

    if (pattern.test(text)) {
        alert('Test is Ok!')
    }
    // test() 方法常用于表单验证
    ```

6. `toLocaleString()` `toString()` 两个方法都返回正则表达式的字面量字符串；

7. `valueOf()` 方法返回正则表达式本身。

## RegExp 构造函数属性
1. `RegExp` 构造函数包含一些属性，适用于作用域中的所有正则表达式，**基于所执行的最后一次正则操作而变化**；

2. 这些属性是属于构造函数的，或者说是属于 `RegExp` 对象类型的；

3. 9 个用于存储捕获组的构造函数属性用于方便地访问匹配历史记录里的 9 个捕获组


|长属性名|短属性名|说明|
|:--:|:--:|:--|
|input|\$_|最后一次执行正则匹配的字符串|
|lastIndex|\$&|最后一次的匹配项|
|lastParen|\$+|最后一次匹配的捕获组|
|leftContext|\$`|input 字符串中 lastMatch 之前的字符串|
|rightContext|\$'|input 字符串中 lastMatch 之后的字符串|
|multiline|$*|布尔值，表示是否所有表达式都使用多行模式|
||\$1-9|存储|

```javascript
var text1 = 'Hello World! Hello yangfch3!',
    text2 = '小富城和小海荞',
    pattern1 = /(.)ello/gmi,
    pattern2 = /小海荞/gm;

pattern2.test(text2); // true
RegExp.input; // "小富城和小海荞"
RegExp.lastMatch; // "小海荞"
RegExp.lastParen; // ""
RegExp.leftContext; // "小富城和"
RegExp.rightContext; // ""
RegExp.multiline; // undefined (chrome not support)
RegExp.$1; // ""

pattern1.exec(text1);
RegExp.input; // "Hello World! Hello yangfch3!"
RegExp.lastMatch; // "Hello"
RegExp.lastParen; // "H"
RegExp.leftContext; // ""
RegExp.rightContext; // " World! Hello yangfch3!"
RegExp.multiline; // undefined (chrome not support)
RegExp.$1; // "H"
RegExp.$2; // ""
```

## JavaScript 正则类型的局限性
1. 缺少某些语言所支持的高级正则表达式特性；

2. 列表：不支持的部分特性
    * 向后查找
        * 但完全支持向前查找
    * 并集和交集类
    * 原子组
    * Unicode 支持
        * 但支持单个字符
    * 命名的捕获组
        * 支持编号的捕获组
    * 单行模式，无间隔匹配模式
    * 条件匹配
    * 正则表达式注释
