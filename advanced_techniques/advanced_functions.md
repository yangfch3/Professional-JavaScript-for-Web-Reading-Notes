# 高级函数
函数的本质是 **简单**、**过程化** 的，但表现却可以是非常复杂和动态的；一些额外的功能可以使用闭包实现。

## 安全的类型检测
1. `JavaScript` 内置的类型检测机制并非完全可靠
    * `typeof` 对于 `null` 的检测，对于不同类型对象的区分
    * `typeof` 在部分浏览器下检测正则对象返回 `function`，其他浏览器则返回 `object`
    * `instanceof` 跨框架、窗口的多个全局作用域下

2. 在检测某个对象是原生对象还是开发人员自定义的对象时（即是原生的构造函数产生，还是自定义的构造函数产生）使用 `typeof` 和 `instanceof` 能力有限；

3. **解决方法**：在任何值上调用 `Object` 原生的 `toString()` 方法，可以得到 **表征引用类型类型的字符串**
    ```javascript
    Object.prototype.toString.call(value);
    // [object ...]
    ```

4. 有时需要检测 `JSON` 是否原生（老道 `JSON` 库定义了一个非原生的 `JSON` 全局对象）

5. 检测函数合集
    ```javascript
    var isType = {
        isString: function(value) {
            return typeof value === 'string';
        },
        isBoolean: function(value) {
            return typeof value === 'boolean';
        },
        isNumber: function(value) {
            return typeof value === 'number';
        },
        isUndefined: function(value) {
            return typeof value === undefined;
        },
        isNull: function(value) {
            return value === null;
        },
        isObject: function(value) {
            if (value === null) {
                return false;
            } else if (typeof value === 'function' || typeof value === 'object') {
                return true;
            }
        },
        isArray: function(value) {
            return Object.prototype.toString.call(value) === '[object Array]';
        },
        isFunction: function(value) {
            return Object.prototype.toString.call(value) === '[object Function]';
        },
        isRegExp: function(value) {
            return Object.prototype.toString.call(value) === '[object RegExp]';
        },
        isNativeJSON: function() {
            return window.JSON && Object.prototype.toString.call(JSON) === '[object JSON]';
        }
    };
    ```

6. 以上检测函数的限制
    * 低版本 IE 下以 `COM` 对象实现的任何函数，`isFunction()` 都将返回 `false`
    * 以上检测方案是在 `Object` 构造函数的原型对象的 `toString()` 方法没有被篡改的前提下进行的

---
## 作用域安全的构造函数
1. 当构造函数像普通函数一样使用时（没有使用 `new` 操作符来调用构造函数），构造函数的 `this` 对象指向 `window`，会导致 `window` 对象意外的被覆盖或更改
    > `new` 操作符调用构造函数时，构造函数中的指向新创建的对象实例

2. 创建一个作用域安全的构造函数
    ```javascript
    function Person(name, age, job) {
        // 首先检测确认 this 是否为正确类型的实例
        if (this instanceof Person) {
            this.name = name;
            this.age = age;
            this.job = job;
        } else {
            return new Person(name, age, job)
        }
    }
    ```

3. 仅使用 2 中的方法的缺陷，看以下函数的表现如何
    ```javascript
    function Polygon(sides) {
        if (this instanceof Polygon) {
            this.sides = sides;
            this.getArea = function() {
                return this.sides;
            };
        } else {
            return new Polygon(sides);
        }
    }

    function Rectangle(width, height) {
        Polygon.call(this, 2);
        this.width = width;
        this.height = height;
        this.getArea = function() {
            return this.width * tghis.height;
        };
    }

    var rect = new Rectangle(5, 10);
    alert(rect.sides); // undefined
    ```
    > 在上面的代码中，`Polygon` 构造函数的作用域是安全的，然而 `Rectangle` 则不是，`Polygon.call(this, 2)` 中的 `this` 对象并不是 `Polygon` 的实例，所以最终导致 `Rectangle` 构造函数中的 `this` -- 即新建的 `Rectangle` 实例没有得到增长，`Polygon.call()` 返回的对象又没有用到

