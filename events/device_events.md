# 设备事件
`orientationchange` `MozOrientation` `deviceorientation` `devicemotion`

## 1. orientationchange 事件
1. `orientationchange` 事件最早由 `Safari` 使用，所有 iOS 设备都支持 orientationchange 事件和 `window.orientation` 属性；

2. 更换横、纵向查看方式时在 `window` 上触发；

3. `window.orientation` 属性值
    * 0：肖像模式
    * 90：左旋横向模式
    * -90：右旋横向模式

## 2. MozOrientation 事件
1. 已废弃，早起版本 Firefox 为检测设备状态引入；

2. 只有带加速器的设备才支持次事件。

## 3. deviceorientation 事件
1. `deviceorientation` 事件是标准规范的，得到了广泛支持的；

2. 原理：设备加速器检测到设备方向在变化时在 `window` 对象上触发；

3. 触发 `deviceorientation` 事件时的 `event` 对象包含五个属性：**每个轴相对于设备静止状态下发生变化的信息**
    * `alpha`：围绕 Z 轴旋转时，Y 轴的度数差
    * `beta`：围绕 X 轴旋转时，Z 轴的度数差
    * `gamma`：在围绕 Y 轴旋转时，X 轴的度数差
    * `absolute`：布尔值，表示设备是否返回一个绝对值
    * `compassCalibration`：布尔值，设备的指南针是否校准过

4. 依据 `deviceorientation` 事件对象的各个属性值我们可以针对手机的运动做出相应地处理。

## 4. devicemotion 事件
1. `devicemotion` 事件能监测设备是否正在往下掉，或者是不是被走着的人拿在手里；

2. 触发 `devicemotion` 事件时，`event` 对象包含以下属性
    * `acceleration`：一个包含 X、Y、Z 属性的对象，告诉你每个方向上的加速度；
    * `accelerationIncludingGravity`：一个包含 x、y、z 属性的对象，在考虑 Z 轴自然重力加速度情况下，告诉你每个方向上的加速度
    * `interval`：以毫秒表示的时间值，必须在另一个 `devicemotion` 事件触发前传入
    * `rotationRate`：一个包含表示方向的 `alpha`、`beta` 和 `gamma` 属性的对象

3. 如果读取不到以上属性（1、2、4），则他们的值为 `null`
