# 组合使用构造函数模式和原型模式
1. 创建自定义类型的最常见方式：**组合使用构造函数模式与原型模式**；
    * 构造函数模式用于定义 **个性化的实例属性**
    * 原型模式用于定义 **共享的方法和共享的属性**

2. 代码实例
    ```javascript
    function Person(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
        this.friends = [];
    }

    Person.prototype = {
        construtor: Person,
        sayName: function() {
            alert(this.name);
        }
    }

    var person1 = new Person('yangfch3', 21, 'UI Dev');
    var person2 = new Person('liuhaq3', 21, 'Designer');

    // 在构造函数内定义的个性化的属性，不会出现纯原型模式的波及情况，因为每个实例都会有自己独立的引用类型对象
    person1.friends.push('aaa');
    person2.friends.push('bbb');
    ```

3. **注意点**：上面 `friends` 属性写在构造函数内，所以每个实例都会有自己的一份副本；
一般在实例上特殊的、不共享的属性和方法在构造函数内定义，共享、关联的属性和方法在原型上定义；

4. **构造函数与原型混成的模式** 是现在使用最广泛、认同度最高的一种创建自定义类型的方法。
