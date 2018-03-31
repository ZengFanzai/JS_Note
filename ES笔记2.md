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

    **即：用resolve函数和reject函数时带有参数，那么它们的参数会被传递给回调函数。**
    **reject函数的参数通常是Error对象的实例，表示抛出的错误；resolve函数的参数除了正常的值以外，还可能是另一个 Promise 实例，比如像下面这样。**
    ```js
    const p1 = new Promise(function (resolve, reject) {
    // ...
    });

    const p2 = new Promise(function (resolve, reject) {
    // ...
    resolve(p1);
    })
    ```
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

    注意，调用resolve或reject并不会终结 Promise 的参数函数的执行。
    ```js
    new Promise((resolve, reject) => {
        resolve(1);
        console.log(2);
    }).then(r => {
        console.log(r);
    });
    // 2
    // 1
    ```

### Promise.prototype.then()

- then方法的**第一个参数是resolved状态的回调函数**，**第二个参数（可选）是rejected状态的回调函数。**
    ```js
    getJSON("/post/1.json").then(
        post => getJSON(post.commentURL)
    ).then(
        comments => console.log("resolved: ", comments),
        err => console.log("rejected: ", err)
    );
    ```

### Promise.prototype.catch()

- Promise.prototype.catch方法是 **.then(null, rejection)的别名**，用于指定发生错误时的回调函数。
    ```js
    getJSON('/posts.json').then(function(posts) {
        // ...
    }).catch(function(error) {
        // 处理 getJSON 和 前一个回调函数运行时发生的错误
        console.log('发生错误！', error);
    });
    ```

### Promise.prototype.finally()

- finally方法用于指定不管 Promise 对象最后状态如何，都会执行的操作。
    ```js
    promise
    .then(result => {···})
    .catch(error => {···})
    .finally(() => {···});
    ```

### Promise.all()

- Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例.
    ```js
    const p = Promise.all([p1, p2, p3]);
    ```
  - Promise.all方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。
  - p的状态由p1、p2、p3决定，分成两种情况。

    （1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

    （2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

### Promise.race()

- Promise.race方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例。
    ```js
    const p = Promise.race([p1, p2, p3]);
    ```
  - 上面代码中，只要p1、p2、p3之中有一个实例率先改变状态，p的状态就跟着改变。那个率先改变的 Promise 实例的返回值，就传递给p的回调函数。
  - Promise.race方法的参数与Promise.all方法一样，如果不是 Promise 实例，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。

### Promise.resolve()

- 有时需要将现有对象转为 Promise 对象，Promise.resolve方法就起到这个作用
    ```js
    const jsPromise = Promise.resolve($.ajax('/whatever.json'));


    Promise.resolve('foo')
    // 等价于
    new Promise(resolve => resolve('foo'))
    ```
- Promise.resolve方法的参数分成四种情况。
  - 参数是一个 Promise 实例
    - Promise.resolve将不做任何修改、原封不动地返回这个实例。
  - 参数是一个thenable对象 thenable对象指的是具有then方法的对象
    ```js
    let thenable = {
        then: function(resolve, reject) {
            resolve(42);
        }
    };
    ```
    - 转为 Promise 对象，然后就立即执行thenable对象的then方法。
  - 参数不是具有then方法的对象，或根本就不是对象
    - 返回一个新的 Promise 对象，状态为resolved。
  - 不带有任何参数
    - Promise.resolve方法允许调用时不带参数，直接返回一个resolved状态的 Promise 对象。所以，如果希望得到一个 Promise 对象，比较方便的方法就是直接调用Promise.resolve方法。
    ```js
    const p = Promise.resolve();

    p.then(function () {
    // ...
    });
    ```

### Promise.reject()

- Promise.reject(reason)方法也会返回一个新的 Promise 实例，该实例的状态为rejected。
    ```js
    const p = Promise.reject('出错了');
    // 等同于
    const p = new Promise((resolve, reject) => reject('出错了'))

    p.then(null, function (s) {
        console.log(s)
    });
    // 出错了
    ```

### Promise.try()

。。。

[原文参考——ECMAScript 6 入门](http://es6.ruanyifeng.com/#docs/promise)
