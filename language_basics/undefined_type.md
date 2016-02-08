### Undefined 类型
1. `Undefined` 类型只有一个值 `undefined`（**小写的代表值，大写的代表数据类型**，需要注意）；

2. 使用 `var` 声明变量，但未对其初始化时，这个变量的值就是 `undefined`。
    ```javascript
    var message;
    alert(message === undefined); // true
    ```

3. `undefined` 值的出现是为了正式区分 空对象指针（`null`）与未经初始化的变量

4. 对于尚未声明，也没有赋值的变量（莫名出现的变量），只支持一个操作：`typeof`
    ```javascript
    var a;
    alert(b); // error:e is not defined
    alert(typeof b); // undefined
    ```
5. **最佳实践**：无论什么情况下，都没有必要吧一个变量的值显式地设置为 `undefined`
    ```javascript
    var a = undefined; // 不推荐这样写
    ```
