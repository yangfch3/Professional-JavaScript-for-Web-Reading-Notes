# Object 类型
1. `Object` 类型是 `ES` 中使用最多的一个类型；

2. `Object` 实例不具备多少功能，主要用于程序中存储和传输数据；

3. 对象字面量表示法用于对象定义的优点：简化创建包含大量属性的对象的过程；
    ```javascript
    var person = new Object();
    person.name = 'Nicholas';
    person.age = 29;
    ```
    ```javascript
    var person = {
        name: 'Nicholas',
        age: 29
    };
    ```
    ```javascript
    var person = {};
    person.name = 'Nicholas';
    person,age = 29;
    ```
4. **表达式上下文**(expression context)：指的是能够返回一个值
**语句上下文**(statement context)：表示一个语句块的开始
    > 上下文：指的是有歧义的代码或符号根据上下 context 决定效果取向，类似高中的阅读理解：请联系上下文回答问题。

5. `{}` 可以用于 **指示对象实例**，还可以用于 **包住代码块**。
能准确识别是因为赋值操作符 `=` 表示后面是一个待返回的值的表达式，这时 `{` 就表示一个表达式的开始（对象字面量的开始）。
`{` 跟在 `if` 语句的后面则表征一个语句块的开始，不会视为对象字面量解析。

6. 对象属性名全部默认为字符串，属性为数值会被转换为字符串，可使用引号，也可不使用；属性名可以包含特殊字符、保留字、关键字！
    ```javascript
    var a = {
        '!@#$%^': 'test', // 不推荐包含特殊字符
        'first name': 'Fucheng', // 不推荐使用空格分割单词
        lastName: 'Young' // 规范写法
    }

    alert(a['!@#$%^']); // 使用 [] 正常访问
    alert(a['first name']); // 使用 []
    ```

6. **最佳实践**：
    1. 最后一个属性不添加逗号；（`IE 7`, `Opara` 视为语法错误）
    2. **属性名：不使用引号，不使用保留字，关键字，不使用特殊符号；**

7. 对象字面量法 `{}` 定义对象实际上并不是调用 `Object` 构造函数；

8. 对象字面量法：代码量少，能够给人封装数据的感觉；

9. **对象常常用来向函数传递大量可选参数**，这样能防止为函数定义过多的命名参数：
    ```javascript
    function displayInfo(args, role) {
        var output = ""; // 不要只声明，不初始化

        // 可选参数使用对象字面量传入，函数内部判断、取舍
        if (typeof args.name === 'string') {
            output += 'Name: ' + args.name + '\n';
        }

        if (typeof args.age === 'number') {
            output += 'Age: ' + args.age + '\n';
        }

        // 必需值使用命名参数
        output += 'Role: ' + role + '\n';

        alert(output);
    }

    // 包装数据，准备传入函数
    var yangfch3 = {
        name: 'yangfch3',
        age: 21
    }

    displayInfo(yangfch3, 'husband');
    displayInfo({
        name: 'liuhq3'
    }, 'wife');
    ```
11. **最佳实践**：定义函数时，必需值使用指定的命名参数，而使用对象字面量来封装多个可选参数（见 **No.10** ）；

12. `JavaScript` 也可以使用方括号表示法来访问对象的属性，注意：使用方括号时，属性以字符串形式放在方括号内
    ```javascript
    alert(husband['name']); // yangfch3
    alert(wife.name); // liuhq3
    ```

13. 方括号语法的优点：一下两种情况使用 `[]`，其他情况一律使用 `.`
    1. 可以通过变量来访问属性，
    2. 如果属性名包含会导致语法错误的特殊字符，或者属性名使用的是关键字或者保留字，使用方括号访问包容性强一些。
    ```javascript
    var propertyName = 'name';
    alert(person[propertyName]); // yangfch3
    alert(person.propertyName); // error: propertyName is not defined
    alert(person['first name']); // 空格是特殊字符，使用 []
    alert(person['!@#$%^&*']); // 属性名包含特殊字符，使用 []
    ```
