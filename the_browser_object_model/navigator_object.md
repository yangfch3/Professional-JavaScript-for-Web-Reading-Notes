# navigator 对象

1. 每个支持 `JavaScript` 所共有；

2. 有用的几个 `navigator` 对象属性
    * `appName`
    * `appVersion`
    * `Platform`
    * `product`
    * `plugins`
    * `userAgent`

3. 检测插件
    1. 在 **非 IE 浏览器** 中使用 `plugins` 数组访问插件，数组的每一项是一个插件对象，对象包含 `name` `description` `filename` `length(处理MIME类型属性)` 等属性
        ```javascript
        function hasPlugin(name) {
            var pluginsList = navigator.plugins;
            name = name.toLowerCase(); // 统一小写，方便比较
            for (var i = 0; i < pluginsList.length; i++) {
                if (pluginsList[i].name.toLowerCase().indexOf(name) > -1) {
                return true;
                }
            }

            return false;
        }

        alert(hasPlugin('Flash')); // true

        alert(hsaPlugin('QuickTime')); // false
        ```

    2. 兼容 IE 浏览器的写法：在 IE 浏览器中需要使用专有的 `ActiveXObject` 类型。IE 是以 `COM` 对象的方式实现插件的，COM 对象使用唯一的标识符来标识
    ```javascript
    // 普通浏览器插件检测函数
    function hasPlugin(name) {
        var pluginsList = navigator.plugins;
        name = name.toLowerCase(); // 统一小写，方便比较
        for (var i = 0; i < pluginsList.length; i++) {
            if (pluginsList[i].name.toLowerCase().indexOf(name) > -1) {
            return true;
            }
        }

        return false;
    }

    // IE 浏览器插件检测函数
    function hasIEPlugin(name) {
        try {
            new ActiveXObject(name);
            return true;
        } catch (err) {
            return false;
        }
    }

    // 兼容性地检测所有浏览器的 Flash
    function hasFlash() {
        var result = hasPlugin('Flash'); // 一般浏览器检测
        if (!result) {
            result = hasIEPlugin('ShockwaveFlash.ShockwaveFlash');
        }
        return result;
    }

    // 兼容性地检测所有浏览器的 QuickTime
    function hasQuickTime() {
        var result = hasPlugin('QuickTime');
        if (!result) {
            result = hasIEPlugin('QuickTime.QuickTime');
        }
        return result;
    }

    alert(hasFlash);
    alert(hasQuickTime);
    ```

4. 注册处理程序（details in Chapter 22）
`navigator.registerContentHandler()` `navigator.registerProtocolHandler()`
两个方法用于指明该站点可以处理的特定类型的信息
    ```javascript
    navigator.registerContentHandler('application/rss+xml', 'http://www.somereader.com?feed=%s', 'Some Reader');

    navigator.registerProtocolHandler('mailto', 'http://www.somemailclient?cmd=%s', 'Some Mail Client');
    ```
