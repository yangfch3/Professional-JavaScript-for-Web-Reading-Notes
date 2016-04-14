# 动态原型模式
1. 所有信息都封装在构造函数内，**在构造函数中初始化原型**（判断、仅在必要的情况下）；

2. **动态**：
    > 通过检查某个应该存在的方法是否有效，来决定是否需要初始化原型

3. 代码实例
    ```javascript
    function Person(name, age, job) {
        // 属性
        this.name = name;
        this.age = age;
        this.job = job;

        // 方法
        if(typeof this.sayName != 'function') {
            Person.prototype.sayName = function() {
                alert(this.name);
            }
        }
    }
    ```
