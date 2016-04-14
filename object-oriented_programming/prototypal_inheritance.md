# 原型式继承
1. 区别 **原型链继承** 和 **原型式继承**；

2. **原型式继承** 的思想：不使用（严格意义上的构造函数），借助原型基于已有的对象创建新对象；

3. 实例代码
    ```javascript
    function object(o) {
        // 创建一个临时性的工作函数
        function F(){}

        // 将传入的对象做为这个构造函数的原型
        F.prototype = o;

        // 返回这个临时类型的新实例
        return new F();
    }
    ```

4. <span style="color: red">本质上 `object()` 是对传入其中的 `o` 的一次 **浅复制**</span>；

5. 原型式继承要求：必须有一个对象（o）可以作为另一个对象的基础
    ```javascript
    function object(o) {
        // 创建一个临时性的工作函数
        function F(){}

        // 将传入的对象做为这个构造函数的原型
        F.prototype = o;

        // 返回这个临时类型的新实例
        return new F();
    }

    var person = {
        name: 'yangfch3',
        friends: ['aaa', 'bbb']
    };

    var anotherPerson = object(person); // 相当于创建（复制）了一个 person 的副本
    abotherPerson.name = 'liuhq3';
    anotherPerson.friends.push('ccc');
    ```

6. ES 5 新增 `Object.create()` 原生方法规范了原型式继承，接收两个参数：
    * 用作新对象原型的对象
    * 为新对象定义额外属性的对象

    ```javascript
    var person = {
        name: 'yangfch3',
        friends: ['aaa', 'bbb']
    };

    // 第二个参数与 `Object.defineProperties()` 参数格式一致
    var person1 = Object.create(person, {
        name: {
            value: 'liuhq3',
            configurable: true
        }
    })
    ```

7. **注意点**：原型式继承中，包含引用类型值的属性始终会共享相应的值，**源对象** 上引用类型值属性的任何修改会对 **浅复制** 的新实例造成影响。
