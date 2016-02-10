# Array Type(下)

## 迭代方法
1. `ES 5` 为数组定义了 5 个迭代方法；

2. 每个方法接受 **2 个参数**：
    * 要在每一项上运行的函数；
    * （可选）运行该函数的作用域对象 -- 影响 `this` 的值。

3. 作用函数接收 **3 个参数**：
    * 数组项的值；
    * 该项在数组中的位置；
    * 数组对象本身。
<br>
<br>
4. 五个迭代操作
    * `every()`：对数组的每一项运行给定的函数，如果该函数对每一项运行都返回 `true` 或转换后为 `true` 的值，则 `every()` 操作 <span style="color:red">**返回 `true`，否则返回 `false`**，对原函数无影响</span>；
<br>
<br>
    * `some()`：对数组中的每一项运行给定 ，如果该函数对任一项返回 `true` 或转换后为 `true` 的值，则返回 `true`；
<br>
<br>
    * `filter()`：对数组的每一项运行给定函数，<span style="color:red">返回该函数会**返回** `true` 的项组成的 **数组**，</span>；
<br>
<br>
    * `forEach()`：对数组中的每一项运行给定函数。<span style="color:red">无返回值，但会对原函数的每一项进行相应的改变</span>相当于用 `for` 循环对数组元素遍历；
<br>
<br>
    * `map()`：对数组中的每一项运行给定函数，<span style="color:red">返回每次函数调用的结果组成的数组；

    ```javascript
    var numbers = [1, 2, 3, 4, 3, 2, 1];

    function operation(item, index, arr) {
        return (item - arr[2]); // item - 3
    }
    // every
    var everyResult = numbers.every(operation);
    alert(everyResult); // false

    // some
    var someResult = numbers.some(operation);
    alert(someResult); // true

    // filter
    var filterResult = numbers.filter(operation);
    filterResult; // 1,2,4,2,1
    numbers; // filter 不对原函数造成影响

    // forEach
    var forEachResult = numbers.forEach(function pop(item, index, arr) {
        alert('index' + index + ': ' +item);
        // idnex0:...
    });

    // map
    var mapResult = numbers.map(function (item, index, arr) {
        return item*index+arr.length;
    });
    numbers; // map 操作对原数组不影响
    mapResult; // [7, 9, 13, 19, 19, 17, 13]
    ```

## 缩小方法
1. `ES 5` 新增两个缩小数组的方法：`reduce()` `reduceRight()`；

2. 分别从左（右）**迭代** 遍历数组的所有项，构建一个最终的返回值；

3. 参数
    * 对每一项运行的函数；
    * （可选）做为缩小基础的初始值。

4. 参数函数的 4 个参数
    * 前一个值
    * 当前值
    * 项的索引
    * 数组对象本身

    ```javascript
    var numbers = [1, 2, 3, 4, 5];

    var sum0 = numbers.reduce(function (pre, cur, index, arr) {
        return pre + cur;
    })

    alert(sum0); // 15

    var sum1 = numbers.reduce(function (pre, cur, index, arr){
        if (cur > 3) {
            return pre + cur;
        }
        return 0;
    })

    alert(sum1); // 9
    ```
5. 说明：第一次执行函数时，数组的第一项是 `pre`，第二项是 `cur`，**执行返回值** 作为新的 `pre` 继续迭代。
