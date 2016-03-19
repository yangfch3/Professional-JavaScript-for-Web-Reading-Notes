# XMLHttpRequest 2 级
基本的 `XMLHttpRequest` 1 级 规定了 `XHR` 对象的基本属性、方法和实现细节，`XMLHttpRequest` 2 级 规范则使得 `XHR` 对象更加现代化。

## FormData
1. 现代 Web 应用中频繁使用的一项功能是 **表单数据的序列化**；

2. `FormData` 类型为 **序列化表单** 以及 **创建与表达格式相同的数据** （用于通过 `XHR` 传输提供了便利）；

3. 实例化 `FormData` 对象
    * 参数（可选）：表单（`Form`）元素
    ```javascript
    // 实例化一个空的 FormData 对象
    var data = new FormData();

    // 基于已有表单元素及其数据实例化一个 FormData 对象
    var data = new FormData(document.forms[0]);
    ```

4. `FormData` 实例有 `append()` 方法
    * 参数一：键
    * 参数二：值
    ```javascript
    var data = new FormData(document.forms[0]);
    data.append('name', 'yangfch3');
    ```

5. `FormData` 实例对象可以直接传给 `XHR` 对象的 `send()` 方法；

6. **使用 `FormData` 实例构建传输表单数据时，不需要再为 `XHR` 对象设置请求头。**
    >`XHR` 对象能识别传入的数据类型是 `FormData` 类型的实例，并自动配置适当的头部信息

## 超时设定
1. `XHR` 对象的 `timeout` 属性用于指定请求超时极限，请求超时会触发 `ontimeout` 事件处理程序；

2. 在超过超时事件限制时，请求自动终止，在请求终止之后再访问 `status` 属性，会发生错误，所以 `onreadystatechange` 的 `status` 判断语句块要放到 `try-catch` 语句块中。
