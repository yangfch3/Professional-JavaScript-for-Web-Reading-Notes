# module pattern and singleton

## 模块模式
1. 这里的模块模式指的是 **为单例创建私有变量和特权方法**

2. 单例（singleton），指的是只有一个实例的对象

3. 使用模块模式为单例增强（为单例添加私有变量和特权方法）
    ```javascript
    var application = function() {
        // 私有变量
        // 创建一个模块容器
        var components = new Array();

        // 私有函数
        function deleteComponent(index) {
            var component = Components[index];
            delete Components[index];
            return component;
        }

        // 初始化
        // 模块列表初始化
        components.push(new BaseComponent());

        // 公共
        // 模块对外暴露接口，用于模块管理
        return {
            // 访问器接口
            getComponenetCount: function() {
                return components.length;
            },

            getCompinent: function(index) {
                return components[index];
            },

            // 注册一个新模块
            registerComponent: function(component) {
                if (typeof component == "object") {
                    components.push(component)
                }
            }

            // 调用私有函数，删除一个模块
            deleteComponent: function(index) {
                // 调用私有函数
                deleteComponent(idnex);
            }
        }
    }();
    ```

4. 在上面的示例代码中，我们可以看到
    1. 从本质上来讲，return 的那个对象存储着这个单例的所有接口
    2. 利用模块模式增强单例可以用于方便的管理模块（所以也常常使用单例来管理应用程序级的消息）

## 增强的模块模式
1. 增强的模块模式：即在返回对象之前加入对其增强的代码；

2. 增强的模块模式基本形式
    ```javascript
    var singleton = function(){
        // 私有变量和函数
        var privateVariable = 10;

        function privateFunction() {
            return false;
        }

        // 创建将返回的对象实例，使用构造函数可以初始化
        var object = new CustomType();

        // 添加特权/公有属性和方法
        object.publicProperty = true;

        object.publicMethod = function() {
            privateVariable++;
            return privateFunction();
        }
    }();
    ```

3. 在 Web 应用程序中，使用一个单例来管理应用程序级的信息，增强的模块模式的写法
    ```javascript
    var application = function() {
        // 私有变量和函数
        var components = new Array();

        // 初始化，BaseComponent 为特定的构造函数，用于初始化组件数组
        conponents.push(new BaseComponent());

        // 创建 app 的一个局部副本
        // app 可以从 CustomType那里得到很多特性与方法
        var app = new CustomType();

        // 添加公共接口
        app.getComponentCount = function() {
            return components.length;
        };

        app.registerComponent = function(component) {
            if(typeof component == 'object') {
                components.push(component);
            }
        };

        // 返回这个副本
        return app;
    }();
    ```
