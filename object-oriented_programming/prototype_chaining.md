# 原型链
1. 利用原型链实现继承的基本思路：**利用原型让一个引用类型继承另一个引用类型的属性和方法**；

2. 构造函数、原型对象、实例的关系
    > 每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。

3. 实现原型链继承的基本思路：
    > 让原型对象等于另一个类型的实例，另一个原型又是另一个类型的实例，如此层层递进，就构成了实例与原型的链条。

4. 代码实例
    ```javascript
    // 上一级类型
    function SuperType() {
        this.property = true;
    }

    SuperType.prototype.getSuperValue = function() {
        return this.property;
    };

    // 下一级类型
    function SubType() {
        this.subproperty = false;
    }

    // 实现 SubType 对 SuperType 的继承
    SubType.prototype = new SuperType();

    SubType.prototype.getSubValue = function() {
        return this.subproperty;
    };

    // 创建 SubType 实例
    var instance = new SubType();
    alert(instance.getSuperValue()); // true
    ```

5. **注意点**
    * 实现的本质是重写原型对象；我们没有使用下一级构造函数提供的默认原型，而是给他换了一个新原型，这个新原型式上一级构造函数（类型）的一个实例（类型下的实例有一个内部指针指向构造函数的原型）
    * 要实现下一个类型对上一级类型的继承，只需要 **使下一级类型的原型对象指向上一级类型的一个实例** 即可；
    * 联通整个原型链的最重要的一环是：`Subtype.prototype = new SuperType();`

## 1. 别忘记默认的原型
1. 所有引用类型都继承了 `Object`；
    所有的构造函数都继承了 `Function`（当然 `Function` 构造函数的原型也继承了 `Object`）；

2. `Object` 是原型链的最高级；
    >其实，`Object` 构造函数的原型中的 `[[prototype]]` 指针指向 `null`，所以，我们也可以认为原型链的最顶端是 `null`，但一般不那么认为，因为 `null` 没有对应的构造函数。

3. 代码实例
    ```javascript
    function Person(name){
        this.name = name;
    }

    Person.prototype.__proto__; // Object {}

    Person.__proto__ === Function.prototype; // true
    // Person.constructor;

    Function.prototype.__proto__ === Object.prototype;

    Object.prototype.__proto__; // null
    ```

4. `SuperType`、`SubType` 以及默认原型链图（省略了构造函数作为普通引用类型实例的那条原型支链，见 6.2.3）
    ![img_1117.png-797.3kB][1]

## 2. 确定原型和实例的关系
1. 两种方式
    * `instanceof` 操作符
        * `person instanceof Person;`
    * `isPrototypeOf()` 方法
        * `Person.prototype.isPrototypeOf(person);`

2. 只要是原型链上出现过的原型，以上两种方式都返回 `true`。

## 3. 谨慎定义方法
1. **给原类型添加方法的代码一定要放在替换原型的语句之后**；

2. 在通过原型链实现继承时，不能使用对象字面量创建原型方法，这样会重写原型链，导致我们设想的原型链被切断。

## 4. 原型链的问题
* 原型对象的属性如果为引用类型（数组、对象），那么在实例上对原型对象上 **数组的元素、对象的属性** 进行的 **增删改写** 会使得所有实例都受到波及；

* 在创建子类型的实例时，不能向超类型的构造函数中传递参数。

所以，实践中很少单独使用原型链。


  [1]: http://static.zybuluo.com/yangfch3/71fzfb96pahx54huyvkbgtki/img_1117.png
