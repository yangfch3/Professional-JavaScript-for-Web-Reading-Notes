# 自定义事件
1. 事件监听处理机制的本质是：**观察者设计模式**；

2. 观察者模式由两类对象组成：**主体** 和 **观察者**；
    > 主体负责发布事件，同时观察者通过订阅这些事件来观察主体；主体对观察者一无所知，观察者知道主体并能注册事件的回调函数

3. DOM 事件处理程序的机制：**DOM 元素是主体，你的事件处理代码便是观察者**；

4. 非 DOM 代码中，通过实现自定义事件（非 DOM 事件），创建一个管理事件的对象，让其他对象监听那些事件。

5. 示例代码
    ```javascript
    function EventTarget() {
        this.handlers = {};
    }

    EventTarget.prototype = {
        constructor: EventTarget,
        addHandler: function(type, handler) {
            if(typeof this.handlers[type] == undefined) {
                this.handlers[type] = []; // 如果 ET 的实例上还没有该事件类型，便建立该事件类型
            }
            this.handlers[type].push(handler); // 如果已有该事件类型，便将事件处理程序推入该事件的处理程序列表中
        },
        fire: function(event) {
            if(!event.target) {
                event.target = this;
            }
            if(this.handlers[event.type] instanceof Array) { // 检查事件类型是否已注册
                var handlers = this.handlers[event.type]; // handlers 为所有事件处理程序的数组
                for(var i = 0, len = handlers.length; i < len; i++) {
                    handlers[i](event); // 触发数组中的每一个事件处理程序
                }
            }
        },
        removeHandler: function(type, handler) {
            if(this.handlers[type] instanceof Array) { // 检查事件类型是否已注册
                var handlers = this.handlers[type];
                for(var i = 0, len = handlers.length; i < len; i++; { // 遍历寻找事件处理程序员
                    if(handlers[i] === handler) {
                        break;
                    }
                    handlers.splice(i, 1); // 删除传入的事件处理程序的对应项
                } )
            }
        }
    };
    ```

6. 使用方法
    ```javascript
    function handlerMessage(event) {
        alert('Message received: ' + event.message);
    }

    // 创建一个新对象
    var traget = new EventTarget();

    // 添加一个事件处理程序
    target.addHandler('message', handleMessage);

    // 触发事件，以事件对象的形式传入事件的相关信息
    target.fire({type: 'message', message: 'Hi!'});

    // 删除事件处理程序
    target.removeHandler('message', handleMessage);

    // 删除后无法再触发
    target.fire({type: 'message', message: 'Hi!'});
    ```

7. 整个过程中的原型链
    ![IMG_4324.JPG-1161.5kB][1]

8. 触发一个事件时，调用 `fire()` 方法，接受一个单独的参数：事件对象，至少需要包含 `type` 属性；

9. 使用寄生组合继承使得其他对象可以继承 `EventTarget`
    ```javascript
    function Person(name, age) {
        EventTarget.call(this);
        this.name = name;
        this.age = age;
    }

    function inheritPrototype(subType, superType) {
        var prototype = Object(superType.prototype);
        prototype.constructor = subType;
        subType.prototype = prototype;
    }

    inheritPrototype(Person, EventTarget);

    Person.prototype.say = function(message) {
        this.fire({type: 'message', message: message});
    }

    // 调用方法
    function handleMessage(event) {
        alert(event.target.name + "say: " + event.message);
    }

    var person = new Person('yangfch3', 21);

    person.addHandler('message', handleMessage);

    person.say('Hi!')
    ```

10. 自定义事件（观察者模式）在代码中 **存在多个部分在特定时刻相互交互** 的情况下十分有用；

11. 使用自定义事件有助于对象之间的解耦，保持功能的隔离，在多数情况下，触发事件的代码和监听事件的代码是完全分离的。


  [1]: http://static.zybuluo.com/yangfch3/nuyi6eis2y8exr6fw9axsbgj/IMG_4324.JPG
