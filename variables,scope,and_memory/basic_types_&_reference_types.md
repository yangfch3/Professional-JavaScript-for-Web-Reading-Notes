# 基本类型和引用类型
1. 从第三章知：数据类型分为两种 -- 基本类型值与引用类型值；

2. 5 种基本数据类型按值访问，我们可以直接操作保存在变量中的实际的值；
    >在许多语言中，字符串（`String type`）以对象的形式存在，因此被认为是引用类型；而 `ES` 将 `String type` 放在了基本数据类型中。

3. `JS` 无法直接操作对象的内存空间，我们在操作对象时，实际上是在操作对象的引用而非实际的对象；
    ```javascript
    var obj = {foo: "bar"}; // obj 是 {foo: "bar"} 的一个引用

    {foo: "bar"}.bar = "foo"; // error
    alert(obj.bar);

    obj.bar = "foo"; // 通过引用操纵对象
    alert(obj.bar); // "foo"
    ```

4. 我们不能给基本类型的值添加属性，无效但不报错；
    ```javascript
    var a = '123';
    a.foo = 'bar';
    alert(a.foo); // undefined
    ```

## 复制变量值
基本类型值与引用类型在复制变量值时存在不同
```javascript
var a = 5;
var b = a; // 新创建一个基本类型值 5 ，再把 5 赋值给 b

var obj1 = {};
var obj2 = obj1; // 创建一个指向 {foo: 'bar'} 指针，将指针复制给 d
var obj1.foo = 'bar'; // 修改了指针指向的引用实体
alert(obj2.foo); // 'bar'
```

## 传递参数
1. `ES` 中所有函数的参数都是 **按值传递** 的；

2. 把函数外部的值复制给函数内部的参数（视为函数内的局部变量），等同于：把值从一个变量复制到另一个变量；
    ```javascript
    function addSum(num1, num2) {
        return (num1 + num2);
    }

    var foo = 10;
    var bar = 20;

    addSum(foo, bar); // 30
    ```

3. 向参数传递引用类型的值时：把这个值（对象）在内存中的地址复制给函数的参数（局部变量）；

4. 由于传递的是引用类型的时候，会出现外部变量与函数参数（局部变量）共享同一个对象的内存地址（指针指向相同），因此在函数内部对引用类型值进行修改会影响到函数外面；
    ```javascript
    var person = {name: "yangfch3"};

    function addAge(foo) {
        foo.age = 20; // 函数内修改参数对应对象的属性
    }

    addAge(person);
    alert(person.age); // 20
    ```

## 检测类型
1. `typeof` 的缺陷：
    * 无法分辨 `null` 与 `object`；
    * 无法更加细致的区分不同的 `Object` 子类型。

2. `instanceof` -- 是否为某个对象类型的实例
    ```javascript
    var colors = ['yellow', 'red', 'green'];

    alert(colors instanceof Array); // true
    alert(colors instanceof Object); // true
    ```
