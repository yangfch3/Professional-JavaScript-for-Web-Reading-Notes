# JavaScript in HTML

1. `<script>`; 元素由 `Netscape` 创建；

2. `<script>` 废弃的 `language` 属性指定脚本语言的种类（早期的浏览器端脚本语言比较多样化，需指定脚本语言的种类）；

3. **在无 `async` 属性的特殊规定外，在解析嵌入式 JS 代码与外部 JS 代码时，页面的其他处理操作会暂停；**

4. `<script>` 元素除 `src`、`type`、`charset` 属性外的两个重要属性：
    * `async`：**异步** 地加载脚本
    * `defer`：**立即下载**，但脚本 **延迟** 到文档完全解析和显示后 **执行**（不是加载）
```javascript
<script defer="defer" src="./scripts/example01.js" >
<!-- defer 与 async 写法不太一样 -->
<script async src="./scripts/example01.js" >
```

5. `type` 的属性基本使用：`text/javascript`
    > 早期，脚本的 `MIME` 类型使用 `type=text/javascript` 和 `type=text/ecmascript`，后来人们认为：`script` 不是纯正的 `text`，而是应该被视为一个应用程序 —— `application`，即 `MIME` 类型应为：`application/javascript`；为了更好地过渡，服务器与浏览器端使用实验性的 `application/x-javascript`；但在部分浏览器无法识别 `type=application/x-javascript` 的脚本，所以现在大部分还是使用原始的 `text/javascript`

6. **最佳实践**：省去 `script` 标签的 `type` 属性（`HTML5` 已默认为 `text/javascript`）

7. **避错原则：**不要在 `inline-script` 内任何地方出现 `"</script>"` 字符串，极易造成误解析
    * 方法1：转移字符 `\` -- `<\/script>`
    * 方法2：`HTML` 实体字符：`<` `>` --- `&lt;` `&gt;`

8. 确保服务器能返回正确的 `MIME` 类型，`.js` 后缀不是浏览器识别脚本文件的标准，而是 `MIME` 类型；

9. 用于 `script` 标签可以引入外部的脚本，再结合浏览器的容错机制，所以我们在引入外部脚本时容易引入恶意的第三方脚本；

10. 现代 `Web` 应用程序一般把全部 `JavaScript` 文件放到 `</body>` 前；

11. `script` 标签的 `defer="defer"` 属性可以使脚本延迟到整个页面 **解析完毕后** 再 **运行** === **立即下载，延迟执行**

12. `defer` 的延迟具体指的是延迟到何时执行？
    > 先于 `DOMContentLoaded` 事件执行

13. A、B 脚本都为 `defer`，A 在 B 前出现，理想的情况：延迟脚本 A 先于延迟脚本 B 执行；现实情况：不一定顺序执行 --- **一个页面最好只包含一个 `defer` 脚本**；

14. 现代浏览器忽略嵌入脚本标签的 `defer` 属性，外部脚本标签的 `defer` 才有效；

15. 使用 `defer` 属性与将脚本放到页面底部效果一样，所以一般 `defer` 属性可以不使用；

16. `async` 只适用于外部脚本，立即下载，同时继续解析页面，脚本下载完成后执行；

17. 多个使用 `async` 属性的外部脚本并不能按照指定顺序先后执行；

18. 使用 `async` 需要满足：
    * 多个异步脚本之间互不依赖（理由：#17）；
    * 异步脚本不能在页面加载期间修改 `DOM`。

19. `noscript` 元素用于在不支持 `JavaScript` 的浏览器中显示替代内容（支持时隐藏）；`noscript` 元素内部可以包含除 `script` 外的任何 `HTML` 元素；

20. `noscript` 因为可以包含 `HTML` 元素，又可以被搜索引擎爬虫抓取，所以可以在一些特殊的情境下担任 `seo` 的角色。


