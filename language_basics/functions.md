# 函数
通过函数可以封装任意多条语句、任何时候调用执行。

1. `ES` 中函数不必指定是否返回值，无返回值时默认返回 `undefined`；`return` 用于返回值与终止函数执行；`return ;` 常用于终止函数执行
    ```javascript
    function test() {
        // ...do something
    }
    ```

2. **最佳实践**：要么让函数始终都返回一个值（不是 `undefined`，可以约定一个固定值用于不需要返回的函数返回），要么永远不要返回值（返回 `undefined`），方便测试

### 参数
1. 理解参数的方法是：**将命名参数视作函数的局部变量**；

2. `ES` 可以接收任意多个参数，多余或少于等于命名参数皆可。`arguments` 的长度是由传入的参数个数决定的，不是由定义函数时的命名参数个数决定的；

3. 传入的参数在函数内以 `arguments` 对象形式（类数组）形式存在；

4. **命名参数只是提供便利，不是必需的**！ -- 原因见 2
    命名参数与 `arguments` 对象可以在函数内一起使用！

5. 非严格模式下：`argunemts` 的值永远与对应命名参数的值保持 **同步**；`arguments` 对象的值会自动反映到对应的命名参数，<span style="color:red">二者在非严格模式下能互相改写</span>；
    >注意：`arguments` 的值与命名参数的 **内存空间独立**，但值会互相同步：

    ```javascript
    function addSum(num1, num2) {
        arguments[0] = 10; // 同步，arguments[0] <---> num1 ---> 10
        num2 = 10; // num2 <---> arguments[1] 10
        alert(num1 + num2); // 20
        alert(arguments[0] + ' ' + arguments[1]); // 10 10
    }

    addSum(1, 1); // 20     10 10
    ```
6. 没有传递值的命名参数会被自动赋予 `undefined`，类似定义了变量（参数视为局部变量）没有初始化一样；

7. 严格模式下重新给命名参数赋值的操作不会导致 `arguments` 的值被重写；
    ```javascript
    function addSum(num1, num2) {
        'use strict'
        alert('num1：' + num1); // 1
        alert('num2：' + num2); // 1
        alert('arguments[0]：' + arguments[0]); // 1
        alert('arguments[1]：' + arguments[1]); // 1
        alert('num1 + num2：' + (num1 + num2)); // 2
        alert('arguments[0] + arguments[1]：' + (arguments[0] + arguments[1])); // 2

        num1 = 10;
        num2 = 10;
        arguments[0] = 100;
        arguments[1] = 100;

        alert('Rewrote, please watch continue!')
        alert('num1：' + num1); // 10
        alert('num2：' + num2); // 10
        alert('arguments[0]：' + arguments[0]); // 100
        alert('arguments[1]：' + arguments[1]); // 100
        alert('num1 + num2：' + (num1 + num2)); // 20
        alert('arguments[0] + arguments[1]：' + (arguments[0] + arguments[1])); // 200
    }

    addSum(1, 1);
    ```

8. **最佳实践**：严格模式下不要在函数内重写 `arguments` 对象的值

### 签名与重载
1. 函数签名（`ES` 无函数签名）：必需遵守的函数接受的参数的类型和数量
    >在其他语言（如 `Java` 中），为一个**同名的函数**编写**多个定义**，只需要这两个定义的**签名不同**即可

2. 重载（`ES` 无重载）：因为函数签名的不同，一个命名可以定义多个函数，依据调用时传入的参数类型和数量决定执行的方向

3. 伪重载（`ES` 实现重载的手段）：检查传入函数中参数的类型和数量并作出不同的反应，可以模仿其他语言函数的重载
