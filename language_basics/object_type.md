# Object 类型
1. 对象是一组数据和功能的集合

2. `Object()` 作为构造函数可用于生成构造对象实例，可以接受一个对象作为参数；

2. **最佳实践**：
    ```javascript
    var a = {foo: "bar"};
    ~~ var b = new Object(); // b = {} ~~ // 不推荐这样写
    var b = new Object(); // b = {}
    var c = new Object(a); // c = {foo: "bar"}
    ```

3. `Object` 类型所具有的任何属性和方法也同样存在于更具体的对象实例中，比较重要的继承方法有：
    * `constructor`
    * `hsaOwnProperty("propertyName")`
    * `isPrototypeof(obj)`
    * `propertyIsEnumerable("propertyName")`
    * `toString()`
    * `valueOf()`

4. 由宿主浏览器环境决定的 `DOM` 和 `BOM` 对象，可能会也可能不会继承 `ES` 标准的 `Object` 类型的属性和方法（一般会继承）；