4. 解决方法：构造函数窃取 + 原型链继承（寄生组合继承）
    ```javascript
    function Polygon(sides) {
        if (this instanceof Polygon) {
            this.sides = sides;
            this.getArea = function() {
                return this.sides;
            };
        } else {
            return new Polygon(sides);
        }
    }

    function Rectangle(width, height) {
        Polygon.call(this, 2);
        this.width = width;
        this.height = height;
        this.getArea = function() {
            return this.width * tghis.height;
        };
    }

    Rectangle.prototype = new Polygon(); // 使得能通过 if (this instanceof Polygon) 判断

    var rect = new Rectangle(5, 10);
    alert(rect.sides); // undefined
    ```

5. 多个程序员在同一个页面上编码时，作用域安全便有用了。

## 惰性载入函数
1. 因为浏览器的差异，我们常常需要包含大量的 `if` 语句做兼容，如果每次运行一个函数，都需要检测 `if` 中的条件，这无疑对资源造成了浪费。

    有没有办法让我们 **不必每次都运行 `if` 语句**，只需要第一次运行后记下分支的正确走向，之后就可以避开判断了呢？

2. 惰性载入：函数执行的分支只会仅会发生一次，实现方式：
    1. 函数在第一次调用时，内部再处理（重写）函数 -- 第一次调用惰性判断
    2. 在声明函数的时候就根据分支条件指定适当的函数 -- 代码首次加载时惰性判断

3. 方法一：
    ```javascript
    function process() {
        if (...) {
            process = function() {
                ... // 重写 process 函数
            }
        } else {
            process = function() {
                ... // 重写 process 函数
            }
        }
    }
    ```

4. 方法二：
    ```javascript
    var process = (function() {
        if (...) {
            return function() {...}
        } else {
            return function() {...}
        }
    })();
    ```

5. 惰性载入函数的优点是：只在第一次执行分支代码时牺牲一点儿性能，之后便可以避免执行不必要的代码。

## 函数绑定
1. 函数绑定是创建一个函数，可以在特定的 `this` 环境中指定参数调用另一个函数；

2. 函数绑定的技巧常常和回调函数与事件处理程序一起使用，用以确保函数执行时的执行环境；

3. 执行环境对函数执行的影响
    ```javascript
    var handler = {
        message: 'Event handled',
        handleClick: function(event) {
            alert(this.message);
        }
    }

    var btn = document.getElementById('myBtn');

    eventUtil.addEventHandler(btn, 'click', handler.handleClick);
    // undefined
   ```
   > `handler.handleClick` 指针指向一个函数，普通执行（不作为对象的一个方法）时 `this` 指向 `btn`，而不是 `handler` 对象

4. 保存（还原）执行环境的方法：闭包保存执行环境
    ```javascript
    var handler = {
        message: 'Event handled',
        handleClick: function(event) {
            alert(this.message);
        }
    }

    var btn = document.getElementById('myBtn');

    eventUtil.addEventHandler(btn, 'click', function(event) {
        handler.handleClick(event);
    });
    // 'Event handled'
    ```
    > 此处，函数是作为 handler 方法调用的，`this` 指向 `handler` 对象

5. `bind` 函数
    ```javascript
    function bind(fn, context) {
        return function() {
            return fn.apply(context, arguments);
        }
    }
    ```
    > 在 `bind` 函数内创建一个闭包，闭包内使用 `apply` 调用传入的函数，并 `apply` 传入的上下文和参数

6. `bind` 函数使用实例
    ```javascript
    var handler = {
        message: 'Event handled',
        handleClick: function(event) {
            alert(this.message);
        }
    }

    var btn = document.getElementById('myBtn');

    eventUtil.addEventHandler(btn, 'click', bind(handler.handleclick, handler));
    ```

7. ES 5 为所有的函数实例新增了 `bind` 方法，与上面定义的 `bind` 函数类似，返回的是一个 **已经绑定好执行环境的函数**
    ```javascript
    var handler = {
        message: 'Event handled',
        handleClick: function(event) {
            alert(this.message);
        }
    }

    var btn = document.getElementById('myBtn');

    eventUtil.addEventHandler(btn, 'click', handler.handleclick.bind(handler));
    ```

8. 兼容的 `bind` 函数
    ```javascript
    function bind(fn, context) {
        return function() {
            if (fn.bind) {
                return fn.bind(context);
            } else {
                return fn.apply(context, arguments);
            }
        }
    }
    ```

