# 语句
1. 语句通常使用一或多个关键字来完成给定任务；

2. **最佳实践**
    * 空格问题：见我的 [code guide](https://www.zybuluo.com/yangfch3/note/277518)

### if
1. **最佳实践**：`if-else` 执行语句请始终使用代码块；

### do-while
1. `do{}while()` 是 **后测试** 循环语句，循环体内的代码至少被执行一次；

### while
1. `while(...){...}` 是前测试循环语句，循环体内代码可能永远不被执行；

### for
1. `for` 语句也是一种前测试循环语句，只是能把与循环有关的代码集中在一个位置；

2. 关于 `for` 循环的几点注意：
    * 使用 `while` 循环做不到的，使用 `for` 也一定做不到，`for` 循环与 `while` 循环可以相互改写；
    * `for` 循环变量的初始化可以在外部执行，`因为 `ES` 没有块级作用域`；
    * `for` 语句的初始化表达式、控制表达式、循环后表达式都是都是可选的，但表达式间的 **分号** 不可以省略！


```javascript
// 使用 for 循环写法
var count = 10;
var i=0; // 在循环外部初始化循环变量
for (; i < count; i++){ // 忽略了初始化部分，但没有去掉分号
    alert(i);
}

// 使用 while 循环改写
var count = 10;
var i = 0; // 初始化
while (i < count){
    // 可以看到 while 循环的初始化和后操作不是像 for 那样集中在一起，但是达到的效果一样
    alert(i);
    i++; // 循环后操作
}

// 无限循环
for (;;){
    console.log("Your browser is about to die...");
}
```
### for-in
1. `for-in` 用于枚举对象的属性，格式如下：
    ```javascript
    for (var propName in window) {
        document.write(propName + "\n");
    }
    ```
2. 关于 `for-in` 的几点注意：
    * 控制语句中的 `var` 不是必须的，但是为了保证使用的是局部变量，推荐总是加上 `var`；
    * `for-in` 遍历对象的属性是没有顺序的，先后次序视浏览器不同；
    * 在使用 `for-in` 之前，先检测确认该对象是否为 `null`、`undefined`；
    * 在检测对象实例的属性时，可以使用 `hasOwnerProperty()` 方法来过滤掉数量众多继承属性。

### break, continue & lable
1. `lable` 语句用于为某段代码添加一个标签，以方便将来识别跳转；这些标签可以由 break 和 continue 语句引用，常见于多层循环嵌套的情况使用；

2. `break`：立即**退出**循环，执行循环体以下的代码（**停止循环**）；
    `continue`：立即**跳过**当次循环，跳过后从循环体的顶部继续执行（**少一步循环**）；

3. 使用 `break` `continuew` `lable` 搭配可以立即跳出循环，然后指定继续往下执行的位置
    ```javascript
    // break & lable
    var num = 0;

    outermost:
    for (var i = 0; i < 10; i++){
        for (var j = 0; j < 10; j++){
            if(i === 5 && j === 5) {
               break outermost; // 直接 break 退出了 outermost 代表的循环
            }
            num++
        }
    }

    alert(num); // 55

    // continue & lable
    var num = 0;

    outermost:
    for (var i = 0; i < 10; i++) {
        for (var j = 0; j < 10; j++){
            if (i === 5 && j ===5) {
                continue outermost; // 跳过某次循环，再从 outermost 标签处继续执行下一次的循环
            }
            num++;
        }
    }

    alter(num); // 95
    ```

## with
1. `with` 语句的作用是将代码的作用域设置到一个特定的对象中，即：为`with(){...}` 将一个对象绑定为代码块的作用域；
    ```javascript
    // 一般写法
    var qs = location.search.substring(1);
    var hostName = location.hostname;
    var url = location.href;

    // with 写法
    with(location){ // 使用 with 语句为接下来的执行代码
        var qs = search.substring(1);
        var hostName = hostname;
        var url = href;
    }
    ```
    `with` 语句代码块内部，每个变量先被视为一个 **局部变量**，区块内寻找定义，未找着该变量，就会查看 `with` 指定对象是否有 **同名的属性**，就**以 `with` 指定对象属性的值作为变量的值**；
    *类似我们平时的 `window` 对象属性省略 `window`，直接使用属性名调用！*

2. 使用 `with` 过多，代码性能下降；**严格模式不允许使用 `with` 语句**；

## switch
1. `ES` 中的 `switch` 语句接近于 `C` 等语言的风格，但相比有以下特殊之处：
    * 可以在 `switch` 语句中**使用任何数据类型**
    * 每个 `case` 的值不一定是常量，可以是**变量**，甚至是**表达式**
    * `switch` 在比较值时用的是严格的 **全等** 操作，不会发生类型转换。
    ```javascript
    var test = 'hello code';

    switch ('hello world'){
        case test:
            alert('test hello world and output hello code.');
            break;
        case 'hello ' + 'world':
            alert('test hello world and output hello world.');
            // break; // none break make it continue flow down
        default:
            alert('test hello world and output the default: hello food!')
    }
    ```
2. 常见语法错误：
    * `case ... :` 缺少 `:` 或者错写成 `:` ;
    * 忘了 `break`；

3. **最佳实践**：
    * 用 `switch` 比用 `if-else` 更加优雅简洁；
    * 有意省略 `break` 务必做好注释。
