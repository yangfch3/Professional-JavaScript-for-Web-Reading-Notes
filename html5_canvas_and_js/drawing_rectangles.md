# 绘制矩形
`fillRect()` `strokeRect()` `clearRect()`

---

1. 矩形是唯一一种可以直接在 2D 上下文中绘制的形状；
    > 圆形及其他形状只能通过路径等手段绘制。

2. `fillRect()` `strokeRect()` `clearRect()` 都接收 4 个参数
    * 矩形的 x 坐标
    * 矩形的 y 坐标
    * 矩形的宽度
    * 矩形的高度

3. 三个方法的不同点
    * `fillRect()` 方法在画布上 **绘制的矩形会填充指定的颜色（`fillStyle`）**
    * `strokeRect()` 方法在画布上 **绘制的矩形会使用指定的颜色（`strokeStyle`）描边**
    * `clearRect()` 方法用于 **清除（擦除）画布上的矩形区域**，可以通过绘制形状，再清除指定区域可以做出许多效果。

---
```javascript
    <div class="canvas-container">
        <canvas class="canvas-box" id="canvas-box" width="200" height="200">
            您的浏览器不支持 canvas，请升级您的浏览器或使用其他现代浏览器浏览本页面！
        </canvas>
    </div>
    <script>
        var drawing = document.getElementById('canvas-box');

        if(drawing.getContext) {
            var context = drawing.getContext('2d');

            // 属性设置
            context.fillStyle = '#ff0000';

            // 绘制填充矩形
            context.fillRect(10, 10, 50, 50);

            // 重写填充属性
            context.fillStyle = 'rgba(0, 0, 255, 0.5)';

            // 绘制另一个透明填充矩形
            context.fillRect(30, 30, 50, 50);

            // 重写描边属性
            context.strokeStyle = 'rgba(0, 255, 0, 0.8)';

            // 绘制描边矩形
            context.strokeRect(100, 100, 50, 50);

            // 绘制清除矩形
            context.clearRect(40, 40, 10, 10);

            var imgURI = drawing.toDataURL('image/png');
            var img = document.createElement('img');
            img.src = imgURI;

            document.body.appendChild(img);
        }
    </script>
```

![2016-04-24_151207.jpg-110kB][1]


  [1]: http://static.zybuluo.com/yangfch3/zveuzhdiku1h75hjova6z8v8/2016-04-24_151207.jpg
