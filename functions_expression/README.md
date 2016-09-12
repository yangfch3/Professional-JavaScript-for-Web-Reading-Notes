# 函数表达式
函数的类型：

* **具名函数**
* **匿名函数**（拉姆达函数）

定义函数的方式有两种：

* **函数声明**：必须有函数名标识符跟在 `function` 关键字上，否则报错，**是一个完整的语句块**
* **函数表达式**，只是一个表达式

函数声明的识别依赖于 `function` 关键字以及后面的标识符；函数表达式的识别依赖于 `function` 关键字和 **表达式上下文**。

> 匿名函数与具名函数没有本质的区别（只是一个存在实实在在的标识符，一个标识符为空），但函数声明与函数表达式有巨大的区别。

**函数声明会发生提升**，而 **函数表达式则只会发生变量声明提升**。这是二者最大的区别！

函数与一般表达式、对象或简单值都可以成为 **赋值供体**，也可以 **做为函数返回值**。

一段代码读懂函数声明与函数表达式，具名函数与匿名函数
```javascript
// f 指向 func 具名函数
var f = function func() {
    alert(f.name);
    // func
}

// f 指向匿名函数
var f = function () {
    alert(f.name);
    // f
}

// 以上两个为函数表达式
// 下面是函数声明
// 函数声明的最终结果 func 指向 function func() {}
function func() {
    alert(func.name);
}

```

---
## 扩展
### 表达式上下文的重要性
```javascript
function(){return true;}();
// SyntaxError: Unexpected token (
```
> 原因：在 `javascript` 代码解释时，当遇到 `function` 关键字时，会 **默认** 把它当做是一个函数声明，而不是函数表达式，如果没有把它 **显式地表达成函数表达式**（给予表达式上下文），就报错了，因为函数声明需要一个函数名，而上面的代码中函数没有函数名。（以上代码，也正是在执行到第一个左括号(时报错，因为(前理论上是应该有个函数名的。）

### 立即执行与分组操作符
```javascript
function foo(){return true;}();
// SyntaxError: Unexpected token )
```
> 原因：在一个 **表达式** 后面加上括号，表示该表达式立即执行；而如果是在一个 **语句** 后面加上括号，该括号完全和之前的语句不搭嘎，而只是一个 **分组操作符**，用来 **控制运算中的优先级**（小括号里的先运算）。所以以上代码等价于两步
>
    function foo(){ /* code */ }
    (); // SyntaxError: Unexpected token )
> `()`（分组操作符）内的表达式不能为空，所以报错.

### 立即执行函数（IIFE）
立即执行函数
```javascript
(function(){ /* code */ })();
(function(){ /* code */ }());


```
> 在 `javascript` 里，**括号内部不能包含语句**，当解析器对代码进行解释的时候，先碰到了 `()`，然后碰到 `function` 关键字就会 **自动将 `()` 里面的代码识别为函数表达式而不是函数声明**。

立即执行函数的其他不推荐写法
```javascript
true && function(){ /* code */ }();
0, function(){ /* code */ }();

!function(){ /* code */ }();
~function(){ /* code */ }();
-function(){ /* code */ }();
+function(){ /* code */ }();
```

最佳实践：使用 `IIFE` 并直接将返回值赋给变量的写法
```javascript
// 可以，但不推荐
var i = function(){ return 10; }();

// 推荐，使用 () 包起整个函数表达式，方便阅读
var i = (function(){ return 10; }());
```

**为什么叫立即执行函数，而不叫自执行函数**
```javascript
// 这是一个自执行函数，函数内部执行的是自己，递归调用
function foo() { foo(); }

// 这是一个自执行匿名函数，因为它没有函数名
// 所以如果要递归调用自己的话必须用arguments.callee
var foo = function() { arguments.callee(); };

// 这可能也算是个自执行匿名函数，但仅仅是foo标志引用它自身
// 如果你将foo改变成其它的，你将得到一个used-to-self-execute匿名函数
var foo = function() { foo(); };

// 有些人叫它自执行匿名函数，尽管它没有执行自己，只是立即执行而已
(function(){ /* code */ }());

// 给函数表达式添加了标志名称，可以方便debug
// 但是一旦添加了标志名称，这个函数就不再是匿名的了
(function foo(){ /* code */ }());

// 立即执行函数也可以自执行，不过不常用罢了
(function(){ arguments.callee(); }());
(function foo(){ foo(); }());
```

---
附图：
![2016-04-21_014929.jpg-35.2kB][1]


  [1]: http://static.zybuluo.com/yangfch3/8lktgw3idpgdvfrqifc9kcz5/2016-04-21_014929.jpg
