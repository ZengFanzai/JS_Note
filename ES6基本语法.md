# <center>**ES6基本语法**</center>
## **问题 #1：JS没有块级作用域**
###  在JS函数中的var声明，其作用域是函数体的全部。
+ 在代码块内声明的变量，其作用域是整个函数作用域而不是块级作用域。
+ 不足之处
    + 假如你现在的代码使用了一个变量t：
    ```
        function runTowerExperiment(tower, startTime) {
        var t = startTime;
        tower.on("tick", function () {
            ... 使用了变量t的代码 ...
        });
        ... 更多代码 ...
        }
    ```
    到目前为止，一切都很顺利。现在你想添加测量保龄球速度的功能，所以你在回调函数内部添加了一个简单的if语句。
    ```
        function runTowerExperiment(tower, startTime) {
        var t = startTime;
        tower.on("tick", function () {
            ... 使用了变量t的代码 ...
            if (bowlingBall.altitude() <= 0) {
            var t = readTachymeter();
            ...
            }
        });
        ... 更多代码 ...
        }
    ```
    之前那段“使用了变量t的代码”运行良好，现在你无意中添加了第二个变量t，这里的t指向的是一个新的内部变量t而不是原来的外部变量。
## **问题 #2：循环内变量过度共享**
+ 执行下面代码
    ```
    var messages = ["嗨！", "我是一个web页面！", "alert()方法非常有趣！"];
    for (var i = 0; i < messages.length; i++) {
      alert(messages[i]);
    }
    ```
    ```
    var messages = ["喵！", "我是一只会说话的猫！", "回调（callback）非常有趣!"];
    for (var i = 0; i < messages.length; i++) {
      setTimeout(function () {
        cat.say(messages[i]);
      }, i * 1500);
    }
    ```
## **let**
+ let所声明的变量，只在let命令所在的代码块内有效
+ **let声明的变量拥有块级作用域**，也就是说let声明的变量的作用域只是外层块，而不是整个外层函数
+ let声明的全局变量不是全局对象的属性。这意味着，你不可以通过windows.变量名的方式访问这些变量。它们只存在于一个不可见的块的作用域中，这个块理论上是Web页面中运行的所有JS代码的外层块。
+ **形如for (let x...)的循环在每次迭代时都为x创建新的绑定。**
+ **let声明的变量直到控制流到达该变量被定义的代码行时才会被装载，所以在到达之前使用该变量会触发错误。**
+ ```
    function update() {
      console.log("当前时间:", t);  // 引用错误（ReferenceError）
      ...
      let t = readTachymeter();
    }
    ```
    ```
    var a = []
    for (var i = 0; i < 10; i++){
        a[i] = function(){****
            console.log(i)
        }
    }
    a(6)[]

    <!-- print 10 -->
    ```
+ **用let重定义变量会抛出一个语法错误（SyntaxError）。**

## **const**
+ const声明的变量与let声明的变量类似，它们的不同之处在于，const声明的变量只可以在声明时赋值，不可随意修改，否则会导致SyntaxError（语法错误）。
+ ```
    const MAX_CAT_SIZE_KG = 3000; // 正确
 
    MAX_CAT_SIZE_KG = 5000; // 语法错误（SyntaxError）
    MAX_CAT_SIZE_KG++; // 虽然换了一种方式，但仍然会导致语法错误
    const theFairest;  // 依然是语法错误，你这个倒霉蛋
  ```
[**参考链接---InfoQ**](http://www.infoq.com/cn/articles/es6-in-depth-let-and-const)