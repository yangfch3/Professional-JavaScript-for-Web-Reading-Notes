# 媒体元素
`<audio>` `<video>`

1. `HTML 5` 新增了两个元素，并提供了用于实现常用功能的 `JavaScript API`；

2. 位于开始标签和结束标签之间的内容会作为后备内容，在浏览器不支持这两个媒体元素的情况下显示；

3. 使用 `source` 元素可以指定多个不同的媒体元素，用于跨浏览器兼容；

4. 不同的浏览器支持不同的编解码器。

## 属性
二元素都提供了完善的 `JavaScript` 接口，下面是二元素的共有属性：

* `autoplay`：boolean
* `buffered`：时间范围
* `bufferedBytes`：字节范围
* `bufferingRate`：int
* `bufferingThrottled`：boolean
* `controls`：boolean
* `currentLoop`：int
* `currentSrc`：str
* `currentTime`：float
* `defaultPlaybackRate`：float
* `duration`：float
* `ended`：boolean
* `loop`：boolean
* `muted`（静音）：boolean
* `networkState`：int
* `paused`：boolean
* `playbackRate`：int
* `played`：事件范围
* `readyState`：int
* `seekable`：时间范围
* `src`：str
* `start`：float
* `totalBytes`：int
* `videoHeight`：int
* `videoWidth`：int
* `volume`：float

上面的部分属性可以直接在 `audio` 和 `video` 元素的特性上进行设置。

## 事件
二元素还可以触发很多事件，这些事件监控着不同属性的变化，这些变化可能来自 **媒体播放**，也可能来自 **用户操作播放器**

* `abort`：下载中断时触发
* `canplay`：可以播放时触发，`readyState` 值为 2
* `canplaythrough`：播放可继续，`readyState` 值为 3
* `canshowcurrentframe`：当前帧已下载完，`readyState` 值为 1
* `dataunavailable`：没有数据、无法播放，`readyState` 值为 0
* `durationchange`：`duration` 属性改变
* `emptied`：网络连接关闭
* `empty`：发生错误无法下载
* `ended`：已播放到末尾
* `error`：下载期间发生网络错误
* `load`：已经不被推荐
* `loadeddata`：第一帧已加载完成
* `loadedmetadata`：媒体的元数据已加载完
* `loadstart`：下载已开始
* `pause`：播放暂停
* `play`：播放
* `playing`：媒体正在播放
* `progress`：正在下载
* `ratechange`：播放媒体的速度改变
* `seeked`：搜索结束
* `seeking`：正移动到新位置
* `stalled`：已建立链接，但未收到数据
* `timeupdate`：`currentTime` 被以不合理或意外的方式更新
* `volumechange`：`volume` 或 `muted` 属性值已改变
* `waiting`：播放暂停

## 自定义媒体播放器
利用二元素的各种属性和事件可以很方便地创建自定义的媒体播放器

## 检测编解码器的支持情况
1. 为了很好的兼容各个浏览器，我们需要提供多个媒体来源；

2. `JavaScript` 为两个媒体元素新增的 API：`canplayType()` 方法
    * 参数：格式/编解码器字符串
    * 返回值
        * `probably`
        * `maybe`
        * `''`：假值

```javascript
var audio = document.getElementById('audio-player');

// maybe
if (audio.canPlayType('audio/mpeg')) {
    // 进一步处理
}

// probably
if (audio.canPlayType('audio/ogg; codecs="vorbis"')) {
    // 进一步处理
}
```

附 1：已得到推广的音频格式及其编解码器

|音频|字符串|支持的浏览器|
|---|---|---|
|AAC|audio/mp4; codecs="mp4a.40.2"|IE 9+、Safari|
|MP3|audio/mpeg|IE 9+、Chrome|
|Vorbis|audio/ogg; codecs="vorbis"|FF、Chrome、Opera|
|WAV|audio/wav; codecs="1"|FF、Opera、Chrome|

附 2：已得到推广的视频格式和编解码器

|视频|字符串|支持的浏览器|
|---|---|---|
|H.264|video/mp4; codecs="avc1.42E01E, mp4a.40.2"|IE 9+、Safari、Webkits|
|Theroa|video/ogg; codecs="theroa"|FF、Opera、Chrome|
|WebM|video/webm; codecs="vp8, vorbis"|FF、Opera、Chrome|

## Audio 类型对象
`<audio>` 元素还有一个原生的 `JavaScript` 构造函数 `Audio`，与 `Image` 类型相似。

想播放音乐的话只需要在代码里新建一个 `audio` 对象实例，并传入音频源文件即可

```javascript
var audio = new Audio('sound.mp3');

eventUtil.addEventHandler(audio, 'canplaythrough', function(event) {
    audio.play();
})
```
