# history 对象
1. `history` 对象是 `window` 对象的属性，**一般用于创建自定义的“后退”和“前进”按钮，以及检测当前页面是不是标签页历史记录中的第一个页面**；

2. 出于安全考虑，开发人员无法获取得知用户浏览过的 URL，使用 `history` 获取到的历史记录只是单个标签页的跳转记录（安全性考虑）；

3. `history.go()`
    * 作用：在用户的历史记录中进行任意跳转
    * 参数
        1. 无参数：重载当前页面
        2. 一个参数
            * 数值：跳转到数值对应的历史记录条目，正前负后
            * 字符串：跳转到历史记录中包含该字符串的第一个位置，可进可退，如历史记录列表没有符合该条件的条目，则刷新当前页面
    * 返回值：`undefined`

    ```javascript
    history.length; // 3

    history.go();
    history.go(-1);
    history.go(2);
    history.go('hope'); // chrome 似乎不支持
    ```

4. `history.back()` `history.forward()` 不接受参数，相当于浏览器的前进和后退；

5. 检测是否是当前标签页打开的第一个页面
    ```javascript
    if (history.length === 0) {
        // ...
    }
    ```

6. [`history` 对象在 `HTML5` 中的新属性、新方法和新事件][1]
    * 属性：`state`
    * 方法：`history.pushState()` `history.replaceState()`
    * 事件：`window.onpopstate`



  [1]: https://www.zybuluo.com/yangfch3/note/241234
