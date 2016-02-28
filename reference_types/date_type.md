# Date 类型
1. `ES` 中的 `Date` 类型是在早起 `Java` 中的 `java.util.Date` 类的基础上构建的；

2. `Date` 类型使用自 `UTC`（Coordinated Universal Time, 国际协调时间）1970 年 1 月 1 日午夜（零时）开始经过的毫秒数来保存日期；

3. `Date.parse()` 方法：**解析接近自然语言的时间格式，返回毫秒时间戳格式或 NaN**

    几种接近自然语言的时间格式：

    * “月/日/年”，如 `'02/10/2016'`
    * 英文月名(或缩写) 日,年，如 `'February 10,2016'` `'Feb 10,2016'`
    * 英文星期几(缩写)英文月名(缩写)日年时:分:秒时区，如 `'Wed Feb 10 2016 18:46:11 GMT+0800'`
    * ISO 8601 扩展格式：YYYY-MM-DDTHH:mm:ss:sssZ，例如：`'2016-02-10T18:47:00'`
    <br>
    ```javascript
    var date0 = Date.parse('!@$%^^&'); // Invalid Date
    date0; // NaN

    var date1 = Date.parse('02/10/2016');
    date1; // 1455033600000

    var date2 = Date.parse('Feb 10, 2016');
    date2; // 1455033600000

    var date3 = Date.parse('Wed Feb 10 2016 18:47:00 GMT+0800');
    date3; // 1455130020000

    var date4 =  Date.parse('2016-02-10T18:47:00');
    date4; // 1455130020000
    ```

4. `Date.UTC()` 方法：**接收特定格式的年月日等参数，返回毫秒时间戳**
    * 参数（从前到后）
        * 年份（必选）
        * 月份（必选） -- 基于 0 （一月是 0 ）的月份
        * 日（可选） -- 1-31，默认是 1
        * 小时（可选） -- 0-23，默认是 0
        * 分钟（可选）
        * 秒（可选）
        * 毫秒（可选）

    <br>
    ```javascript
    var date0 = Date.UTC(2016, 2);
    date0; // 1456790400000

    var date1 = Date.UTC(2016, 1, 10, 18, 21);
    date1; // 1455128460000
    ```

5. 创建日期对象
    （可选）`new` 操作符、 `Date` 构造函数
    * 参数
        * 无参数：日起对象获得创建时的当前日期和时间；
        * 一个参数：对应某时间点的毫秒数（传入其他时间格式会隐式调用 `Date.parse()` 转换为毫秒数）<br>
        * 两个或多个参数：传入的参数被被 `Date.UTC()` 解析转换为毫秒时间戳，再创建时间对象。
    <br>

    ```javascript
    var now = new Date();
    now; // Wed Feb 10 2016 19:55:53 GMT+0800 (中国标准时间)

    var date0 = new Date(1455105353000);
    date0; // Wed Feb 10 2016 19:55:53 GMT+0800 (中国标准时间)

    // 隐式调用 Date.parse()
    var date1 = new Date('Feb 10, 2016');
    date1; // Wed Feb 10 2016 00:00:00 GMT+0800 (中国标准时间)

    var date2 = new Date(Date.parse('Feb 10, 2016'));
    date1; // Wed Feb 10 2016 00:00:00 GMT+0800 (中国标准时间)

    // 隐式调用 Date.UTC()
    var date3 = new Date(2016, 1, 10, 19, 38);
    date3; // Wed Feb 10 2016 19:38:00 GMT+0800 (中国标准时间)
    ```

6. `Date.now()`：返回表示调用这个方法的日期和时间的时间戳毫秒数
    ```javascript
    // 取得开始时间
    var start = Date.now();

    // 代码运行
    doSomeThing();

    // 取得停止时间
    var stop = Date.now();

    // 计算代码运行时间
    result = stop - start;
    ```

7. `+new Date()`：使用 `+` 将 `Date` 对象转换为字符串，作用与 `Date.now()` 一样
    ```javascript
    var date0 = +new Date();
    date0; // 1455108289508
    ```

## 继承的方法
1. `Date` 类型的 `toLocaleString` 方法会按照与浏览器设置的地区相适应的格式返回时间和日期；
    ```javascript
    var date0 = new Date();
    date0.toLocaleString(); // "2016/2/10 下午9:03:03"
    ```

2. `toString()` 方法则返回带有时区信息的日期和时间
    ```javascript
    var date0 = new Date();
    date0.toString(); // "Wed Feb 10 2016 21:03:45 GMT+0800 (中国标准时间)"
    ```

> 各个浏览器对于 `Date` 类型的 `toLocaleString()` 和 `toString()` 方法存在比较大的差异；
>
>**这两个方法一般仅在调试代码时比较有用**

## 日期格式化方法
各个浏览器对于日期格式化方法的输出采用不同的形式，难以统一，所以这些方法一般也只用于调试代码时使用：

* `toDateString()`
* `toTimeString()`
* `toLocaleDateString()`
* `toLocaleTimeString()`
* `toUTCString()`
* `toGMTString()`

## Date 类型的方法
|方法|说明|
|:--:|:--|
|`getTime()`|返回时间点的换算毫秒数，与 `valueOf()` 方法返回的值相同|
|`setTime(毫秒)`|以毫秒数设置日期，会改变整个日期|
|`getFullYear()`|取得四位数的年份|
|`getUTCFullYear`|不常用，返回 UTC 日期的四位数年份|
|`setFullYear(年)`|设置日期的年份，四位数|
|...|...|

基本格式：

* `get**()`;
* `getUTC**()`;
* `set**()`;
* `setUTC**()`;

上面的 `**` 为 `FullYear` `Month` `Date` `Day` `Hours` `Minutes` `Seconds` `Milliseconds`


```javascript
var date0 = new Date();
// Wed Feb 10 2016 21:42:26 GMT+0800 (中国标准时间)

date0.getTime(); // 1455111658254
date0.setTime(1455111600000); // Wed Feb 10 2016 21:40:00 GMT+0800 (中国标准时间)

date0.getFullYear(); // 2016
date0.setFullYear(1995); // 792423600000
date0; // Fri Feb 10 1995 21:40:00 GMT+0800 (中国标准时间)
```
