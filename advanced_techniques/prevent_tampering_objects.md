# 防篡改对象
1. 任何对象（包括原生对象）都可以被在同一环境（以及下一级环境）中运行的代码所修改，有时候这些修改使我们不经意的，我们并不需要的；

2. ES 5 引入了几种创建 **防篡改对象** 的方法；

3. **注意点**：**一旦对象定义为防篡改，则在运行过程中无法撤销**；

4. 第六章讨论了对象属性的特性和方法：`[[Configurable]]`、`[[Writable]]`、`[[Enumerable]]`、`[[Value]]`、`[[Get]]`、`[[Set]]` 也可以改变对象属性的行为

## 不可扩展对象
1. 默认情况下，所有对象都是可扩展的；

2. 使用 `Object.preventExtensions()` 方法可以为对象指定不可扩展性
    ```javascript
    var person = {
        name: 'yangfch3'
    }

    Object.preventExtensions(person);

    person.age = 21;
    alert(person.age); // undefined

    person.name = 'liuhq3';
    alert(person.name); // 'liuhq3'

    delete person.name;
    person.name; // undefined
    ```

3. 虽然不可扩展，但仍旧可以修改和删除对象的已有属性和方法；

4. `Object.isExtensible()` 方法可以确定对象是否支持扩展。

## 密封的对象
1. 密封对象不可扩展，而且已有成员的 `[[Configurable]]` 将被设置为 `false`，**无法删除属性和方法，不能使用 `Object.defineProperty()` 把数据属性修改为访问器属性，但是可以修改已有属性值、方法值**；

2. 使用 `Object.seal()` 方法密封对象
    ```javascript
    var person = {
        name: 'yangfch3'
    }

    Object.seal(person);

    person.age = 21;
    alert(person.age); // undefined

    delete person.name;
    person.name; // 'yangfch3'
    ```

3. 使用 `Object.isSealed()` 方法可以确定对象是否被密封；

4. 被密封的对象必然也是必然不可扩展的，所以使用 `Object.isExtensible()` 方法检测密封对象也会返回 `false`。

## 冻结的对象
1. `frozen object` 是对对象做的最严密的保护措施；

2. 冻结的对象既不可以扩展，又是密封的，而且对象数据属性的 `[[Writable]]` 特性会被设置为 `false`

3. `Object.freeze()` 冻结对象

4. `Object.isFrozen()` 检查对象是否被冻结，一旦对象被冻结，检测可拓展、密封情况都会返回不可扩展、已密封。

5. 冻结对象常用于一些库或框架，冻结对象以防止你对库或框架里的重要对象进行误改写。
