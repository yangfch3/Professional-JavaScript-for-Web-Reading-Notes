# 选择符 API
选择符 API 允许直接使用 CSS 选择器来选取页面元素，众多库推进了选择符 API 的标准化。

选择符 API 性能因为原生，所以性能必定强于各个库同类型的查询操作。

## querySelector()
1. Selector API **Level 1**；

2. 接收一个 CSS 选择符（可复杂），**返回匹配的第一个元素** 或返回 `null`；

3. 传入不支持的选择符会抛出错误；

4. 能调用 `querySelector()` 的对象为：`Document` 类型、`Element` 类型、`DocumentFragment` 类型。


## querySelectorAll()
1. Selector API Level 1；

2. 接收一个 CSS 选择符（可复杂），**返回所有匹配元素** 组成的 `NodeList` 实例或 `null`；

3. 传入不支持的选择符会抛出错误；

4. 能调用 `querySelector()` 的对象为：`Document` 类型、`Element` 类型、`DocumentFragment` 类型。

## matchesSelector()
1. Selector API **Level 2**；

2. 接收一个 **`CSS` 选择符** 做参数，比较选择符与调用方法的元素是否匹配，返回布尔值；

3. 各个浏览器支持性不同：
    > `document.body.msMatchesSelector();` IE 下等同的专有扩展
    `document.body.matches();` Chrome 下等同的专有扩展
