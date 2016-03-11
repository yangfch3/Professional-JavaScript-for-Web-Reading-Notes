#  内存和性能
添加到页面上的事件处理程序的数量会影响页面的运行性能。每个事件处理函数都是一个对象，内存中的对象越多，性能就越差。

优化过多的事件处理程序带来的性能问题的方法是：**事件委托**、**移除事件处理程序**。

## 事件委托
1. 事件委托利用的是事件的冒泡特性；

2. 在 DOM 树上尽可能高的层次（委托者）添加事件处理程序，从而管理下级元素的某一类型的所有事件；
    > 例如：`click` 事件会一直冒泡到 `document` 层次，也就是说，我们可以为整个页面指定一个 `click` 事件处理程序，而不必为每个可单击的元素添加事件处理程序。

3. 示例
    ```javascript
    // HTML
    <ul id="myLinks">
        <li id="link1">I am link 1!</li>
        <li id="link2">I am link 2!</li>
        <li id="link3">I am link 3!</li>
    </ul>

    // JavaScript
    var list = document.getElementById('myLinks');

    eventUtil.addEventHandler(list, 'click', function() {
        event = eventUtil.getEvent(event);
        var target = eventUtil.getTarget(event);

        switch (target.id) {
            case 'link1':
                alert(target.innerHTML);
                break;
            case 'link2':
                alert(target.innerHTML);
                break;
            case 'link3':
                alert(target.innerHTML);
                break;
        }
    });
    ```

4. 采用事件委托的优点
    * 添加的事件处理程序少；
    * 需要占用的内存少；
    * 多数鼠标事件和键盘事件都可以采用时间委托；
    * 易于移除事件处理程序。

5. 最适合使用事件委托技术的事件包括
    * click
    * mousedown
    * mouseup
    * keydown
    * keyup
    * keypress

6. 虽然 `mouseover` 和 `mouseout` 事件也冒泡，但是适当处理他们并不容易，而且需要经常计算元素的位置，所以一般不采取事件委托。

7. [事件委托demo.html][1]

## 移除事件处理程序
1. 内存中留有一些过时不用的“空事件处理函数”（`dangling event handler`），会造成页面性能与内存问题；

2. 从文档移除带有事件处理程序的元素时（例如使用 `removeChild()` 和 `replaceChild()`，使用 `innerHTML` 重写 DOM 树），原来添加到元素上的时间处理程序极有可能无法被当做垃圾回收（仍旧保存在内存中）；

3. 解决方法：在删除或重写元素之前先手工移除元素上的事件处理程序
    ```javascript
    var btn = document.getElementById('myBtn');

    btn.onclick = function(event) {
        // ...event process code

        btn.onclick = null; // 移除重写之前先移除事件处理程序

        document.getElemnetById('myDiv').innerHTML = '...';
    }
    ```

4. 如果需要提升用户体验，可以在页面卸载（`unload`）时移除所有事件处理程序（当然不是必须的）；

5. 事件委托把事件集中，使我们需要追踪的事件很少，移除也就方便了很多。

  [1]: http://static.zybuluo.com/yangfch3/7v1zoq23rwqeyzcqu9fb1tqx/%E4%BA%8B%E4%BB%B6%E5%A7%94%E6%89%98demo.html
