# 表单序列化
1. 为什么需要进行表单序列化？
    > `Ajax` 可以为我们异步地发送表单数据，但在发送之前我们需要先进行手工地序列化（当然可以使用 `XMLHttpRequest` 的新 `API`：`FormData`，但被兼容性限制）。

2. 序列化的具体要求
    * 对表单字段的名称和值进行 `URL` 编码，使用 `&` 分隔
    * 不发送禁用的表单字段
    * 只发送勾选的复选框和单选按钮
    * 不发送 `type` 为 `reset` 和 `button` 的按钮
    * 多选选择框中的每个选中的值单独一个条目
    * 在单击提交按钮提交表单的情况下，也会发送提交按钮；否则不发送提交按钮。也包括 `type` 为 `image` 的 `input` 元素
    * `select` 元素下 `option` 元素的值
        * 有 `value` 特性时选择 `value` 特性的值
        * 没有 `value` 特性时选择 `option` 元素的

3. 序列化表单函数
    ```javascript
    function serialize(form) {
    if (!form || form.nodeName.toLowerCase() !== "FORM".toLowerCase()) {
            throw new Error('serialize(): The only arg must be form element!')
    }

    var parts = [],
        filed = null,
        i,
        len,
        j,
        optLen,
        option,
        optValue;

    for (i = 0, len = form.elements.length; i < len; i++) {
        field = form.elements[i];

        if (field.name === '') {
            continue;
        }

        switch(field.type) {
            case 'select-one':
            case 'select-multiple':
                for (j = 0, optLen = field.options.length; j < optLen; j++) {
                    option = field.options[j];
                    if (option.selected) {
                        optValue = '';
                        if (option.hasAttribute) {
                            optValue = (option.hasAttribute('value') ? option.value : option.text);
                        } else {
                            optValue = (option.attributes['value'].specified ? option.value : option.text);
                        }

                        parts.push(encodeURIComponent(field.name) + '=' + encodeURIComponent(optValue));
                    }
                }
                break;
            case undefined:
            case 'file':
            case 'submit':
            case 'reset':
            case 'button':
                break;
            case 'radio':
            case 'checkbox':
                if (!field.checked) {
                    break;
                }
            default:
                parts.push(encodeURIComponent(field.name) + '=' + encodeURIComponent(field.value));
        }
    }

    return parts.join('&');
}
    ```

4. 序列化表单的注意点：
    * `select` 元素最麻烦，可能是单选框，也可能是多选框；`value` 特性可能有确定值，也可能没有 `value` 特性，也可能有 `value` 特性但没有但值为空。
        * 多选框需要遍历控件中的每一个选项
        * 遇到没有 `value` 特性或者 `value` 特性为空的（用 `hasAttribute()` 或 `specified` 属性（IE）检测），`value` 值取选项的文本值
    * 表单中包含 `fieldset` 元素也会出现 `form.elemnts` list 内，可以使用 `type` 属性为 `undefined` 来排除；
    * 文件输入字段无法模拟，序列化时一般忽略。
