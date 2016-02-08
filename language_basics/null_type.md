# Null 类型
1. `Null` 类型只有一个值 `null`（大小写含义不同，要区分）；

2. 逻辑角度：`null` 值表示一个空对象指针，所以用 `typeof` 检测返回 `'object'`；

3. `undefined` 值派生自 `null`，二者假性相等
    ```javascript
    console.log(undefined == null); // true
    console.log(undefined === null); // false
    ```

4. **最佳实践**：如果一个变量准备在将来用于保存对象，最好将该变量初始化为 `null`
    ```javascript
    var a = null; // a 初始化为 null 说明在之后的操作是用来保存对象的
    a = {foo: "bar"};
    ```
