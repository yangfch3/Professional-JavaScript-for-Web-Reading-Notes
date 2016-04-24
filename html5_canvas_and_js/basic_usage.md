# 基本用法
1. `<canvas>` 标签的 `width` 和 `height` 特性指定绘图区域大小；
    > 也可以通过 CSS 样式指定宽高。

2. `<canvas>` 内部为后备信息（类似 `noscript`），在浏览器不支持时显示；

3. 要在画布（`<canvas>`）上绘图，需要先取得上下文，使用 `canvas` DOM 对象的 `getContext()` 方法，一个参数：上下文的名字，如 `'2d'`；

4. 在绘制之前，首先检测目标 `canvas` 元素对象的 `getContext()` 对象是否存在；
    > 有些浏览器会为 HTML 规范之外的元素创建默认的 HTML 元素对象，在一些旧浏览器下，`canvas` 被当做普通自定义元素识别，但是不具备绘图的能力。

5. 使用 `canvas` 元素的 `toDataURL()` 可以导出 `<canvas>` 元素上绘制的图像，接收一个参数：图像的 `MIME` 格式。
    > `toDataURL()` 方法的返回值是一个 `data URI`。

---
![2016-04-24_135858.jpg-95.7kB][1]
![2016-04-24_135919.jpg-24.9kB][2]


  [1]: http://static.zybuluo.com/yangfch3/3jjq58c3fw0ak9ivb7t5lnz6/2016-04-24_135858.jpg
  [2]: http://static.zybuluo.com/yangfch3/0e35ktzogpmji398r2tm7dkn/2016-04-24_135919.jpg
