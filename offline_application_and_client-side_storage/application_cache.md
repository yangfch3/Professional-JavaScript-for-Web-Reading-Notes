# 应用缓存

## mainfest 文件
`manifest` 是一个描述文件，当我们需要开发一个严谨的 Web App 时，`manifest` 是必不可少的配置文件。

`manifest` 文件的文件类型（MIME）为：`text/cache-manifest`，应用缓存的 `manifest` 文件（除此之外，还有其他用途的 `manifest` 文件）一般会在 html 标签处引入 `manifest` 文件：
```
<html manifest="/offline.appcache">
<!-- 推荐使用最新推荐的 appcache 后缀文件 -->
<html manifest="/offline.manifest">
```

应用缓存 `manifest` 文件内的一段应用缓存配置：
```
CACHE MANIFEST
file.js
file.css
404.html

NETWORK:
*               # 表明除 CACHE 外的其他资源都需要在网络下才能使用

FALLBACK:
/html5/ /404.html   # 页面代替回退（404 页面代替需要网络的页面）
```

> 除了用于配置网络缓存的 `manifest`，还有用于配置 Web App 其他信息的 `manifest` 文件。
例如配置 Web App 桌面图标、应用标题等信息的 APP：
>
    <link rel="manifest" href="/manifest.webmanifest">
>
> https://developer.mozilla.org/en-US/docs/Web/Manifest

## window.applicationCache
HTML5 在 window 上增加了 `applicationCache` 对象，同时在这个对象上部署了状态、方法、事件。

![image_1b6ti5u0614h2kodpe93t51sn69.png-131.8kB][1]
(上图忘了标注还有一个 `abort()` 方法)

为 `application` 对象绑定事件可以使用以下两种模式：
```javascript
window.applicationCache.onupdateready = function() {
    window.applicationCache.swapCache();
}

// or
applicationCache.addEventListener('updateready', function(e) {
    applicationCache.swapCache();
}, fasle);
```

```
                         |--- cached
update() --- checking ---|
                         |--- updateready --- swapCache()
```

  [1]: http://static.zybuluo.com/yangfch3/ehn9tnbkxwm99vujdqap5h68/image_1b6ti5u0614h2kodpe93t51sn69.png
