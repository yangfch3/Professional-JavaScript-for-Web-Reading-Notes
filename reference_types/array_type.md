# Array 类型(上)
1. `ECMAScript` 数组与其他语言的数组有着相当大的区别：
    * `ES` 的数组每一项都可以保存任何类型的数据；
    * `ES` 的数组大小是可以动态调整的。

2. `Array()` 构造函数的三种使用：依据接受的单数的数量与类型决定不同的操作，可用 `new`，也可以不用（二者效果一致）；
    ```javascript
    var arr = Array();
    var arr = new Array(); // 创建一个空数组

    var arr = Array(20);
    var arr = new Array(20); // 创建一个 length 长为 20 的数组

    var arr = Array('20');
    var arr = new Array('20'); // 创建一个只有 '20' 一项的数组

    var arr = Array('cool', 'great', 'awesome');
    var arr = new Array('cool', 'great', 'awesome'); // 创建一个包含该三项的数组
    ```

4. 数字字面量法注意事项：
    * 最后一项后面不要加 `,`；
    * 不要有其他多余的逗号；
    * 最佳实践：最后一项与 `]` 间不要有空格。
    ```javascript
    var values = [1, 2,] // 不要这样，在不同浏览器解析的数组长度不同

    var options = [,,,,,,] // 不要这样，除非你傻逼
    ```
5. 与对象一样，在使用数组字面量表示法时，也不会调用 `Array()` 构造函数；

6. 数组的 `length` 属性是可写的；
    ```
    var arr = ['a', 'b', 'c'];
    alert(arr.length); // 3

    arr.length = 5; // ['a', 'b', 'c', undefined, undefined]
    arr.length = 2; // ['a', 'b']
    ```

7. **容易忽略的规律**：数组最后一项的索引是 `length-1`，下一个新项的位置是 `length`；
    ```javascript
    var colors = ['red', 'blue', 'green'];
    alert(colors[colors.length-1]); // "green"

    colors[colors.length] = 'black';
    alert(colors[colors.length-1]); // "black"
    ```

8. 数组最多包含 `4,294,967,295` 项。

## 检测数组
1. `instanceof` 检测的问题：多个框架间的构造函数是不同的，跨多个框架（多个全局环境）引用数组时无法检测
    ```javascript
    // 单个框架可行
    var colors = ['red', 'yellow', 'green'];
    alert(colors instanceof Array); // true

    //多个框架
    var colors = ['red', 'yellow', 'green'];
    frames[0].a = window.colors;
    frames[1].b = frames[0].a;

    alert(frames[0].a instanceof Array); // true
    alert(frames[0].a instanceof window.Array); // false
    ```
2. `Array.isArray()` 方法用于最终确定某个值是不是数组：
    ```javascript
    var colors = ['red', 'yellow', 'green'];

    if (Array.isArray(colors)) {
        // 对数组执行某些操作
    }
    ```
## 转换
1. 所有对象都具有 `toString()` `toLocaleString()` `valueOf()` 方法；

2. 数组调用 `toString()` `valueOf()` 方法返回拼接的字符串时的本质是：**调用数组每一项的 `toString()` 方法**；

3. 数组调用 `toLocaleString()` 方法时会调用数组每一项的 `toLocaleString()` 方法；
    ```javascript
    var person1 = {
        toLocaleString: function() {
            return '小富城';
        },
        toString: function() {
            return 'yangfch3';
        }
    };

    var person2 = {
        toLocaleString: function() {
            return '小海荞';
        },
        toString: function() {
            return 'liuhq3';
        }
    };

    var us = [person1, person2];
    alert(us); // use each toString -- "yangfch3,liuhq3"
    alert(us.toString()); // use each toString -- "yangfch3,liuhq3"
    alert(us.valueOf()); // use each toString -- "yangfch3,liuhq3"
    alert(us.toLocaleString()); // use each toLocaleString -- "小富城,小海荞"
    ```

4. `alert()` 要接收字符串参数，如不是字符串参数，会进行隐式地强制转换；

5. 使用 `join()` 方法，使用非 `,` 来构建转换后的字符串；接收一个 **参数：用作分隔符的字符串**，不传参或传入 `undefined` 则默认使用 `,` 作为分隔符。
    ```javascript
    var colors = ['red', 'green', 'yellow'];

    alert(colors.join('--')); // 'red--green--yellow
    ```

## 栈方法
1. **栈**：一种 `LIFO（Last-In-First-Out, 后进先出）` 的 **数据结构**，特征是：最新添加的项最早被移除；

2. 两个操作：**推入（push）** 和 **弹出（pop）**；
    一个位置：只发生在栈的顶部；
    两个方法：`push()` `pop()`

3. `push()` 方法接受任意数量的参数，把他们逐个 **推入** 数组末尾，<span style="color:red">返回的是：修改后数组的长度！</span>；
    ```javascript
    var colors = [];

    var count = colors.push('red', 'green');
    alert(count); // 2
    ```

4. `pop()` 方法不接受参数，从数组末尾移除最后一项，减少数组的 `length`，<span style="color:red">然后返回移除的项</span>；
    ```javascript
    var colors = ['red', 'green', 'yellow'];

    var lastItem = colors.pop();
    alert(lastItem); // 'yellow'
    alert(colors.length); // 2
    ```

## 队列方法
1. **队列数据结构**：一种遵循访问规则 `FIFO（First-In-First-out, 先进先出）` 的数据结构；

2. **队列在列表的末端添加项，从列表的前端移除项**；

3. `shift()` 方法：移除数组中的第一项，<span style="color:red">返回被移除的项</span>，同时将数组 `length` 减 1；

4. 使用 `shift()` 和 `push()` 可以像队列一样使用数组；
    ```javascript
    var colors = ['red', 'yellow', 'green'];
    var count = colors.push('white', 'black');
    alert(count); // 5
    var firstItem = colors.shift();
    alert(firstItem); // 'red'
    ```

5. `unshift()` 方法：接收任意个参数，在数组前端添加任意个项并<span style="color:red">返回新数组的长度</span>；
    ```javascript
    var colors = ['red', 'yellow', 'green'];
    var count = colors.unshift('brown', 'pink');
    alert(count); // 5
    ```

6. 同时使用 `unshift()` 和 `pop()` 方法可以从相反的方向来模拟队列：**即在数组的前端添加项，数组末端移除项**；

    > `IE7` 及更早版本对 `unshift()` 方法的处理有偏差：总是返回 `undefined` 而不是数组的新长度

7. `delete` 操作对数组也是有效的，`in` 操作也能作用于数组
    ```javascript
    delete arr[index];
    index in arr; // => false
    ```
