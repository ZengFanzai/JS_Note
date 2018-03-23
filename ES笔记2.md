# **ES基本语法二**
## 模板字符串
+ 模板字面量（Template literals） 是允许嵌入表达式的字符串字面量。你可以使用多行字符串和字符串插值功能。它们在ES2015规范的先前版本中被称为“模板字符（template strings）”。
+ 可以定义多行字符串，可以当普通字符串使用
+ 在字符串中嵌入变量
+ ```
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
+ ```
    var f = v => v
    //等价于
    var f = function(v){
        return v;
    }
    ```
## **Promise对象**
+ ### Promise是异步编程的一种解决方案，比传统的解决方案——回调函数和事件，更合理和强大
+ 所谓Promise，简单说就是一个<font color="green" size=4>**容器**</font>，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。