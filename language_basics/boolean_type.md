
# Boolean 类型
1. `true` 和 `false` 两个 `Boolean` 类型值区分大小写；

2. **`new Boolean(sth)`**
`Boolean` 作为 `Boolean` 对象的生成器（构造函数），接受任何值作为参数，生成一个 `Boolean` 对象；不传值则返回 `[Boolean:false]`
    ```javascript
    var a = new Boolean(null); // [Boolean:false] 一个 Boolean 对象
    a == false; // true
    a === false; // false
    typeof a; // 'object'
    ```

3. `Boolean(sth)`
转型函数 `Boolean()` 可以将任何值转换为布尔类型的值
    ```javascript
    var a = "Hello";
    var b = Boolean(a); // b ---> true
    b === false; // true
    typeof b; // 'boolean'
    ```

4. `Boolean()` 转换为 `false` 的值：`""` `0` `NaN` `null` `undefined`

5. 流程控制语句（如 `if`）条件判断中会自动隐士地执行相应的 `Boolean` 转换

6. **最佳实践**：
    1. 不使用 `new Boolean(sth)`来生成 `Boolean` 对象，一般不用 `Boolean` 对象；
    2. 可以使用 `!` 运算符来进行 `Boolean` 转换，推荐尝试写法：
    ```javascript
    var a = "hello";
    if(!!a){ // 两次取反，将一个值转变为对应的 boolean 值
        alert("Value is true");
    }
    ```
