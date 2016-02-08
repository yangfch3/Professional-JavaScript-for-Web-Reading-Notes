# 乘性操作符
1. `ES` 中的乘性操作符与 `Java` `C` `Perl` 等类似，多了一个特性：**操作数非数值时会执行自动的类型转换（`Number()` 转化）**；

2. 乘法运算特殊规则：
    * `x*y` 乘积超出 `ES` 数值的表示范围返回 `Infinity` 或 `-Infinity`
    * `x*NaN` 结果为 `NaN`
    * `Infinity*0` 结果为 `NaN`
    * `Infinity*Infinity` 结果为 `Infinity`

3. 除法运算特殊规则
    * `x/y` 乘积超出 `ES` 数值的表示范围返回 `Infinity` `-Infinity`
    * `x/NaN` `NaN/x` 结果为 `NaN`
    * `Infinity/Infinity` 结果为 `NaN`
    * `0/0` 结果为 `NaN`

4. 求模运算符 `%`
    * 一般的非法特殊运算返回 `NaN`
    * `0/n; // n 为任何有效数值` 结果为 0
    * `x/Infinity` 结果为 `x`

5. 求模运算常用于检验奇偶数。
