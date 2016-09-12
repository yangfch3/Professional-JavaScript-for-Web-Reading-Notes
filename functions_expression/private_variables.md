# 私有变量
1. `JavaScript` 没有 **私有成员** 的概念，所有**对象属性都是公有的**；

2. `JavaScript` 中的 **私有变量**（不能在外部访问到）：函数的参数、局部变量和在函数中定义的其他函数；

3. **创建用于访问私有变量的公有方法（特权方法）**
    > 在函数内部创建一个 **闭包**，那么 **闭包通过自己的作用域链就可以访问这些私有变量**

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


