# 闭包
1. 匿名函数不等于闭包，匿名函数是闭包的一种实现手段；

2. 闭包的本质：**有权访问另一个函数作用域中的变量的函数**；

3. 函数调用（执行）过程详解：
    * 执行流进入该函数
    * 为函数创建一个执行环境，推入环境栈，接替执行权
    * 初始化函数的局部参数及内建参数（`arguments`, `this`），创建函数活动对象，构建执行环境的作用域链，完成作用域链指针正确指向
    * 执行环境的 `[[scope chain]]` 指针指向该作用域链
    * 执行完毕，函数执行环境被销毁，作用域链被销毁，函数的活动对象无特殊情况（闭包）也被销毁
    * 执行流继续往下

4. 作用域链的本质是一个指向变量对象的指针列表；

5. 闭包实现的技术性解释
    > 函数在执行完毕后，执行环境销毁，作用域链销毁，**但活动对象不会销毁**，仍然留存在内存中，因为闭包的作用域链仍旧在引用这个活动对象。

6. 将指向闭包的变量解除对闭包的引用（`null`），则 **闭包的作用域链被销毁**，其作用域链引用的驻留在内存中的变量对象也可被垃圾回收例程清除；

7. 作用域链关系图
    ![img_1130.png-696.6kB][1]

## 1. 闭包与变量
1. 闭包的作用域链保存（指向）着包含函数的整个变量对象；

2. 闭包只能取得 **包含函数中任何变量** 的最后一个值，因为 **闭包获得包含函数中变量的值是通过对包含函数变量对象的引用实现的**；
    ```javascript
    function createFunctions() {
        var result = new Array();

        for(var i = 0; i < 10; i++) {
            result[i] = function() {
                return i;
            };
        }

        return result;
    }

    var result = createFunctions(); // 得到 10 个闭包组成的数组

    result[5](); // 10
    ```

3. 可以通过创建另一个匿名函数强制让闭包的行为符合预期（预期是一个模糊的概念），**在这个中间层匿名函数中我们创建最外层包含函数变量对象中变量值的副本**；
    ```javascript
    function createFunctions() {
        var result = new Array();

        for(var i = 0; i < 10; i++) {
            result[i] = (function(num){
                return function() {
                    return num;
                };
            })(i);
            // 循环运行十次匿名函数，每个匿名函数都有一个不同状态 i 的副本
            // 返回的数组十项都是子匿名函数，每个子匿名函数有权访问与之对应的中间层匿名函数的变量对象，所以每个子匿名函数都能获取到不同的 `num`
        }
    }
    ```

4. 牢记：**函数传参是按值传递**，与变量的复制一样。

## 2. 关于 this 对象
1. 函数一经运行，函数的 **活动对象** 会初始化 `this`、`arguments`、**参数** 和 **局部变量**；

2. `this`、`arguments` 这两个隐藏的特殊变量会在函数被调用时自动取得；

3. 函数 `this` 指针是在运行时 **基于函数的执行环境绑定的**：
    > 一般函数，`this` 指向 `window`；
做为某个对象的方法调用时，`this` 指向那个对象；
匿名函数的执行环境具有全局性，单独执行 `this` 指向 `window`。

4. 内部函数在搜索 `this`, `arguments` 对象时，只会搜索到其活动对象为止，因此 **永远不可能直接访问外部函数中的这两个变量**；

5. **手段**：需要访问或保留 **包含函数中** 的 `this` 和 `arguments` 变量时，把外部作用域中的 `this` 或 `arguments` 引用保存在一个闭包能访问到的变量里，就可以让闭包访问二者了
    ```javascript
    var name = 'The window';

    var object = {
        name: "My Object",
        getNameFunc: function() {
            var that = this; // lock the 'this'
            return function() {
                return that.name;
            }
        }
    };

    object.getNameFunc()();
    ```

6. `this` 的值可能会由于调用方式的不同而发生意外的改变
    ```javascript
    var name = 'The window';

    var object = {
        name: "My Object",
        getName: function() {
            return that.name;
        }
    };

    object.getName(); // 'My Object'
    (object.getName)(); // 'My Object'
    (object.getName = object.getName)(); // 'The window'
    ```
    > 要理解第三次调用为什么是“The window”，请查阅语句与表达式的定义与区别，二者返回值的区别。

## 3. 内存泄露
1. 在 **IE 9 以下**，如果闭包的作用域链中保存着一个 HTML 元素，那么就意味着该元素将无法销毁；
    > IE 9 及之前的版本对 `JScript` 和 `COM` 对象使用不同的垃圾收集例程。

2. IE 9 以下内存泄露代码
    ```javascript
    function assignHandler() {
        var element = document.getElementById('someElement');

        element.onclick = function() {
            alert(element.id);
        }
    }
    ```
    > `element` 的引用次数至少是 1（匿名函数内 `element.id` 带来的引用次数）。

3. 改写，轻量化包含函数的活动对象
    ```javascript
    function assignHandler() {
        var element = document.getElementById('someElement');
        var id = element.id;

        element.onclick = function() {
            alert(id);
            // 只访问包含函数的 id 变量，而没有访问包含函数中的 DOM 元素了
        };

        // 将原先引用 `DOM` 的变量指向 `null`
        element = null;
    }
    ```

  [1]: http://static.zybuluo.com/yangfch3/ut6l1dvrh0ptzvxacronk7fc/img_1130.png
