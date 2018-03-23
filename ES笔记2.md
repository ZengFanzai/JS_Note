# **ES基本语法二**

- [模板字符串](#模板字符串)
- [箭头函数](#箭头函数)
- [Promise对象](#箭头函数)

## 模板字符串

- 模板字面量（Template literals） 是允许嵌入表达式的字符串字面量。你可以使用多行字符串和字符串插值功能。它们在ES2015规范的先前版本中被称为“模板字符（template strings）”。
- 可以定义多行字符串，可以当普通字符串使用
- 在字符串中嵌入变量

- ```js

    //多行字符串
    console.log(`string text line 1
    string text line 2`);
    // "string text line 1
    // string text line 2"
    var a = 5;
    var b = 10;
    //表达式插入
    console.log(`Fifteen is ${a + b} and\nnot ${2 * a + b}.`);
    // "Fifteen is 15 and
    // not 20."
    //带标签的模板字符串
    let name = '张三',
    age = 20,
    message = show`我来给大家介绍:${name}的年龄是${age}.`;
    function show(stringArr,...values){
        let output ="";
        let index = 0
        for(;index<values.length;index++){
            output += stringArr [index]+values[index];
        }
            output += stringArr [index];
            return output;
    }
        message;       //"我来给大家介绍:张三的年龄是20."
    ```

## **箭头函数**

- ```js

    var f = v => v
    //等价于
    var f = function(v){
        return v;
    }
    ```

## **Promise对象**

- Promise是异步编程的一种解决方案，比传统的解决方案——回调函数和事件，更合理和强大

- 所谓Promise，简单说就是一个**容器**，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。
    1. **对象的状态不受外界影响。** Promise对象代表一个**异步操作**，有三种状态：**pending（进行中）、fulfilled（已成功）和rejected（已失败）**。只有异步操作的**结果**，可以**决定**当前是哪一种**状态**，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“**承诺**”，表示其他手段无法改变。
    2. **一旦状态改变，就不会再变，任何时候都可以得到这个结果。** Promise对象的状态改变，**只有两种可能：从pending变为fulfilled和从pending变为rejected。** 只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果，这时就称为 resolved（**已定型**）。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

### 基本用法

- ES6 规定，Promise对象是一个构造函数，用来生成Promise实例。
    ```js
    const promise = new Promise(function(resolve, reject) {
    // ... some code

        if (/* 异步操作成功 */){
            resolve(value);
        } else {
            reject(error);
        }
    });
    ```
    Promise构造函数接受一个函数作为参数，该函数的两个参数分别是resolve和reject。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

    **resolve函数的作用是**，将Promise对象的状态从“未完成”变为“成功”（即从 pending 变为 resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；

    **reject函数的作用是**，将Promise对象的状态从“未完成”变为“失败”（即从 pending 变为 rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去

    Promise实例生成以后，可以用then方法分别指定resolved状态和rejected状态的回调函数。

    ```js
    promise.then(function(value) {
    // success
    }, function(error) {
    // failure
    });
    ```

    第一个回调函数在Promise对象的状态变为resolved时调用

    第二个回调函数是Promise对象变为rejected时调用

    ```js
    function timeout(ms) {
        return new Promise((resolve, reject) => {
            setTimeout(resolve, ms, 'done');
        });
    }

    timeout(100).then((value) => {
        console.log(value);
    });
    ```

    上面代码中，timeout方法返回一个Promise实例，表示一段时间以后才会发生的结果。过了指定的时间（ms参数）以后，Promise实例的状态变为resolved，就会触发then方法绑定的回调函数。

    。。。

    [原文参考——ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/promise)
