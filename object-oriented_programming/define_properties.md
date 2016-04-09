# 定义多个属性
1. 一次定义多个属性使用 ES 5 提供的 `Object.defineProperties()` 方法；

2. 两个参数
    * 目标对象（要添加和修改属性的对象）
    * 多个描述符对象组成的属性列表对象

3. 示例
    ```javascript
    var book = {};

    Object.defineProperties(book, {
        _year: {
            value: 2004
        },

        edition: {
            value: 1
        },

        year: {
            get: function() {
                return this._year;
            },
            set: function(newValue) {
                if(newValue > 2004) {
                    this._year = newValue;
                    this.edition += newValue - 2004;
                }
            }
        }
    })
    ```

4. IE 9+ 支持 `Object.defineProperties()` 方法。
