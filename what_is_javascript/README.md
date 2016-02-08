# What Is JavaScript?
JavaScript 诞生于 1995 年，出现的目的是代替服务端脚本进行输入验证，现在他已经成为了一门拥有着许多特性的高级编程语言。

## JavaScript 简史
1. 浏览器与服务器的交互机制
> 一个链接的点击产生一次 `http` 请求

2. `JavaScript` 写法：`J` 和 `S` 大写；
3. `JavaScript` 语言遵循 `ECMAScript` 的标准，读音：`ek-ma-script`
4. `JavaScript` 由三个部分组成
    * 核心（`ECMAScript`）
        > `JavaScript` 含义丰富于 `ECMAScript`，`ECMAScript` 是 `JavaScript` 语言核心部分遵循的标准

    * 文档对象模型（`DOM`）
    * 浏览器对象模型（`BOM`）
        > `DOM` 和 `BOM` 是各个浏览器厂商基于 `ECMAScript` 标准来实现 `JavaScript` 的扩展部分

5. `ECMAScript` 与 `Web` 浏览器没有依赖关系，`Web` 浏览器只是 `ECMAScript` 实现可能的**宿主环境**之一。**遵循 `ECMAScript` 标准的脚本语言不止 `JavaScript`！**示例：`Adobe ActionScript`
> 其他的宿主环境如：`Node`、`Adobe Flash`

6. `ECMAScript` 规定了遵循标准的系列语言的基本组成部分
    * 语法
    * 类型
    * 语句
    * 关键字
    * 保留字
    * 操作符
    * 对象
7. 重要版本更新
    * `ES` 初代版本只是在 `Ns` 的 `JavaScript` 标准下新增加两个特性：`Unicode` 支持以及与平台无关特性；
    * `ES3` 正式开始大幅度增添新特性：错误与异常处理、正则表达式
    * 废弃的 `ES4`：强类型、新语法、类与继承
    * 顶替 `ES4` 的版本是 `ES3.1`
    * `ES 3.1` 后来发展成为了 `ES5`（2009.12）：新功能包括 `JSON` 对象、严格模式
    * `ES 6`（2015.6）
8. 一门语言要成为 `ECMAScript` 的实现，需要：
    * 支持第 6 点里标准的基本组成；
    * **支持 `Unicode` 字符标准；**[链接：Unicode 与 JavaScript](http://www.ruanyifeng.com/blog/2014/12/unicode.html)
    * 添加 `ECMAScript` 没有规定的新对象和对象的属性；
    * 可以修改和扩展内置的正则表达式语法。

9. `JavaScript` 的 `DOM` -- 文档对象模型，针对 `XML`，经过扩展用于 `HTML` 的应用程序编程接口（`API`）
10. `DOM` 标准由负责制定通信标准的 W3C（万维网联盟）与各个大型浏览器厂商制定
11. **`DOM Level 0` 标准是不存在的**，只是大家对 `IE 4.0` 和 `Ns` 最初支持的 `DOM` 的 **昵称**
12. `DOM Level 1` 的组成：
    * `DOM Core`：基础部分，通用于 `XML、HTML` 系列标记语言
    * `DOM HTML`：针对 `HTML` 做的新扩展
13. 许多语言都实现了 `DOM`，不只有依托浏览器宿主的 `JavaScript`。如：`SVG`、`MathML` 等；
14. `BOM` **控制浏览器显示页面以外的部分**，处理浏览器窗口与框架
15. `BOM` 没有相关的标准，各个浏览器厂商的浏览器都有一定的不同（可参考各个浏览器的开发者文档），`HTML5` 致力于把许多 `BOM` 写入规范！
