# Function 类型(中)
## 函数内部属性
1.  `arguments`
    1. 属性：`callee`；
    2. `arguments.callee` 用在函数内部，指向拥有这个 `arguments` 对象的函数
    3. 借助 `callee` 属性实现递归
        ```javascript
        function factorial(n) {
            if (n <= 1) {
                return 1;
            } else {
                return n * arguments.callee (n -1)
                // 考虑到函数名的灵活性，请不要写成
                // return n * factorial (n -1)
            }
        }

        factorial(5); // 120
        factorial(10); // 3628800
        ```
    4. **最佳实践**：在函数内部引用函数本身，使用 `arguments.callee` 指针属性，而不要直接使用函数名引用。


2. `this` 的指向 -- 牢记 **环境对象**
    1. 与 `Java` 和 `C#` 中的 `this` 类似；
    2. `this` 是一个对象的引用，指向目标对象；
    3. <span style="color:red">`this` 引用的是函数据以执行的 **环境对象**</span>；

        ```javascript
        window.color = 'red';
        var o =  {color: 'blue'};

        function showColor() {
            alert(this.color);
        }

        // showColor() 赖以执行的环境对象是 window
        showColor(); // 'red'

        o.showColor = showColor;
        // o.showColor 赖以执行的环境对象是 o
        o.showColor(); // 'blue'

        // 牢记：函数名仅仅是一个包含指针的变量
        var fn = o.showColor; // fn ---> o.showColor ---> showColor
        // fn 只是指向 showColor 函数的一个变量，并没有绑定 环境对象
        fn(); // 'red'
        ```

3. **`this` 指向扩展**
    1. **扩展一：普通函数（非对象方法）中的 `this` 指向**
        * 非严格模式下：指向 `window`
            ```javascript
            var name = 'liuhq3';

            function showName() {
                alert(this.name);
            }

            showName(); // liuhq3
            ```
        * 严格模式下：指向 `undefined`
            ```javascript
            'use strict';

            var name = 'liuhq3';

            function showName() {
                alert(this);
            }

            showName(); // undefined
            ```
            <br>

    2. **扩展二：对象方法内的二级函数 `this` 指向**
        * 非严格模式下：指向 `window`
            ```javascript
            var name = 'yangfch3';

            var liuhq3 = {
                name: 'liuhq3',
                age: 21,
                showName: function() {
                    alert(this.name);
                    function getName() {
                        alert(this.name); // 'yangfch3'
                    }
                    return getName();
                }
            };

            liuhq3.showName();
            ```
        * 严格模式下：指向 `undefined`
            ```javascript
            'use strict';
            var name = 'yangfch3';

            var liuhq3 = {
                name: 'liuhq3',
                age: 21,
                showName: function() {
                    alert(this.name);
                    function getName() {
                        alert(this.name); // error: not defined
                    }
                    return getName();
                }
            };

            liuhq3.showName();
            ```

4. 对象方法内的二级函数 `this` 指向与对象方法同步的方法：**使用 `that` `_this` `self` 来临时保存 `this`**
    ```javascript
    var liuhq3 = {
        name: 'liuhq3',
        age: 21,
        showName: function() {
            alert(this.name);
            var that = this; // this 指向临时保存给 that
            function getName() {
                alert(this.name); // error: not defined
            }
            return getName();
        }
    };
    ```

5. 其他更改 `this` 指向的方法见下节（`apply()`、`call()`）；

## caller 属性
1. `caller` 属性保存着调用当前函数的函数的引用；
    > 情景：在 A 函数内调用 B 函数，则 B 函数的 `caller` 就是 A 函数。

2. 示例：
    ```javascript
    function outer() {
        inner();
    }

    function inner() {
        alert(inner.caller);
    }

    outer(); // function outer() {...}
    ```

3. 搭配 `arguments.callee` 使用
    ```javascript
    function outer() {
        (function() {
            alert(arguments.callee.caller);
        })()
    }

    outer(); // function outer() {...}
    ```

4. **最佳实践**：保持 `caller` 属性的只读性
