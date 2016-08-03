# 寄生组合式继承
1. 组合继承的不足之处
    > 无论什么情况下，都会调用两次超类型构造函数；第一次是在 **子类型构造函数** 中，调用 **超类型构造函数** 以继承超类型部分属性；第二次是调用 **超类型构造函数** 创建超类型实例，并改写 **子类型构造函数** 的 `prototype` 指向超类型实例。

2. 不足之处代码实例
    ```javascript
    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue'];
    }

    SuperType.prototype.sayName = function() {
        alert(this.name);
    };

    function SubType(name, age) {
        // 第二次调用
        SuperType.call(this, name);

        this.age = age;
    }

    // 第一次调用
    SubType.prototype = new SuperType();
    SubType.prototype.constructor = SubType;

    SubType.prototype.sayAge = function() {
        alert(this.age);
    };
    ```

3. **寄生组合式继承**：**构造函数借用** 来继承属性，原型链的混成形式来继承方法；
    基本思路是：使用 **寄生式继承** 来复制超类型的原型，然后再将结果指定给子类型的原型

4. 寄生组合式继承代码实例
    ```javascript
    // 实现子、超类型继承函数封装
    function inheritPrototype(SubType, SuperType) {
        var prototype = Object.create(SuperType);
        prototype.constructor = SubType;
        SubType.prototype = prototype;
    }

    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue'];
    }

    SuperType.prototype.sayName = function() {
        alert(this.name);
    };

    function SubType(name, age) {
        SuperType.call(this, name);

        this.age = age;
    }

    inheritPrototype(SubType, SuperType);

    SubType.prototype.sayAge = function() {
        alert(this.age);
    }
    ```

5. 原型链变化情况
    ![img_1119.png-619.4kB][1]

6. **寄生组合式继承** 的优点
    * 高效：只调用了一次 `SuperType` 构造函数（相对于单纯的 **组合继承**）
    * 避免了在 `SubType.prototype` 上创建不必要、多余的属性
    * 原型链得到了很好的保持

7. **寄生式组合继承是最理想的继承范式**。


  [1]: http://static.zybuluo.com/yangfch3/bdv6oqb3ytdeawcgyy3ol8yq/img_1119.png
