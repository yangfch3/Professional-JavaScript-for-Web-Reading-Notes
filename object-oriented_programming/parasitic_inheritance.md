# 寄生式继承
1. 寄生式继承：原型式继承寄生在封装的函数内部；
    > 创建一个仅用于封装继承与扩展过程的函数；函数内部首先使用原型式继承克隆一个对象；然后对这个对象的属性和方法进行扩展。

2. 代码实例：
    ```javascript
    // 原型式继承封装函数
    function object(o) {
        // 创建一个临时性的工作函数
        function F(){}

        // 将传入的对象做为这个构造函数的原型
        F.prototype = o;

        // 返回这个临时类型的新实例
        return new F();
    }

    // 寄生式继承封装函数
    function createAnother(original) {
        // 使用原型式继承创建一个新对象
        var clone = object(original);

        // 为新对象扩展属性与方法
        clone.sayHi = function() {
            alert('Hi');
        };

        // 返回对象
        return clone;
    }

    // 使用寄生式继承封装函数
    var person = {
        name: 'yangfch3',
        friends: ['aaa', 'bbb']
    };

    var anotherPerson = createAnother(person);
    anotherPerson.sayHi();
    ```

3. 什么情况下使用 **寄生式继承**
    > 在主要考虑对象，而不是自定义类型和构造函数的情况下

4. **寄生式继承** 的缺点：不能做到函数（方法）复用。
