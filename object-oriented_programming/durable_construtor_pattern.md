# 稳妥构造函数
1. 老道初创；

2. **稳妥对象**：没有公共属性，其方法也不引用 `this`；
    **稳妥**：创建的对象是十分安全的；

3. 稳妥对象适合在一些需要安全的环境（防止数据被不经意改动）中使用；

4. 代码实例
    ```javascript
    function Person(name, age, job) {
        // 创建要返回的对象
        var o = new Object();

        // 定义私有变量和函数

        // 添加方法
        o.sayName = function() {
            alert(name);
        }

        // 返回对象
        return o;
    }

    var person1 = Person('yangfch3', 21, 'UI Dev');
    person1.name; // undefined
    person1.sayName(); // 'yangfch3'
    ```

5. **注意点**
    * 新创建对象的实例方法不引用 `this`
    * 不使用 `new` 操作符调用构造函数
    * 除了使用 `sayName()` 方法外，没有其他办法访问 `name` 的值（闭包特性）
    * 使用稳妥构造函数模式创建的对象与构造函数之间没有什么关系
