# **JavaScript基础语法**

- [IIFE](#IIFE)
- [函数参数和函数内部定义的变量重名](#函数参数和函数内部定义的变量重名)
- [闭包](#闭包解答)
- [函数提升和变量提升](#函数提升和变量提升)
- [Apply,Call,Bind用法对比](#abc)
- []()

## **IIFE** <a id="IIFE"></a>

- IIFE：立即调用函数表达式，在定义时就会立即执行的JS函数
- 这是一个被称为**自执行匿名函数**的设计模式，包含两部分。
  1. 包围在**圆括号运算符()**里的一个**匿名函数**，这个匿名函数拥有**独立的词法作用域**。**避免了外界访问此IIFE中的变量，而且不会污染全局作用域**
  2. 再次使用()创建一个立即执行函数表达式，JS引擎到此将直接执行函数。

  ```js
  (function(){
      var name = "Barry";
  })();
  //外部不能访问变量name
  name //undefined
  ```

  1. 将IIFE分配给一变量，不是存储IIFE本身，而是存储IIFE执行后返回的结果。

  ```js
  var result = (function(){
      var name = "Barry";
      return name;
  })();
  //IIFE执行后返回的结果
  result; //"Barry"
  ```

## **函数参数和函数内部定义的变量重名**

- 在函数体内部的局部变量的优先级高于同名的全局变量，如果在函数体内声明的一个局部变量或者函数参数中带有的变量和全局重名，那么全局变量就被局部变量所覆盖。

## **闭包解答**

- 闭包是函数和声明该函数的词法环境的组合

### **词法作用域**

- 考虑如下情况

  ```js
  function init() {
    var name = "Mozilla"; // name 是一个被 init 创建的局部变量
    function displayName() { // displayName() 是内部函数,一个闭包
        alert(name); // 使用了父函数中声明的变量
    }
    displayName();
  }
  init();
  ```

  init()创建了一个局部变量name和一个名为displayName()的函数。displayName()仅在该函数体内可用。displayName() 可以使用父函数 init() 中声明的变量 name 。但是，如果有同名变量 name 在 displayName() 中被定义，则会使用的 displayName() 中定义的 name 。

### **闭包**

- 考虑如下例子

    ```js
    function makeFunc() {
        var name = "Mozilla";
        function displayName() {
            alert(name);
        }
        return displayName;
    }

    var myFunc = makeFunc();
    myFunc();
    ```
    JS中的函数会形成闭包，闭包是由函数以及创建该函数的词法环境组合而成。这个环境包含了这个闭包创建时所能访问的所有局部变量。
    ```js
    function makeAdder(x) {
        return function(y) {
        return x + y;
        };
    }
    var add5 = makeAdder(5);
    var add10 = makeAdder(10;
    console.log(add5(2));// 7
    console.log(add10(2))//12
    ```
- [**闭包参考**](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)

## **函数提升和变量提升**

### **变量提升**

- ES6之前 没有块级作用域
- 通常JS引擎会在正式执行前先进行一次预编译，在这个过程中，首先将变量声明及函数声明提升至当前作用域的顶端，然后进行接下来的处理。
- eg.

  ```js
  //var foo = 10; //打印5
  function hoistVariable() {
    if (!foo) {
        var foo = 5;
    }
    console.log(foo); // 5
  }
  hoistVariable();
  ```

  运行代码，我们会发现foo的值是5，如果外层作用域也存在一个foo变量，就更加困惑了，该不会是打印外层作用域中的foo变量吧？答案是：不会，如果当前作用域中存在此变量声明，无论它在什么地方声明，引用此变量时就会在当前作用域中查找，不会去外层作用域了。

### 函数提升

- eg.

  ```js
  function hoistFunction() {
    foo(); // output: I am hoisted
        function foo() {
            console.log('I am hoisted');
        }
    }
    hoistFunction();
    ```
  预编译后

  ```js

  //预编译之后
  function hoistFunction() {
    function foo() {
        console.log('I am hoisted');
    }
    foo(); // output: I am hoisted
  }
  hoistFunction();

  ```
  相似的，如果在同一个作用域中存在多个同名函数声明，后面出现的将会覆盖前面的函数声明：

  ```js

    //预编译之后
    function hoistFunction(){
        function foo() {
            console.log('I am hoisted');
        }
        foo(); // output: I am hoisted
    }
    hoistFunction();

  ```
- **函数声明和函数表达式的提升**
  ```js

    //函数声明
    function foo() {
        console.log('function declaration');
    }

    //匿名函数表达式
    var foo = function() {
        console.log('anonymous function expression');
    };

    //具名函数表达式
    var foo = function bar() {
        console.log('named function expression');
    };

  ```
- 代码如下
  ```js

  function hoistFunction() {
    foo(); // 2
    var foo = function() {
        console.log(1);
    };
    foo(); // 1
    function foo() {
        console.log(2);
    }
    foo(); // 1
  }
  hoistFunction();

  ```
  预编译后
  ```js

    //预编译之后
    function hoistFunction() {
        var foo;
        foo = function foo() {
            console.log(2);
        }
        foo(); // 2
        foo = function() {
            console.log(1);
        };
        foo(); // 1
        foo(); // 1
    }
    hoistFunction();

  ```
- 函数提升就是为了解决相互递归的问题
- [JavaScript系列文章：变量提升和函数提升](http://www.cnblogs.com/liuhe688/p/5891273.html)

## **Apply,Call,Bind用法对比** <a id="abc"></a>

### **Bind方法**

- 我们用 Bind() 来实现在指明函数内部 this 指向的情况下去调用该函数, 换句话说, bind() 允许我们非常简单的在函数或者方法被调用时绑定 this 到指定对象上.
- 当我们在一个方法中用到了 this, 而这个方法调用于一个接收器对象, 我们会需要使用到 bind() 方法; 在这种情况下, 由于 this 不一定完全如我们所期待的绑定在目标对象上, 程序有时便会出错;

#### Bind允许我们明确指定方法中的this指向

```js

// <button>Get Random Person</button>​
// <input type="text">

var user = {
    data        :[
        {name:"T. Woods", age:37},
        {name:"P. Mickelson", age:43}
    ],
    clickHandler:function(event) {
        var randomNum = ((Math.random () * 2 | 0) + 1) - 1; // random number between 0 and 1​
​
        // 从 data 数组中随机选取一个名字填入 input 框内
        $("input").val(this.data[randomNum].name + " " + this.data[randomNum].age);
    }
}
​
// 给点击事件添加一个事件处理器
$("button").click(user.clickHandler);

```
  当点击按钮时，会发现一个报错信息：因为 clickHandler() 方法中的 this 绑定的是按钮 HTML 内容的上下文, 因为这才是 clickHandler 方法的执行时的调用对象.

  为了解决之前例子中存在的问题, 我们利用 bind() 方法将 $("button").click(user.clickHandler); 换成以下形式:

  ```js

  $("button").click(user.clickHandler.bind(user));

  ```

#### Bind 方法允许我们实现函数借用

- 在 JavaScript 中, 我们可以传递函数, 返回函数, 借用他们等等, 而 bind() 方法使函数借用变得极其简单. 以下为一个函数借用的例子:

```js

// car 对象
var cars = {
    data:[{name:"Honda Accord", age:14},
    {name:"Tesla Model S", age:2}
    ]
}
// 假如我们从user对象中借用showData方法
// 这里我们将user.showData方法绑定到刚刚新建的cars对象上
cars.showData = user.showData.bind(cars);
cars.showData(); // Honda Accord 14

```

#### Bind方法允许我们柯里化一个函数

>柯里化的概念很简单, 只传递给函数一部分参数来调用它, 让它返回一个函数去处理剩下的参数. 你可以一次性地调用 curry 函数, 也可以每次只传一个参数分多次调用,如下

```js

var add = function(x) {
  return function(y) {
    return x + y;
  };
};
var increment = add(1);
var addTen = add(10);
increment(2);
// 3
addTen(2);
// 12

```

- 使用bind()方法来实现函数的柯里化
```js

function greet(gender, age, name){
    //if a male,use Mr., else use Ms.
    var salutation = gender === "male" ? "Mr. " : "Ms. ";
    if (age > 25) {
        return "hello, " + salutation + name + ".";
    } else {
        return "Hey, " + name + ".";
    }
}

```

使用bind()方法柯里化greet()方法。bind()接收的第一个参数指定了this的值

```js

 // 在 greet 函数中我们可以传递 null, 因为函数中并未使用到 this 关键字
var greetAnAdultMale = greet.bind (null, "male", 45);
​
greetAnAdultMale("John Hartlove"); // "Hello, Mr. John Hartlove."​
​
var greetAYoungster = greet.bind(null, "", 16);
greetAYoungster("Alex"); // "Hey, Alex."​
greetAYoungster("Emma Waterloo"); // "Hey, Emma Waterloo."​

```

- bind() 方法允许我们明确指定对象方法中的 this 指向, 我们可以借用, 复制一个方法或者将方法赋值为一个可作为函数执行的变量. 我们以可以借用 bind 实现函数柯里化.

### **Apply和Call方法**

- apply和call允许我们借用函数以及在函数调用中指定this指向。
- 除此之外，Apply函数允许我么在执行函数时传入一个参数数组，以此使函数在执行可变参数的函数时可以将每个参数单独传入函数并处理

#### 使用Apply或者Call设置this

- apply 和 call 的用法几乎相同, 唯一的差别在于当函数需要传递多个变量时, apply 可以接受一个数组作为参数输入, call 则是接受一系列的单独变量.

#### 在回调函数中使用Call或者Apply设置this

```js

// 定义一个方法
var clientData = {
    id: 094545,
    fullName: "Not Set",
    // clientData 对象中的一个方法
    setUserName: function (firstName, lastName)  {
        this.fullName = firstName + " " + lastName;
    }
}

function getUserInput(firstName, lastName, callback, callbackObj) {
    // 使用 apply 方法将 "this" 绑定到 callbackObj 对象
    callback.apply(callbackObj, [firstName, lastName]);
}

```
如下样例中传递给 callback 函数 中的参数将会在 clientData 对象中被设置/更新.

```js

getUserInput("Barack", "Obama", clientData.setUserName, clientData);
console.log(clientData.fullName); // Barack Obama​

```

#### **使用 Apply 或者 Call 借用函数(必备知识)**

#### apply() 执行参数可变的函数

```js

// 使用 apply() 执行参数可变的函数
console.log(Math.max(23, 11, 34, 56)); // 56

var allNumbers = [23, 11, 34, 56];
console.log(Math.max(allNumbers)); // NaN

var allNumbers = [23, 11, 34, 56];
console.log(Math.max.apply(null, allNumbers)); // 56

```

### **区别与注意事项**

- **三个函数存在的区别, 用一句话来说的话就是: bind是返回对应函数, 便于稍后调用; apply, call则是立即调用.**
- 除此外, 在 ES6 的箭头函数下, call 和 apply 是失效的, 对于箭头函数来说:
  - 函数体内的 this 对象, 就是定义时所在的对象, 而不是使用时所在的对象;
  - 不可以当作构造函数, 也就是说不可以使用 new 命令, 否则会抛出一个错误;
  - 不可以使用 arguments 对象, 该对象在函数体内不存在. 如果要用, 可以用 Rest 参数代替;
  - 不可以使用 yield 命令, 因此箭头函数不能用作 Generator 函数;

[JavaScript 中至关重要的 Apply, Call 和 Bind](https://hijiangtao.github.io/2017/05/07/Full-Usage-of-Apply-Call-and-Bind-in-JavaScript/)


## **&lt;script&gt;、&lt;script async&gt;、&lt;script defer&gt;**

```js

<script src="script.js"></script>

```

- 没有 defer 或 async，浏览器会立即加载并执行指定的脚本，“立即”指的是在渲染该 script 标签之下的文档元素之前，也就是说不等待后续载入的文档元素，读到就加载并执行。

```js

<script async src="script.js"></script>

```

- 有 async，加载和渲染后续文档元素的过程将和 script.js 的加载与执行并行进行（异步）。

```js

<script defer src="myscript.js"></script>

```
- 有 defer，加载后续文档元素的过程将和 script.js 的加载并行进行（异步），但是 script.js 的执行要在所有元素解析完成之后，DOMContentLoaded 事件触发之前完成。


