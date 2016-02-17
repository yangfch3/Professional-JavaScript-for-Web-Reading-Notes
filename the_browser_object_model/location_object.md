# location 对象
1. `location` 对象既是 window 的属性，也是 document 的属性，`window.location` 和 `document.location` 引用的是同一对象；

2. `location.search` 返回整个查询字符串，格式化查询字符串的封装函数的写法（以对象的键值对形式存储）
    ```javascript
    function getQueryStringArgs() {
        var qs = (location.search.length > 0 ? location.search.substring(1) : ''),
            args = {},
            items = qs.length ? qs.split('&') : [];
            item = null,
            name = null,
            value = null,
            i = 0,
            len = items.length;

            for (i=0; i<len; i++) {
                item = items[i].split('=');
                name = decodeURIComponent(item[0]);
                value = decodeURIComponent(item[1]);

                if (name.length) {
                    args[name] = value;
                }
            }

        return args;
    }
    ```

3. 位置操作
    1. `location.assign()` 用于浏览器地址跳转并在记录中生成一条记录；

    2. 更改 `location` 对象的各个属性也会暗中执行 `assign()` 操作，并生成新的历史记录；

    3. 大部分浏览器中更改 `hash` 也会在浏览器中生成新的历史记录；

    4. `location.reload()` 用于重载页面，传入参数 `true` 则进行 `no cache` 重载；

    5. `location.replace()` 接收一个 URL 参数，导航至该 URL，不会生成历史记录，常用于页面的重定向。
