# 数据类型与检测
1. **5 简 1 复**
五种基本数据类型：`Undefined` `Null` `Boolean` `Number` `String`
一种复杂数据类型：`Object`
> 在作为数据类型（原始对象或原始构造函数）时以上六个标识符全为 **首字母大写**
>
>**小 Tips**：各个数据类型自己本身都可以作为一个构造函数（如：`String()` `Boolean()` `Object()`），可选地接受参数，生成相应类型的 **对象**

2. **`typeof` 是一个操作符，不是一个函数**，所以写法为 `typeof sth`，而不是 ~~`typeof(sth)`~~；

3. `typeof` 操作符返回的是以下 **六个小写字符串** 中的一个：`undefined` `boolean` `number` `string` `object` `function`

4. `typeof null` 返回结果为 `'object'`；因为特殊值 `null` 是一个**空的对象引用**；

5. `typeof` 检测函数时返回的不是 `'object'` 而是 `'function'`；
    >从以上可知，`typeof` 可以检测函数类型对象，但是对于细分对象的精确检测及对象与 `null` 的检测欠缺能力

6. `ES-262` 规范规定任何在内部实现 `[[call]]` 方法的对象都应在使用 `typeof` 检测时返回 `function`

6. 使用 `typeof` 对正则表达式进行检测时，在不同类型不同版本浏览器里结果不太一致。旧版 `chrome` 与 `safari` 返回 `function`，`IE` 和 `FF` 下返回 `object`，现在大部分现代浏览器是返回 `object`。
