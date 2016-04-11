# 工厂模式
1. `ECMAScript` 中无法创建类；

2. 用函数来封装以特定接口创建对象的细节；

3. 代码
    ```javascript
    function createPerson(name, age, job) {
        var o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function() {
            alert(this.name);
        };
        return o;
    }

    // 使用
    var yangfch3 = createPerson('yangfch3', 21, 'UIDev');
    ```

4. 工厂模式优点
    * 解决了创建多个相似对象时重复书写代码的问题

5. 工厂模式缺点
    * 没有解决对象识别的问题（如何知道一个对象的 **类型**）

> 注意，是类型，不是类，ES 中（暂时）没有 **类** 的概念，只有 **类型** 一说
