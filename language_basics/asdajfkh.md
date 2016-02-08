# 加性操作符
1. 操作数非数值时会执行自动的类型转换（`Number()` 转化）；

2. 不仅用于数值相加，还可用于字符串拼接；

3. `x + NaN` 结果为 `NaN`；
`Infinity + -Infinity` 结果为 `NaN`；
`null` 与 `undefined` 作为操作数时，会进行 `String()` 转化为 `'null'` `'undefined'`

4. 操作数为对象时对象即会经历我们熟悉的 `valueOf() -- toString()` 过程
