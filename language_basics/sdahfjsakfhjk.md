#逻辑（布尔）运算符
![][1]

1. 两个逻辑非操作模拟 `Boolean()` 转型函数
```javascript
!!NaN; // flase
!!null; // false
!!undefined; // false
```
2. 多个操作值的逻辑与运算
    > **可以将多个逻辑与运算想象为一个产品在检测流水线上多个检测的过程，遇到 `true` 则通过，遇到 `false` 则停留在该步骤不再往下，一直 `true` 则返回最后一次检测 -- <span style="color:red">短路操作</span>**。如：

    ```javascript
    "123" && "abc" && null && undefined && {}; // true&&true&&false...
    > null

    null && "123" && {}; // false&&...
    > null

    "123" && {} && "abc"
    > 'abc'

    ```
3. 多个操作值的**逻辑或**运算
    > 原理类似多个操作值的**逻辑与**运算，只是遇到 `true` 才返回，一直 `false` 则返回最后一个

4. 由于短路操作，当操作值里有未定义的变量时可能不会报错（短路操作在这个操作值前便已终止）：
    ```javascript
    var found = 'false';
    var result = (found && someUndefinedVar); // 不会发生报错，短路操作，found 即返回，不再往下运算
    alert(result); // "false"

    var found = 'hello world!';
    var result = (found || someUndefinedVar); // 不会报错，found 为 true，逻辑或操作返回 found
    alert(result); // "hello world!"
    ```

5. **最佳实践**：利用逻辑或来避免为变量赋值 `null` 或 `undefined`
    ```javascript
    var myObject = preferredObject || backupObject; // 设定一个备选值，避免被默认赋值为 `null` 或 `undefined`
    ```

[1]:http://static.zybuluo.com/yangfch3/5rjqym1mtzzr9hch7zr5d4le/2016-02-06_111358.jpg

