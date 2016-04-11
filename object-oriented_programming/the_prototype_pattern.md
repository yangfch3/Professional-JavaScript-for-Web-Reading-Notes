# 原型模式
1. 每个函数都有一个 `prototype` 属性；

2. `prototype` 属性是一个指针，指向一个对象，**这个对象的用途是包含可以由特定类型所有实例共享的属性和方法**；

3. `prototype` 在默认情况下（无改写）就是通过调用构造函数而创建的那个对象实例的原型对象；

4. 代码示例
    ```javascript
    function Person() {
    }

    Person.prototype.name = 'yangfch3';
    Person.prototype.age = 21;
    Person.prototype.sayName = function() {
        alert(this.name);
    }

    // 使用
    var person1 = new Person();
    var person2 = new Person();
    person1.sayName();
    person2.sayName();
    ```

## 1. 理解原型对象
1. 只要创建一个函数，就会 **根据一组特定的规则** 为该函数创建一个 `Prototype` 属性，指向函数的原型对象；

2. 默认情况（未改写）下，所有原型对象都会自动获得一个 `construtor` 属性，指向 `prototype` 属性所在的函数；

3. 使用构造函数创建一个新实例后，该 **实例的内部包含一个指针**（`[[prototype]]` 或者说 `__proto__`），指向构造函数的原型对象；
    >部分浏览器支持使用在每个对象上调用 `__proto__` 以访问实例对象的 `[[prototype]]`。
而在部分浏览器中，构造函数的原型对象对于其实例是不可见（访问）的。

4. 图片：构造函数、原型对象、实例之间的关系图
![IMG_4404.JPG-976.6kB][1]

5. **注意点**：图中需要注意的点
    * `Person.prototype` `Person.prototype.construtor` 刚好一个指向原型对象，一个指向构造函数；
    * `Person` 的每个实例与构造函数没有直接的关系，每个实例内部的 `[[prototype]]` 指针指向构造函数的原型对象；
    * **构造函数本身也是 `Object` 构造函数的一个实例**；
    * 通过原型链的查找，`Person` 的每个实例不仅可以调用实例本身的方法，还可以调用其构造函数的原型上的属性和方法，更可以上溯调用 `Object` 构造函数原型对象上的属性和方法。

6. 无法直接访问到实例的 `[[prototype]]`，可以通过 `isPrototypeOf()` 方法确定对象之间的是否存在 **原型类关系**；
    ```javascript
    Person.prototype.isPrototypeOf(person1); // true
    ```

7. ES 5 规定可以通过 `Object.getPrototypeOf()` 访问对象（实例）的 `[[prototype]]` 方便地取得一个对象的原型；
    ```javascript
    Object.getPrototypeOf(person1) == Person.prototype; // true
    ```

8. 对象属性访问原则：原型链上溯搜索法则
    * 在实例本身搜索属性（短路原则）；
    * 继续搜索指针指向的原型对象（即沿 `[[prototype]]` 线路）上溯

9. 原型对象上的属性可以被实例的同名属性屏蔽；

10. 使用 `hasOwnProperty()` 方法可以检测一个属性是存在于实例中（`true`），还是存在于原型中或不存在（`false`）；

## 2. 原型与 in 操作符
1. `in` 操作符的两个使用情景
    * `for-in` 循环中使用
    * 单独使用

    ```javascript
    // 1
    for(var prop in person1) {
        // prop...
    }

    // 2
    propName in person1; // true or false
    ```

2. `for-in` 循环时，会访问所有能通过对象访问（实例与原型上）的、**可枚举的属性**；
`in` 操作符单独使用时，无论该属性存在于实例中还是原型中都返回 `true`；

3. `hasOwnProperty()` 只在属性存在于实例时才返回 `true`；

4. 如何确定一个对象能访问的属性在原型上？
    > 只要 `in` 操作符返回 `true` 而 `hasOwnProperty()` 返回 `false`，就可以确定属性是原型的属性

5. IE 8 及以下 `Bug`：
    >屏蔽 **原型中不可枚举属性** 的 **实例属性** 不会出现在 `for-in` 循环中。

6. ES 5 的 `Object.keys()` 方法，获取所有 **可枚举属性**（包括实例与原型上的） 的 **字符串数组**；

7. `Object.getOwnPropertyNames(obj)` 可获取所有属性，无论处于原型链还是对象实例本身，无论是否可以枚举。

## 3. 更简单的原型语法
1. 用一个包含所有属性和方法的对象字面量来重写整个原型对象；

2. **注意点**：使用对象字面量重写原型后，原型的 `constructor` 会指向 `Object`，记得复原；

3. 代码
    ```
    function Person() {
    }

    Person.prototype = {
        // 还原 constructor
        construtor: Person,

        name: 'yangfch3',
        age: 21,
        sayName: function() {
            alert(this.name);
        }
    }
    ```

4. 重置 `construtor` 属性会导致原型的 `construtor` 属性的 `[[Enumerable]]` 变为 `true`（默认情况下为 `false`），可以使用 `Object.defineProperty()` 方法详细规定 `construtor` 属性的特性。

## 4. 原型的动态性
1. 我们对原型对象属性与方法的任何修改都能立即从实例上反映出来；

2. 牢记：实例与原型之间是通过 **指针** 松散连接的；

3. 重写整个原型对象会切断构造函数与最初原型之间的联系，在重写之前与之后的对象实例的 `[[prototype]]` 指针指向不同的原型对象。
    > 重写原型对象之前创建的实例的 `[[prototype]]` 指针仍旧指向旧的原型对象，重写原型之后创建的实例的 `[[prototype]]` 则指向新的原型对象
![01.png-104.8kB][2]

## 5. 原生对象的原型
1. 所有原生的引用类型也是基于原型模式定义与创建的；

2. 引用类型实例共享的属性或方法都定义在引用类型构造函数的原型对象上；

3. 我们可以很自由地为引用类型的原型添加属性和方法，改写原有的属性或方法，这一点在做浏览器兼容时时常用到。

## 6. 原型对象的优缺点
1. 优点
    * 方便地实现了继承
    * 解决了引用类型复用的问题（同时也引发了新的问题，见缺点第二点）

2. 缺点
    * 纯原型对象模式省略了为构造函数传递初始化参数的环节，所有实例都没有了特异性
    * 原型对象的属性如果为引用类型（数组、对象），那么在实例上对原型对象上 **数组的元素、对象的属性** 进行的 **增删改写** 会使得所有实例都受到波及

    ```javascript
    function Person() {
    }

    Person.prototype = {
        construtor: Person,
        name: 'xxx',
        age: 21,
        friends: ['aaa', 'bbb'],
        job: {
            where: 'tencent',
            post: 'UI Dev'
        },
        sayName: function() {
            alert(this.name);
        }
    }

    var person1 = new Person();
    var person2 = new Person();

    person1.friends.push('ccc');
    person1.job.beginningTime = '2016/07/01';

    alert(person2.firends);
    alert(person2.job.beginningTime);
    alert(Person.prototype.friends);
    alert(Person.prototype.job.beginningTime);
    ```


  [1]: http://static.zybuluo.com/yangfch3/88r4rl8ms18eykwx6ymmcg3z/IMG_4404.JPG
  [2]: http://static.zybuluo.com/yangfch3/zg4dpsqo2udhg5vt2sqpoivb/01.png
