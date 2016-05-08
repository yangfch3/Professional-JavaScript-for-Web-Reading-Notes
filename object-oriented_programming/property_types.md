# 属性类型
1. 对象属性内部的特性（attribute）描述了对象属性（Property）的各种特征；

2. **特性是对象属性的属性**，规范把它们放在 **两对方括号** 中；

3. 对象属性的特性一般无法直接通过 `JavaScript` 直接访问；

4. `ES` 中有两种属性：**数据属性** 和 **访问器属性**。

## 1. 数据属性
1. 数据属性的四个描述行为的特性
    * `[[Configurable]]`：默认为 `true`，表能否 **删除**、能否 **修改属性的特性**
    * `[[Enumerable]]`：默认为 `true`，表能否通过 `for-in` 循环遍历该属性
    * `[[Writable]]`：默认为 `true`，表示能否 **修改属性的值**
    * `[[value]]`：默认为 `undefined`

2. `Object.defineProperty()` 方法可以定义与修改属性的特性，接收三个参数：**属性所在的对象**、**属性的名字** 和一个 **描述符对象**；

3. 示例代码
    ```javascript
    var person = {};

    Object.defineProperty(person, 'name', {
        writable: false,
        value: 'yangfch3'
    });

    alert(person.name);
    person.name = 'Greg';
    alert(person.name);
    ```

4. 把属性的 `configurable` 设置为 false，那么 **不能从对象中删除该属性**，且无法重新配置 `configurable` 的值（不可逆性）；

5. 定位了 `configurable` 为 false 的属性唯一能进行的操作：**使用 `Object.defineProperty()` 方法修改 `writable` 的值**；

6. **注意点**：在调用 `Object.defineProperty()` 方法时，如果没有明确指定，`configurable` `writable` `enumerable` 的值默认为 `false`；

7. IE 8 及以下不要使用 `Object.defineProperty()` 方法。

## 2. 访问器属性
1. 访问器属性不包含数据值，它们包含一对 `getter` 和 `setter` 函数（二者皆为可选）；

2. 访问器属性包含 4 个特性
    * `[[Configurable]]`
    * `[[Enumerable]]`
    * `[[Get]]`：读取属性时调用，默认值为 `undefined`
    * `[[Set]]`：写入属性时调用，默认值为 `undefined`

3. 读取访问器属性时，调用 `getter` 函数，返回值即为访问器属性的 **表现值**；写入访问器属性时，调用 `setter` 函数，新属性值作为参数传入 `setter` 函数；

4. 示例
    ```javascript
    var book = {
        _year: 2004,
        edition: 1
    };

    Object.defineProperty(book, 'year', {
        writable: true,
        get: function() {
            return this._year;
        },
        set: function(newValue) {
            if(newValue > 2004) {
                this._year = newValue;
                this.edition += newValue - 2004;
            }
        }
    });

    book.year; // 读取，调用 `get` 特性函数
    book.year = 2005; // 写入，会调用 `set` 特性函数

    alert(book.edition);
    ```

5. 类似 `_year` 这样的，`_` 是一种约定俗成的标记，用于表示 **只能通过对象方法访问的属性**；

6. 使用访问器属性的常见方式：**设置一个属性的值会导致其他属性发生变化**
    > 利用这个连带变化的特性，常用于 **数据绑定**（双向）

7. 只指定 `getter` 意味着属性是 **不可写** 的；

8. 没有指定 `getter` 函数的属性 **不可读**；

9. 在 `Object.defineProperty()` 方法之前，使用以下两个非标准方法创建访问器属性
    * `obj.__defineGetter__()`
    * `obj.__defineSetter__()`

10. 使用旧方法创建访问器属性实例
    ```javascript
    var book = {
        _year: 2004,
        edition: 1
    };

    book.__defineGetter__('year', function() {
        return this._year;
    });

    book.__defineSetter__('year', function(newValue) {
        if(newValue > 2004) {
            this._year = newValue;
            this.edition += newValue -2004;
        }
    });

    book.year;
    book.year = 2005;
    alert(book.edition);
    ```

11. 在不支持 `Object.defineProperty()` 方法的浏览器中不能修改 `[[Configurable]]` `Enumerable`。


![configurable and writable](http://img.mukewang.com/5719f3ca0001520312800720.jpg)
