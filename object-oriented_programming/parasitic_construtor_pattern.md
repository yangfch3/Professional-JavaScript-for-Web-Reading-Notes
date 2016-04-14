# 寄生构造函数模式
1. 创建一个函数，该函数的作用仅仅是封装创建对象的代码，然后再返回新创建的对象，从表面上看，这个函数像一个构造函数，但是只是一个普通的函数，封装了创建对象的代码而已；

2. 理解寄生：
    > 工厂模式寄生在构造函数内部；构造函数内部被工厂模式代码寄生了；创建对象代码寄生在构造函数内部，构造函数只是个躯壳

3. 代码实例
    ```javascript
    function Person(name, age, job) {
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function() {
            alert(this.name);
        }

        return o;
    }

    var person1 = new Person('yangfch3', 21, 'UI Dev');
    person1.sayName();
    ```

4. **注意点**
    * 除了使用 `new` 操作符，并把使用的包装函数叫做构造函数外，这个模式与之前的工厂模式是一样的；
    * 通过在构造函数末尾添加一个 `return` 语句，可以重写使用 `new` 调用构造函数时返回的值
    * 返回的对象与构造函数或者与构造函数的原型属性之间没有关系，不能依赖 `instanceof` 操作符来检测实例与构造函数的关系。

5. **寄生构造函数模式使用情景**
    > 当我们需要扩展原生构造函数的实例的属性和方法，在原生构造函数的记住上封装一个扩展版构造函数时使用

6. 实例：创建一个具有额外方法的特殊数组
    ```javascript
    function SpecialArray() {
        // 创建数组
        var values = new Array();

        // 添加值
        values.push.apply(values, arguments);

        // 添加额外方法
        values.toPipedString = function() {
            return this.joib('|');
        };

        // 返回数组
        return values;
    }

    var colors = new SpecialArray('red', 'blue');
    colors.toPipedString();
    ```
