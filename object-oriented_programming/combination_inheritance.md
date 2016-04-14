# 组合继承
1. 将原型链和构造函数借用组合使用；

2. **使用原型链实现对原型属性（公有）和方法（函数复用）的继承，通过构造函数借用实现对实例属性（个性化）的继承**；

3. 实例代码
    ```javascript
    function SuperType(name) {
        this.name = name;
        // 构造函数内确定引用类型属性，使每个实例拥有该副本
        this.colors = ['red', 'blue'];
    }

    SuperType.prototype.sayName = function() {
        alert(this.name);
    };

    function SubType(name, age) {
        // 构造函数借用以继承超类属性
        SuperType.call(this, name);

        this.age = age;
    }

    SubType.prototype = new SuperType();

    SubType.prototype.sayAge = function() {
        alert(this.age);
    };

    var instance1 = new SubType('yangfch3', 21);
    instance1.colors.push('green');
    instance1.colors; // ['red', 'blue', 'green']
    instance1.sayName(); // 'yangfch3'
    instance1.sayAge(); // 21

    var instance2 = new SubType('liuhq3', 21);
    instance2.colors; // ['red', 'blue']
    instance2.sayName(); // 'yangfch3'
    instance2.sayAge(); // 21
    ```

3. 优点：
    * 通过在原型上定义方法实现了函数复用
    * 通过构造函数借用保证了每个实例属性的各异性
    * 通过构造函数借用保证了每个实例拥有一个引用类型属性的副本，防止引用类型原型互扰

4. **组合继承** 融合了原型链、构造函数借用的优点，最常用的继承模式。
