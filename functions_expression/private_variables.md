# 私有变量
1. `JavaScript` 没有 **私有成员** 的概念，所有**对象属性都是公有的**；

2. `JavaScript` 中的 **私有变量**：函数的参数、局部变量和在函数中定义的其他函数；

3. **创建用于访问私有变量的公有方法（特权方法）**
    > 在函数内部创建一个 **闭包**，那么 **闭包通过自己的作用域链也可以访问这些变量**

4. 在 **对象** 上创建特权方法的方式
    1. **构造函数中定义特权方法**：特权方法作为闭包有权访问在构造函数中定义的所有私有变量和函数，且是外部访问的唯一手段；利用私有和特权机制，可以 **隐藏那些不应该被直接修改的数据**。
        ```javascript
        function MyObject() {
            // 定义私有变量和私有函数
            var privateVariable = 10;

            function privateFunction() {
                return false;
            }

            // 特权方法（公有方法）
            this.publicMethod = function() {
                privateVariable++;
                return privateFunction();
            }
        }
        ```
        ```javascript
        function Person(name) {
            // 这里，函数的 name 参数便是私有变量了

            this.getName = function() {
                return name;
            };

            this.setName = function(value) {
                name = value;
            };
        }

        var person = new Person('yangfch3');
        person.getName(); // 'yangfch3'
        person.setName('liuhq3');
        person.getName(); // 'liuhq3'
        ```
    2. **静态私有变量**（见下方）

5. **构造函数模式的缺点**：针对每个实例都会创建同样的一组新方法，内存复用性较差。

## 静态私有变量
1. 通过在私有作用域（**立即执行函数表达式**）中定义私有变量或函数；

2. **静态私有变量的基本模式**
    ```javascript
    (function() {
        // 私有变量和私有函数
        var privateVariable = 10;

        function privateFunction() {
            return false;
        }

        // 构造函数
        MyObject = function() {
        };

        // 公有/特权方法
        MyObject.prototype.publicMethod = function() {
            privateVariable++;
            return privateFunction();
        };
    })();
    ```

3. 注意点
    * 公有方法在构造函数的原型上定义
    * 函数声明只能创建局部函数，所以我们在这里使用函数表达式来创建构造函数
    * 没有使用 `var` 来声明变量，从而使变量全局化
    * 这种模式在严格模式下会报错，因为严格模式下不能初始化未经声明的变量
    * 这个模式下所有对象实例都使用同一个方法，没有内存的浪费

4. 代码实例
    ```javascript
    (function() {
        // 局部变量 name
        var name = '';

        // 无 var，全局化 Person 构造函数
        Person = function(value) {
            name = value;
        };

        Person.prototype.getName = function() {
            return name;
        };

        Person.prototype.setName = function(value) {
            name = value;
        };
    })();

    var person1 = new Person('yangfch3');
    person1.getName();
    person1.setName('liuhq3');
    person1.getName();

    var person1 = new Person('liuhq3');
    person1.getName();
    person1.setName('yangfch3');
    person1.getName();
    ```
    > `name` 是静态的、由所有实例共享的属性
    `Person` 构造函数全局化

4. 使用闭包与私有变量的缺点：**作用域链增加了一环，查找速度受到了轻微的影响**。

## 模块模式
1. **单例**（`singleton`）：只有一个实例的对象，例如我们使用对象字面量创建的对象；
    ```javascript
    var singleton = {
        name: value,
        method: function() {
            // ...
        }
    }
    ```

2. **模块模式**（`module pattern`）：为单例创建私有变量和特权方法；

3. 模块模式基本形式
    ```javascript
    var singleton = function() {
        // 私有变量和私有函数
        var privateVariable = 10;

        function privateFunction() {
            return false;
        }

        // 返回单例
        return {
            publicProperty: true,

            publicMethod: function() {
                privateVariable++;
                return privateFunction();
            }
        };
    }();
    ```
    > 对象在匿名函数内定义，这个对象字面量定义的是单例的公共接口。


4. 这种模式在 **需要对单例进行某些初始化**，同时 **又需要维护其私有变量** 时使用；

5. 在 Web 应用程序中，经常使用一个单例来管理应用程序级的信息
    ```javascript
    var application = function() {
        // 私有变量和函数
        var components = new Array();

        // 初始化，BaseComponent 为特定的构造函数，用于初始化组件数组
        conponents.push(new BaseComponent());

        // 公共
        return {
            getComponentCount: function() {
                return components.length;
            },

            registerComponent: function(component) {
                if(typeof component == 'object') {
                    components.push(component);
                }
            }
        }
    }();
    ```

6. 什么时候使用模块模式？
    > 如果必须创建一个对象并以某些数据对齐进行初始化，同时还要公开一些能够访问这些私有数据的方法，就应该使用模块模式

## 增强的模块模式
1. 增强的模块模式：即在返回对象之前加入对其增强的代码；

2. 增强的模块模式基本形式
    ```javascript
    var singleton = function(){
        // 私有变量和函数
        var privateVariable = 10;

        function privateFunction() {
            return false;
        }

        // 创建将返回的对象实例，使用构造函数可以初始化
        var object = new CustomType();

        // 添加特权/公有属性和方法
        object.publicProperty = true;

        object.publicMethod = function() {
            privateVariable++;
            return privateFunction();
        }
    }();
    ```

3. 在 Web 应用程序中，使用一个单例来管理应用程序级的信息，增强的模块模式的写法
    ```javascript
    var application = function() {
        // 私有变量和函数
        var components = new Array();

        // 初始化，BaseComponent 为特定的构造函数，用于初始化组件数组
        conponents.push(new BaseComponent());

        // 创建 application 的一个局部副本
        var app = new BaseComponent();

        // 公共接口
        app.getComponentCount = function() {
            return components.length;
        };

        app.registerComponent = function(component) {
            if(typeof component == 'object') {
                components.push(component);
            }
        };

        // 返回这个副本
        return app;
    }();
    ```
