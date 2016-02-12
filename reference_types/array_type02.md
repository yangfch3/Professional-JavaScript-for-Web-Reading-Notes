# Array Type(中)
## 重排序方法
1. `reverse()` -- 反转 和 `sort()` -- 排序，返回的是排序后的数组；

2. `sort()` 方法 **无参数** 时：每个数组项调用自身 `toString()` 方法转为 **字符串**，再按字符编码升序（非数值大小）排列数组项；
    ```javascript
    var values = [0, 1, 5, 10, 15];
    values.sort();
    alert(values); // [0, 1, 10, 15, 5]
    ```
3. `sort()` 方法可以接收一个比较函数作为参数，用以指定哪个值位于哪个值前面；

4. 比较函数的写法：接收任意名字的两个参数，**表征数组中的项**，如果第一个参数应该位于第二个之前（`<`）则返回一个负数，如果两个参数相等（`=`）则返回0，如果第一个参数应该位于第二个之后（`>`）则返回一个正数：
    ```javascript
    // 数值升序排序的写法
    function compare(value1, value2) {
        if (value1 < value2) {
            return -1; // 需要反序时 return 1
        } else if (value1 > value2) {
            return 1; // 需要反序时 return -1
        } else {
            return 0;
        }
    }

    var values = [0, 1, 10, 15, 5];
    values.sort(compare); // [0, 1, 5, 10, 15]
    ```

5. 数值类型或 `valueOf()` 方法返回数值类型的对象数组项，可使用下面的方法简写达到上面繁琐的比较函数相同的效果：
    ```javascript
    function compare(value1, value2) {
        return (value2 - value1);
    }
    ```

## 操作方法
* `arr.concat()` 合并
    * 作用：先创建当前数组的一个 **副本**，然后将接收到的参数添加到这个副本的末尾，最后 **返回新构建的数组**
    * 参数（可以任意多个）
        * 无参数：复制当前数组并返回副本；
        * 参数为数组：将这些数组的每一项都添加到结果数组中；
        * 参数为非数组：这些值简单地添加到数组的末尾

    ```javascript
    var colors = ['red', 'green'];
    var colors2 = colors.concat('white', ['black', 'brown']);
    alert(colors); // 'red,green'
    alert(colors2); // 'red,green,white,black,brown'
    ```
* `arr.slice()` 切片
    * 作用：接受参数，依据参数对数组截取数组区间，以新数组的形式返回；
    * 参数（无、一到两个）
        * 无参数：返回原数组
        * 一个参数：返回从该参数指定位置开始到数组末尾的所有项组成的数组；
        * 两个参数：分别代表起始和结束位，返回起始和结束之间的项组成的新数组；

    ```javascript
    var colors = ['red', 'green', 'blue', 'yellow', 'purple'];
    var colors2 = colors.slice(2);
    var colors3 = colors.slice(1, 4);

    alert(colors); // slice() 方法不会影响原数组
    alert(colors2); // 'blue,yellow,purple'
    alert(colors3); // 'green,blue,yellow'
    ```
    几点注意：

    * `slice()` 方法中有一个负数，则用数组长度加上该数来确定相应位置 `length = 5`  `slice(-2, -1)` 时会转换为 `slice(3, 4)`；
    * 如果结束位置小于起始位置，则返回空数组

* `splice()` -- 接合，**一定对原数组造成影响**
    功能三合一：**删除、插入、替换**；返回的是删除的项组成的数组，同时操作会直接对原数组造成影响！
    * 删除：指定 **2 个参数** -- 要删除的 **第一项的位置** 和要 **删除的项数**
    ```javascript
    var colors = ['red', 'yellow', 'green', 'black', 'brown'];
    var colors2 = colors.splice(1,2);

    colors; // ['red', 'black', 'brown']
    colors2; // ['yellow', 'green']
    ```
    * 插入：**至少 3 个参数**，起始位置、0（删除的项数）、要插入的项（可多项）
    ```javascript
    var colors = ['red', 'yellow', 'green', 'black', 'brown'];
    var colors2 = colors.splice(1, 0, 'white');

    colors; // ['red', 'white', 'yellow', 'green', 'black', 'brown']
    colors2;
    ```
    * 替换：**至少 3 个参数** -- 起始位置、要删除的项数、要插入的项数（可多项）
    ```javascript
    var colors = ['red', 'yellow', 'green', 'black', 'brown'];

    colors.splice(1, 2, 'white', 'pink');

    colors; // ["red", "white", "pink", "black", "brown"]
    ```

## 位置方法
* `indexOf()` `lastIndexOf()` -- 查找某项的索引
    `indexOf()` 是从前到后，所以返回最先出现的位置索引；`lastIndexOf()` 是从后到前，返回最后出现的位置索引；
    * 参数
        * **一个参数**：1. 要查找的项，2. 查找起点位置默认从最前（0）或最后
        * **两个参数**：1. 要查找的项，2. 查找起点位置的索引
    * 返回：返回要查找的项的位置索引，没找到时返回 **-1**
    * 注意：查找的项与数组项必须严格相等

    ```javascript
    var colors = ['red', 'green', 'blue', 'red'];

    alert(colors.indexOf('green')); // 1
    alert(colors.lastIndexOf('red')); // 3

    alert(colors.indexOf('red', 1)); // 3
    alert(colors.lastIndexOf('red', 2)); // 0
    ```
