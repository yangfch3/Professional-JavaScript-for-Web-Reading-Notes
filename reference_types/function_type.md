# Function 类型(上)
1. 函数是对象，**函数名仅仅是指向函数对象的指针**；

2. **生僻知识点**：使用 `Function` 构造函数定义函数
    * 参数
    任意多个，最后一个参数被视为函数体
    ```javascript
    var sum = new Function('num1', 'num2', 'num3', 'return num1 + num2 + num3');

    sum(1, 2, 3); // 6
    ```

3. 使用构造函数定义函数优缺点
    * 优点：直观，服从“函数是对象，函数名是指针”的原则
    * 缺点：导致两次解析，影响性能

## 没有重载
1. 松散的 `ES` 中的函数没有函数签名的概念，也就没有重载；
    见链接：[Functions][1]

## 函数声明与函数表达式
```javascript
// 函数声明
alert(sum(1, 1)); // 2
function sum(num1, num2) {
    return num1 + num2;
}
```
```javascript
// 函数表达式
alert(sum(1, 2)); // sum is not a function
var sum = function (num1, num2) {
    return num1 + num2;
}
```

1. `function declaration hoisting` -- 函数声明提升；

2. `JavaScript` 引擎优先处理函数声明，提升至源代码的顶部；

3. 函数表达式只会发生变量提升。

## 作为值的函数 -- 以变量的眼光看
1. 函数名本身即是变量，函数可以方便作为普通类型值使用，可以作为参数传递给另一个函数；
    ```javascript
    function callSomeFunction (someFunction, someArgument) {
        return someFunction(someArgument);
    }
    ```

2. 可以 **将一个函数作为另一个函数的返回值返回**，这是一种极为有用的技术
    ```javascript
    // 目的：依据数组内对象的某个属性进行数组对象项的排序
    var person1 = {
        name: 'liuhq3',
        age: 21
    }
    var person2 = {
        name: 'yangfch3',
        age: 20
    }

    var person = [person1, person2]; // [Object, Object]

    function createComparisonFunction(propertyName) {
        // object1 object2 是数组中的每一个对象项
        return function (object1, object2) {
            var value1 = object1[propertyName];
            var value2 = object2[propertyName];

            /*
             * 下面的 if 可简写为：
             * return value1 - value2;
             * 即可
             */
            if (value1 < value2) {
                return -1;
            } else if (value1 > value2) {
                return 1
            } else {
                return 0;
            }
        }
    }

    // 创建一个依据 age 排序的比较函数；
    person.sort(createComparisonFunction('age')); // [person2, person1]

    person.sort(createComparisonFunction('age')); // [person2, person1]
    ```

3. 上面的代码展示了：我们可以很方便地根据数组项对象的不同属性进行排序

  [1]: https://yangfch3.gitbooks.io/professional-javascript-for-web-reading-notes/content/language_basics/functions.html
