# 离线检测
## navigator.online
1. HTML5 规范要求了 `navigator.online` API 用于浏览器检测设备能否访问网络。

2. `navigator.online` API 在不同浏览器下的表现可能存在差异，所以 `navigator.online` 检测到的结果可能并不准确

```javascript
if (navigator.online) {
    // ...
} else {
    // 离线逻辑
}
```

## online 与 offline 事件
1. window 上的两个 HTML5 事件：`online` 与 `offline`

2. 当网络情况发生变化时，会触发相应的时间
