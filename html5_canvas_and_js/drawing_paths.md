# 绘制路径
`context.beginPath()` `context.closePath()`

`context.arc()` `context.arcTo()` `context.bezierCurveTo()` `context.lineTo()` `context.moveTo()` `context.quadraticCurveTo()` `context.rect()`

`context.fill()` `context.stroke()` `context.clip()`

`context.isPointInPath()`

---
1. 通过路径可以创造出复杂的形状和线条；

2. 几种路径
    * `arc(x, y, radius, startAngle, endAngle, counterclockwise)`
        以 `(x, y)` 为圆心，`radius` 为半径，后三个参数表意：**起始弧度，结束弧度，是否逆时针**
    * `arcTo(x1, y1, x2, y2, radius)`
        从 **上一点** 开始绘制一条弧线，到 `(x2, y2)` 为止，并且以给定的半径 `radius` 穿过 `(x1, y1)`
    * `bezierCurveTo(c1x, c1y, c2x, c2y, x, y)`
        从 **上一点** 开始绘制一条曲线，到 `(x, y)` 为止，并且以 `(c1x, c1y)` `(c2x, c2y)` 为控制点
    * `lineTo(x, y)`
        从 **上一点** 开始绘制一条直线，到 `(x, y)` 为止
    * `moveTo(x, y)`
        将绘图游标移动到 `(x, y)`，不画线
    * `quadraticCurveTo(cx, cy, x, y)`
        从 **上一点** 开始绘制一条二次曲线，到 `(x, y)` 为止，并且以 `(cx, cy)` 为控制点
    * `rect(x, y, width, height)`
        从 `(x, y)` 开始绘制矩形路径

3. 闭合路径：`context.closePath()`
    自动闭合路径，头尾相接

4. 填充路径围成的区域：`context.fill()`
    描边路径：`context.stroke()`

5. 在路径上创建一个剪切区域：`context.clip()`；

6. 检测某个点是否位于路径上：`context.isPointInPath(x, y)`
    ```javascript
    if(context.isPointInPath(100, 100)) {
        alert('Point (100, 100) is in the path.');
    }
    ```
