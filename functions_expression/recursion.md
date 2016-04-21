# 递归
1. 递归函数是在一个函数中调用自身；

2. 函数内部的 `argument.callee` 是一个指向正在执行函数的指针，非严格模式下，使用 `argument.callee` 来实现递归可以避免函数名变更带来的混乱；
    ```javascript
    function factorial(num) {
        if(num <= 1) {
            return 1;
        } else {
            return num*arguments.callee(num - 1);
        }
    }
    ```

3. 严格模式下，无法使用 `argument.callee`，此时使用命名函数表达式来维护递归函数的稳定性
    ```javascript
    var factorial = function f(num) {
        if(num <= 1) {
            return 1;
        } else {
            return num * f(num-1);
        }
    }
    ```
