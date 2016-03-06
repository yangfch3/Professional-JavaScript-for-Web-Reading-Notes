# 专有扩展

## 插入文本
`innerText` `outerText`

### innerText
1. 读取元素中包含的所有文本内容，只读取文本内容；

2. 写入时会删除元素的所有节点，插入文本内容；

3. 不同浏览器处理空白和换行的方式不一样；

4. FF 不支持 `innerText`，但支持 `textContent` 属性；
    > `textContent` 与 `innerText` 的不同点
`innerText` 会忽略行内的样式和脚本，`textContent` 不会；**最佳实践：从不包含行内样式和行内脚本的 DOM 片段中读取文本**

5. **使用 `innerText` 可以过滤标签，快捷地获得纯文本**。

```javascript
function getInnerText(element) {
    return (typeof element.textContent == 'string') ? element.textContent : element.innerText;
}

function setInnerText(element, text) {
    if (typeof element.textContent == 'string') {
        element.textContent = text;
    } else {
        element.innerText = text;
    }
}
```

## 滚动相关
* `scrollIntoView()` 对元素调用此方法可以使页面滚动到元素顶部平齐；

* `scrollIntoViewIfNeeded(alignCenter)` 调用方法的元素不可见的情况下运行，接受**一个参数：是否垂直居中布尔值**

* `scrollByLines(lineCount)` 将元素内容滚动到指定的行高

* `scrollByPages(pageCount)` 将元素内容滚动至指定的页面高度

以上三个方法支持性都不是很好！
