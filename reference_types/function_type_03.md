# Function 类型(下)
## 函数属性和方法
1. `length`
    表示函数希望接收的命名参数的个数

    ```javascript
    function addSum(num1, num2, num3) {
        return num1 + num2 + num3;
    }

    alert(addSum.length); // 3
    ```

2. `prototype`
    对于 `ES` 中的引用类型， `prototype` 属性是保存他们所有实例方法的真正所在，是 `JS` 实现 `OOP` 的根基；

`prototype` 详细介绍见 **OOP** 一章

## apply()、call() 和 bind()
1. 为调用函数指定 **环境对象**，换句话说：设置函数体内 `this` 对象的值；

2. `apply()` 方法
    * 参数
        * 参数1：运行函数的作用域（环境对象）
        * （可选）参数2：参数数组（`Array` 实例或 `argemunts` 对象）

    ```javascript
    function sum(num1, num2) {
        return num1 + num2;
    }

    function applySum1(num1, num2) {
        return sum.apply(this, arguments); // 传入 arguments 对象，环境对象 this 指向 window
    }

    function applySum2(num1, num2) {
        return sum.apply(this, [num1, num2]); // 传入参数数组，环境对象 this 指向 window
    }

    function applySum3(num1, num2) {
        return sum.apply(null, [num1, num2]); //
    }

    applySum1(1, 1); // 2
    applySum2(1, 1); // 2
    applySum3(1, 1); // 2
    ```

3. `call()` 方法 -- 接收参数的方式与 `apply()` 不同
    * 参数
        * 参数1：运行函数的作用域（环境对象）；
        * （可选参数）参数n(n>1)：传递给函数的参数

    ```javascript
    function sum(num1, num2) {
        return num1 + num2;
    }

    function callSum1(num1, num2) {
        return sum.call(this, num1, num2);
    }

    function callSum2(num1, num2) {
        return sum.call(null, num1, num2);
    }

    callSum1(1, 1); // 2
    callSum2(1, 1); // 2
    ```

4. 何时使用 `apply()`，何时使用 `call()`
    > 取决于你采用哪种给函数传递参数的方式最方便

5. `apply()`、`call()` 的强大之处在于<span style="color:red">能够扩充函数赖以运行的作用域（环境对象）</sapn>
    ```javascript
    window.color = 'red';
    var o = {
        color: 'blue'
    };

    function showColor() {
        alert(this.color);
    }

    showColor(); // 'red'
    showColor.apply(this); // 'red'
    showColor.apply(null); // 'red'
    showColor.apply(o); // 'blue'
    ```

6. 使用 `apply()`、`call()` 扩充作用域的好处
    > 对象不需要与方法有任何耦合关系
    >
    > 对象和方法（函数）完全可以分离书写，调用时使用 `apply()`、`call()` 为函数指定 **环境对象（作用域）** 即可（如上示例）

7. `bind()`
    * 作用：创建函数实例，绑定环境对象
    * 返回值：<span style="color:red">一个绑定了环境对象的函数实例</span>
    * 参数：1 个，需要为函数绑定的环境对象
    * 兼容：**IE 9+**

    ```javascript
    window.color = 'red';
    var o = {
        color: 'blue'
    }

    function showColor() {
        alert(this.color);
    }

    var oShowColor = showColor.bind(o);
    oShowColor(); // 'blue'
    ```

## 其他方法
1. 函数的 `toString()`、`toLocaleString()` 方法都返回函数的代码，返回代码的格式（注释？改动？）因浏览器而异；

2. `valueOf()` 方法也返回函数代码。
