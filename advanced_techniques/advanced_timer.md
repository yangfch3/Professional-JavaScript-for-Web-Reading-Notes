# 高级定时器
1. 定时器不是线程；

2. 定时器仅仅只是 **计划** 代码在未来的某个时间执行，但 **执行时机是不能保证的**（类似我们的日常安排、事件安排）；

3. 代码运行、事件处理程序、`Ajax` 回调函数都是使用同一个线程来执行；浏览器负责进行排序，**指派某段代码在某个时间点运行的优先级**。

## 理解时间线机制
![1.png-207.2kB][1]

1. 可以将 `JavaScript` 想象成在时间线上运行；

2. JS 代码运行过程简介
    * 页面载入，可执行代码的执行，简单的函数、变量声明，初始数据的处理
    * JS 执行进程（即时间线）空闲
    * 定时器、事件处理程序、回调函数加入事件线，执行；如有发生执行代码重叠，依据浏览器 **优先机制** 与 **队列排序机制** 在空闲时依次执行

3. 保证 JS 代码按序运行的两个必要机制
    * `JavaScript` 主进程
    * 代码队列

4. 代码会按照执行的顺序添加入队列，并在下一个可能的时间里执行；

5. `JavaScript` 里没有任何代码是立刻执行的，但一旦进入进程空闲则尽快执行；

6. 定时器对队列的工作方式
    >当特定时间过去后将代码插入到执行队列，如果在这个时间点上，队列中没有其他东西，那么这段代码就会被执行；如果这个时间点上已经有代码正在运行（进程非空闲），则会发生队列等待。

7. 定时器指定的时间间隔只是表示 **何时将定时器的代码添加到执行队列，而不是何时执行代码**；

8. 队列中的所有代码都要等到 `JavaScript` 进程空闲之后才能执行，而不管他们是如何添加到队列中的；

9. 队列机制实例
![2.png-334.7kB][2]

## 重复的定时器
1. 使用 `setInterval()` 间歇调用的优缺点
    * 优点
        * 确保了定时器代码规则地插入到队列中
    * 问题（原因见 2）
        * 某些间隔会被跳过
        * 多个定时器代码之间的间隔可能比预期的小（由于队列机制）

2. 定时器代码可能在代码再次被添加到队列之前还没有完成执行，结果导致定时器代码发生叠加 **的情况是不会发生的，因为 `JavaScript` 引擎已经给我们做好了优化**
    > 当使用 `setInterval()` 时，仅当没有该定时器的任何其他代码实例时，才将定时器代码添加到队列中。

3. 解决两个问题的方法：链式超时调用
    ```javascript
    setTimeout(function() {
        // ...处理中

        setTimeout(arguments.callee, interval);
    }, interval);
    ```

4. 链式超时调用代替间歇调用的好处
    * 在前一个定时器代码执行完之前，不会向队列插入新的定时器代码，确保不会有任何缺失的间隔
    * 可以保证在下一次定时器代码执行之前，至少要等待指定的间隔，避免了连续的执行

5. 每个浏览器窗口、标签页、或者框架都有各自的代码执行队列

## Yielding Processes
1. 浏览器的内存配额限制
    >运行在浏览器的 `JavaScript` 不同于桌面应用（能随意控制他们要的内存大小和处理器时间），一般内存分配与处理器时间的分配都有限制，这一措施是为了恶意程序导致系统崩溃。
    >
    >当代码运行超过特定时间、超过特定的语句数量、超过特定的内存限制时浏览器会有中断机制，以及询问用户确认是否继续运行的机制

2. 脚本长时间运行的问题通常有两个原因
    * 过长的、过深嵌套的函数调用
    * 进行大量处理的循环

3. 数组中项目数量直接影响到循环的时间长度

4. 使用 `Yielding Processes` 的三个条件
    * 一个循环占用了大量的时间
    * 循环内的处理不需要同步完成
        * 即循环中的处理可以推迟一些处理，而不会对循环之后的代码执行造成影响
    * 数据不需要按顺序完成

5. **数组分块**（array chunking），使用定时器分割这个循环，小块小块地处理数组
    ```javascript
    setTimeout(function() {
        // 取出下一个条目并处理
        var item = arr.shift();
        process(item);

        // 若还有条目，再设置另一个定时器
        if(arr.length > 0) {
            setTimeout(arguments.callee, 100);
        }
    }, 100);
    ```

6. `chunk` 数组分块封装函数
    ```javascript
    function chunk(arr, process, context) {
        setTimeout(function() {
            var item = arr.shift();
            process.call(context, item);

            if(arr.length > 0) {
                setTimeout(arguments.callee, 100);
            }
        })
    }
    ```
    > **注意点**：传递给 `chunk()` 的数组（`arr`）是用作一个队列的，因此随着数据的处理，数组中的条目也在变化，如果想保持原数组不变，则应该将数组的克隆（副本）传给 `chunk()`

## 函数节流
1. 连续进行过多的 DOM 操作（需要很多的内存和 CPU 时间）会导致浏览器挂起\_(:зゝ∠)_；

2. `onreize` 事件在部分浏览器下会随着页面的拉伸连续不断的出发，`onscroll`、`onmousemove` 也有同样的问题；

3. 可以使用定时器对函数的执行进行节流
    > 函数节流：使用定时器与清除定时器的机制，使得某些代码不能在没有间断的情况下连续重复执行
    >第一次调用函数，创建一个定时器，在指定的时间间隔之后运行代码；在第二次调用该函数时，他会清除前一次的定时器并重新设置一个

4. 节流函数封装
    ```javascript
    function throttle(method, context) {
        clearTimeout(method.tId);
        timer = setTimeout(function() {
            method.call(context);
        }, 100)
    }

    // 使用方法：以 resize 事件处理程序为例
    function resizeDiv() {
        var div = document.getElementById('myDiv');
        div.style.height = div.offsetWidth + 'px';
    }

    window.onresize = function() {
        throttle(resizeDiv);
    }
    ```

5. 常见于 `resize`、`scroll` 事件，确保浏览器不会在极短的时间内进行过多的运算。


  [1]: http://static.zybuluo.com/yangfch3/y37ljzt6j6bjbgtis9fu639p/1.png
  [2]: http://static.zybuluo.com/yangfch3/2owfr69adkidxll4ehj8z3ll/2.png
