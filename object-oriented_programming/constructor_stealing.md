# 构造函数借用继承
1. 借用构造函数是为了解决原型中包含引用类型值带来的问题（见上一节最后）；

2. 又叫做 **伪造对象** 或 **经典继承**；

3. 基本思想：在子类型构造函数的内部调用超类型构造函数；

4. 代码实例
    ```javascript
    function SuperType() {
        // 将值为引用类型的属性调到构造函数内部，每一个实例都有了引用类型属性的一个副本，防止原型互扰
        this.colors = ['red', 'blue'];
    }

    function SubType() {
        // 调用父类构造函数，使继承 SuperType 属性
        SuperType.call(this);
    }

    var instance1 = new SubType();
    instance1.colors.push('green');
    instance1.colors; // ['red', 'blue', 'green']

    // 看其他实例有无受到影响，并没有
    var instance2 = new SubType();
    instance2.colors; // ['red', 'blue']
    ```

## 1. 传递参数
1. 在子类型构造函数中调用超类型构造函数时，可以传参；
    ```javascript
    function SuperType(name) {
        this.name = name;
    }

    function SubType() {
        // 继承了 SuperType，同时还进行了传参调用初始化
        SuperType.call(this, 'yangfch3');

        this.age = 21;
    }

    var instance = new SubType();
    instance.name; // 'yangfch3'
    instance.age; // 21
    ```

## 2. 构造函数借用的问题
1. 构造函数借用解决了引用类型属性互扰的问题；

2. 构造函数借用仍旧存在问题：**纯构造函数调用** 脱离了原型链，方法都定义在了构造函数内，每个实例都有一个方法的副本，函数也便无法复用；

3. 构造函数借用极少单独使用。