## 偏函数
1. partial function

2. 用于创建已经设置好一个或多个参数的函数；
    >技巧：使用闭包返回一个函数
调用偏函数（方法时）为它传入要局部缓存的函数和必要参数，返回的是已经配置好初始参数的函数的偏函数版本。

3. 代码：绑定了执行环境的偏函数
    ```javascript
    function partial(fn) {
        var args = Array.prototype.slice.call(arguments, 1);

        return function() {
            var innerArgs = Array.prototype.slice.call(arguments);
            var finalArgs = args.concat(innerArgs);
            return fn.apply(null, finalArgs)
        }
    }
    ```

4. 使用实例
    ```javascript
    function add(num1, num2) {
        return num1 + num2;
    }

    var partialAdd = partial(add, 5); // 传入待偏化的函数以及默认参数
    curriedAdd(3); // 8
    ```

5. 在偏函数化的同时为函数绑定好执行环境
    ```javascript
    function partial(fn, context) {
        var args = Array.prototype.slice.call(arguments, 2);

        return function() {
            var innerArgs = Array.prototype.slice.call(arguments);
            var finalArgs = args.concat(innerArgs);
            return fn.apply(context, finalArgs)
        }
    }

    // 使用实例
    function add(num1, num2) {
        return num1 + num2;
    }

    var partialAdd = partial(add, null, 5); // 传入待偏化的函数、执行环境以及默认参数
    partialAdd(3); // 8
    ```

6. ES 5 的 `bind` 方法也是一个偏函数：`bind` 方法可以新建一个函数，第一个参数执行新函数的执行环境，之后的参数列表是函数的默认参数
    ```javascript
    // fn.bind(context, args+)

    ```

## 柯里化
1. partial function

2. 与偏函数的区别
    1. 偏函数：fn(x, y, z) ----> partialFn = partial(fn, x) -----> partialFn(y, z)
    2. 柯里化：fn(x, y, z) ----> curriedFn = curry(fn) ------> curriedFn(x)(y)(z)/curriedFn(x)(y, z)/curriedFn(x, y)(z)

3. 加强版柯里化示例
    ```javascript
    const _ = {}
    function crazyCurryingHelper(fn, length, args, holes) {
        length = length || fn.length    // 第一遍是fn所需的参数个数，以后是
        args = args || []
        holes = holes || []
        
        return function(...rest) {
            let _args = args.slice(),
                _holes = holes.slice(),
                argLength = _args.length,        // 存储接收到的args和holes的长度
                holeLength = _holes.length,
                arg, i = 0
            for (; i < rest.length; i++) {
                arg = rest[i]
                if (arg === _ && holeLength) {
                    holeLength--                      // 循环_holes的位置
                    _holes.push(_holes.shift())      // _holes最后一个移到第一个
                } else if (arg === _) {
                    _holes.push(argLength + i)          // 存储_hole就是_的位置
                } else if (holeLength) {              // 是否还有没有填补的hole
                    holeLength--
                    _args.splice(_holes.shift(), 0, arg)           // 在参数列表指定hole的地方插入当前参数
                } else {
                    _args.push(arg)            // 不需要填补hole,直接添加到参数列表里面
                }
            }
            
            return _args.length >= length                          // 递归的进行柯里化
                ? fn.apply(this, _args)
                : crazyCurryingHelper.call(this, fn, length, _args, _holes)
        }
    }

    function sayHello(name, age, fruit) { console.log(`我叫 ${name},我 ${age} 岁了, 我喜欢吃 ${fruit}`) }

    // _ 用于参数占位
    const betterShowMsg = crazyCurryingHelper(sayHello)
    betterShowMsg(_, 20)('小衰', _, '西瓜')          // 我叫 小衰,我 20 岁了, 我喜欢吃 西瓜
    betterShowMsg(_, _, '南瓜')('小猪')(25)          // 我叫 小猪,我 25 岁了, 我喜欢吃 南瓜
    betterShowMsg('小明')(_, 22)(_, _, '倭瓜')          // 我叫 小明,我 22 岁了, 我喜欢吃 倭瓜
    betterShowMsg('小拽')(28)('冬瓜')          // 我叫 小拽,我 28 岁了, 我喜欢吃 冬瓜
    ```



