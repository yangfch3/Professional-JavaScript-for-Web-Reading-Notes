# 静态私有变量与 JS 的 OOP

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
    * 没有使用 `var` 来声明变量，从而使构造函数全局化（当然也可以让立即执行函数将构造函数 return 出去达到暴露的目的，见下面代码实例）
    * 这种模式在严格模式下会报错，因为严格模式下不能初始化未经声明的变量
    * 这个模式下所有对象实例都使用同一个方法，没有内存的浪费

4. 代码实例
    ```javascript
    var Person = (function() {
        // 私有变量
        var name = '';

        // 无 var，全局化 Person 构造函数
        var Person = function(value) {
            name = value;
        };

        Person.prototype.getName = function() {
            return name;
        };

        Person.prototype.setName = function(value) {
            name = value;
        };

        return Person;
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
    > `name` 是静态的、由所有实例共享的“类级别”属性
    `Person` 构造函数全局化

4. 使用闭包与私有变量的缺点：**作用域链增加了一环，查找速度受到了轻微的影响**。

5.

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
