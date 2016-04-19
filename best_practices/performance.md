# 性能
`JavaScript` 是一门解释型语言，执行速度比编译型语言慢得多；`Chrome` 是第一款内置优化引擎，将 `JavaScript` 编译成本地代码的浏览器，实现了 `JavaScript` 的编译执行。

## 注意作用域
随着作用域数量的增加，访问当前作用域以外的变量时间也在增加。访问全局变量总要比访问局部变量慢，因为需要遍历作用域链。

1. 避免全局查找
    全局变量本地副本化，可以将多次全局查找变为单次全局查找
2. 避免 `with` 语句
    `with` 会增加执行的代码的作用域链的长度

## 使用正确的方法
**一、避免不必要的属性查找**
常见算法（运算）类型与算法的复杂度关系
|标记|描述|
|:---:|:---:|
|O(1)|简单值与简单变量赋值|
|O(logn)|例如二分查找|
|O(n)|例如遍历|
|O(n^2)|例如插入排序操作|

使用 **变量和数组** 要比访问 **对象上的属性** 更有效率，后者是一个 `O(n)` 操作（对象属性遍历）。

优化多重属性查找，将常需要访问的对象属性存储在局部变量里，这样，第一次访问该值会是 `O(n)`，第二次便是 `O(1)`。

**二、优化循环**

1. **减值迭代** -- 从最大值开始（**如果确实可以的话**），在循环中不断减的迭代器更加高效；

2. **简化终止条件** -- 在终止条件中避免 `O(n)` 操作

3. **简化循环体** -- 避免密集计算

4. **尽量使用后测试循环** -- 因为可以避免最初终止条件的计算

**三、展开循环**

当 **循环次数是确定的**，**消除循环** 并使用 **多次函数调用** 往往更快！

当 **循环次数不能事先确定**，则可以使用一种叫做 **Duff 装置** 的技术。
> Duff 装置的基本概念：通过计算迭代的次数是否为 8 的倍数 **将一个循环展开为一系列语句**

代码实例
```javascript
var iterations = Math.ceil(values.length / 8);
var startAt = values.length % 8;
var i = 0;

do {
    switch (startAt) {
        case 0:
            process(values[i++]);
        case 7:
            process(values[i++]);
        case 6:
            process(values[i++]);
        case 5:
            process(values[i++]);
        case 4:
            process(values[i++]);
        case 3:
            process(values[i++]);
        case 2:
            process(values[i++]);
        case 1:
            process(values[i++]);
    }

    startAt = 0;
} while(--iterations > 0)
```

改进版 **Duff 装置**
```javascript
var iterations = Math.floor(values.length / 8);
var leftover = values.length % 8;
var i = 0;

if(leftover > 0) {
    do {
        process(values[i++])
    } while(--leftover > 0)
}

do {
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
    process(values[i++]);
} while(--iterations > 0)
```

**Duff 装置** 一般只在针对大数据集时才展开循环，对于小数据集，会有额外的开销。

**四、避免双重解释**
`eval()` `new Function()` `setTimeout()` 这些操作内部可以接收 `JavaScript` 代码字符串，但是会存在双重解释惩罚。
> 这些操作内部的 JavaScript 代码字符串不能在初始的解析过程中完成，当运行到这些 JavaScript 代码字符串时，JS 引擎会新启动一个解析器来解析新的代码。

**五、其他性能注意事项**
1. 原生方法较库封装方法快；

2. `switch` 语句比复杂的 `if-else` 语句更快；

3. 位运算符较快；
    >位运算操作符要比布尔运算或算数运算更快，可以选择性的使用 **位运算** 替换 **取模**，**逻辑或**，**逻辑与** 运算。

## 最小化语句数
1. 合并多个变量声明；

2. 使用迭代值时尽可能合并语句；
    > 合并表达式与自增运算
例如：`var value = values[i++];` 等效于先取值 `values[i]` 再 `i++`

3. 使用数组和对象字面量

## 优化和 DOM 的交互
DOM 操作（交互）是最慢的；

1. 最小化现场更新
    * 减少零碎的 DOM 更新，合并之
    * 尽可能使用文档碎片：`document.createDocumentFragment()`

2. 更新大量 `HTML` 结构时，使用 `innerHTML`
    > 使用 `innerHTML` 时，后台会新创建一个 `HTML` 解释器，然后使用 **内部** 的 `DOM` 调用来创建 `DOM` 结构。借助原生解释器，而非 `JavaScript`，所以执行很快。

3. 使用事件代理

4. 注意 `HTMLCollection` 的动态性与一经调用实时查找性
    > 大部分情况下，我们应该将 `HTMLCollection` 对象的属性或方法存储在局部变量，以避免在循环体内部多次调用 `HTMLCollection`。
