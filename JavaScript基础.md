# **JavaScript基础**

- [数据类型](#数据类型)
- [DOM](#dom)
- [正则表达式](#正则表达式)

## **数据类型**

- 基本类型：Undefined、Null、Boolean、Numeber、String
- 基本类型间的比较是其值的比较：’{}‘ === '{}' //true
- 引用类型：Object、Array、Function、Date、RegExp、Error、(Map、Set和Symbo)(ES6)
- 引用类型间比较是其引用的比较:{} === {} //false

---

>JS中’===‘和’==‘区别:
>
>- == equality 等同，=== identity 恒等。
>- ==， 两边值类型不同的时候，**要先进行类型转换，再比较**。
>- ===，**不做类型转换**，**类型不同的一定不等**。
>- 先说 ====,下面的规则用来判断两个值是否===相等。
>>1. 如果类型不同，就[不相等]
>>2. 如果两个都是数值，并且是同一个值，那么[相等]；(！例外)的是，如果其中至少一个是NaN，那么[不相等]。（判断一个值是否是NaN，只能用isNaN()来判断）
>>3. 如果两个都是字符串，每个位置的字符都一样，那么[相等]；否则[不相等]。
>>4. 如果两个值都是true，或者都是false，那么[相等]。
>>5. 如果两个值都引用同一个对象或函数，那么[相等]；否则[不相等]。
>>6. 如果两个值都是null，或者都是undefined，那么[相等]。
>- 再说 ==，根据以下规则：
>>1. 如果两个值类型相同，进行 === 比较。
>>2. 如果两个值类型不同，他们可能相等。根据下面规则进行类型转换再比较：
>>3. 如果一个是null、一个是undefined，那么[相等]。
>>4. 如果一个是字符串，一个是数值，把字符串转换成数值再进行比较。
>>5. 如果任一值是 true，把它转换成 1 再比较；如果任一值是 false，把它转换成 0 再比较。
>>6. 如果一个是对象，另一个是数值或字符串，把对象转换成基础类型的值再比较。对象转换成基础类型，利用它的toString或者valueOf方法。 js核心内置类，会尝试valueOf先于toString；例外的是Date，Date利用的是toString转换。
>>7. 任何其他组合，都[不相等]。
>> ![比较图](数值比较.png)
---

- JS拥有**动态类型**，相同变量可以用作不同类型，程序允许时，变量会进行自动转换

```js
var x; //x为undefined
var x = 6; //x为数字
var x = 'Bill' //x为字符串
1 + '2' = '12'; //1转换为‘1’
if(1){} //1转换为true
```

## **DOM介绍**  <a id='dom'></a>

- [深入浅出DOM基础——《DOM探索之基础详解篇》学习笔记](https://github.com/jawil/blog/issues/9)
- 获取DOM节点
  - 获取**id属性**为test的DOM节点: //&lt;div id='test'&gt;&lt;/div&gt;
    ```js
    document.getElementById('test');
    ```
  - 获取**class属性**含有test的DOM节点数组://&lt;div class='test'&gt;&lt;/div&gt;
    ```js
    document.getElementByClassName('test');
  - 获取**name属性**为test的DOM节点数组：//&lt;input name='test'&gt;
    ```js
    document.getElementByName('test');
    ```
  - 获取**标签名**为p的DOM节点数组：//&lt;p&gt;&lt;/p&gt;
    ```js
    document.getElementByTagName('p');
- IE8及以上的现代浏览器支持**querySelector**和**querySelectorAll**方法来获取DOM节点，更加灵活高效，querySelector查询的是第一个满足条件的节点，而querySelectorAll查询的是所有满足条件的节点。
  - ```js
    document.querySelector("#test");
    document.querySelector(".test");
    document.querySelector("p")
    document.querySelector("div.test>p:first-child");
    ```
- DOM内容操作
  - 修改HTML元素内容：
    ```js
    <p id="p1">Hello World!</p>
    document.getElementById("p1").innerHTML = "New Next!";
    ```
  - 修改HTML属性：
    ```js
    <img id="image" src="smiley.gif">
    document.getElementById("image").src = "landscape.jpg";
    ```
  - 修改CSS样式：
    ```js
    <p id="p2">Hello World!</p>
    document.getElementById("p2").style.backgroundColor = "blue"
    ```
  - DOM事件：
    ```js
    <h1 onclick="changetext(this)">请点击该文本</h1>
    function changetext(id){id.innerHTML = "谢谢";}
    ```
- 常全局对象
  - Window对象
    - 所有浏览器都支持window对象，他表示浏览器窗口。
    ```js
    document.getElementById("a") //等同

    window.document.getElementById("a")
    ```
  - Screen对象
    - window.screen对象包含有关用户屏幕的信息。
    - 获取浏览器窗口的尺寸
      - screen.availWidth - 可用的屏幕宽度
      - screen.availHeight - 可用的屏幕高度
  - Location对象
    - window.location 对象用于获得当前页面的地址(URL)，并把浏览器重定向到新页面。
    - 参数：hostname,pathname,port,protocol(http: // or https: //)
  - History对象
    - window.history 对象包含浏览器的历史
    - history.back()-加载历史列表中的前一个URL
    - history.forward()-加载历史列表中的下一个URL
  - JSON对象
    - JSON.parse()：用于将JSON字符转换为JS对象
    - JSON.stringify()：用于将JS对象转化为JSON字符串
- 数据操作
  - Array方法
    方法名|作用
    |:----|:-----|
      join|将数组转化为字符串并连接在一起，可以指定一个字符串用来分隔各个数组元素
      toString|效果与不带参数的join()方法类似
      toLocalString|同上
      reverse|将数组中的元素颠倒
      sort|对数组进行排序
      concat|创建并返回一个新数组，元素包括元素数组和参数
      slice|返回指定数组的一个子数组，两个参数分别指定开始和结束的位置，若只指定一个参数，则返回从该参数开始到数组结束这一子数组
      splice|在数组中插入或截取元素
      push|在数组尾部添加一个或多个元素，并返回数组新的长度
      pop|删除数组最后一个元素并将其返回
      ...|...
      [Array数组方法参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array)
  - String方法
    方法名|作用
    |:----|:----|
      split|把一个字符串根据参数分割为字符串数组
      charCodeAt|返回一个整数，代表指定位置字符的Unicode编码
      fromCharCode|从一些Unicode字符串返回一个字符串
      charAt|返回指定索引位置处的字符。如果超出有效范围的索引返回空字符串
      trim|去掉字符串前后的空格
      ...|...
      [String方法参考](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)
  - Math方法
    方法名|作用
    |:---|:---|
    round|四舍五入
    ceil|向上取整
    floor|向下取整
    abs|取绝对值
    max|取最大值
    pow|Math.pow(a,b)，返回a的b次幂
    trunc|去掉一个数的小数部分，并返回整数部分，如Math.trunc(4.1)//4
    sign|参数伪正数,返回+1，参数为负数，返回-1；参数为0，返回0；参数伪-0，返回-0；其他值，返回NaN
    cbrt|返回数字的立方根
  - Object方法：
    方法名|作用
    |:---|:---|
    keys|获取该对象所有可遍历的key值数组
    hasOwnProperty|确定某个对象是否具有带指定名称的属性，该方法不会检查对象的原型链中的属性
    isPrototypeOf|确定一个对象是否存在于另一个对象的原型链中
    **ES6新增方法**|
    values|获取该对象所有可遍历的键值数组
    entries|获取该对象所有可遍历的键值对数组

## **正则表达式**

- 在JS中，正则表达式也是对象。这些模式被用于RegExp的exec和test方法，以及String的match、replace、search、和split方法。
- 创建一个正则表达式：其由包含在斜杠之间的模式组成

  ```js
  /*
  /pattern/flags
  */
  const regex = /ab+c/;
  const regex = /^[a-zA-Z]+[0-9]*\W?_$/gi;
  ```

  在加载脚本后，正则表达式字面值提供正则表达式的编译。当正则表达式保持不变时，使用此方法可获得更好的性能。
- 或者调用RegExp对象的构造函数，如下所示

  ```js
  /*
  new RegExp(pattern [,flags])
  */
  let regex = new RegExp("ab+c");
  let regex = new RegExp(/^[a-zA-Z]+[0-9]*\W?_$/,"gi");
  let regex = new RegExp("^[a-zA-Z]+[0-9]*\\W?_$","gi");
  ```

  字符|含义
  |:---|:---|
  **\\**|匹配依照下列规则：<br> a).**在非特殊字符前的反斜杠表示下一个字符是特殊的**，不能从字面上解释。eg. 'b'和'\b'。<br>b).**反斜杠也可以将其后的特殊字符转义为字面量**。eg. a*/表示匹配0个或多个a相反，模式/a\*/将‘*’的特殊性移除，从而匹配像'a*'这样的字符串。<br>注意：使用new RegExp("pattern")的时候要将\进行转义。
  **^**|a).**匹配输入的开始**。如果多行标志被设置为true，那么也匹配换行符后紧跟的位置eg./^A/不会匹配"an A"中的”A",但会匹配“An E”中的"A"。<br>b).**当 '^' 作为第一个字符出现在一个字符集合模式时，它将会有不同的含义。**
  **$**|**匹配输入的结束**。如果多行标示被设置为true，那么也匹配换行符前的位置。<br>eg. /t$/ 并不会匹配 "eater" 中的 't'，但是会匹配 "eat" 中的 't'。
  **\***|**匹配前一个表达式0次或多次,等价于 {0,}**。<br>eg. /bo*/会匹配 "A ghost boooooed" 中的 'booooo' 和 "A bird warbled" 中的 'b'，但是在 "A goat grunted" 中将不会匹配任何东西。
  **+**|**匹配前面一个表达式1次或者多次。等价于 {1,}。**<br>eg. /a+/匹配了在 "candy" 中的 'a'，和在 "caaaaaaandy" 中所有的 'a'。
  **?**|a).**匹配前面一个表达式0次或者1次。等价于 {0,1}。**<br>eg. /e?le?/ 匹配 "angel" 中的 'el'，和 "angle" 中的 'le' 以及"oslo' 中的'l'。<br>b).**如果紧跟在任何量词 *、 +、? 或 {} 的后面，将会使量词变为非贪婪的（匹配尽量少的字符）**，和缺省使用的贪婪模式（匹配尽可能多的字符）正好相反。<br>eg.对 "123abc" 应用 /\d+/ 将会返回 "123"，如果使用 /\d+?/,那么就只会匹配到 "1"。<br>c).**还可以运用于先行断言**，如本表的 x(?=y) 和 x(?!y) 条目中所述。
  **.**|（小数点）**匹配除换行符之外的任何单个字符**。<br>eg. /.n/将会匹配 "nay, an apple is on the tree" 中的 'an' 和 'on'，但是不会匹配 'nay'。
  **(X)**|**匹配 'x' 并且记住匹配项**，就像下面的例子展示的那样。括号被称为 **捕获括号**。
  (?:x)|**匹配 'x' 但是不记住匹配项。****这种叫作非捕获括号**，使得你能够定义为与正则表达式运算符一起使用的子表达式。
  x(?=y)|**匹配'x'仅仅当'x'后面跟着'y'.这种叫做正向肯定查找。**
  x(?\|y)|**匹配'x'仅仅当'x'后面不跟着'y',这个叫做正向否定查找。**
  x\|y|匹配‘x’或者‘y’。
  **{n}**|**n是一个正整数，匹配了前面一个字符刚好发生了n次**。<br>eg. /a{2}/不会匹配“candy”中的'a',但是会匹配“caandy”中所有的a，以及“caaandy”中的前两个'a'。
  **{n,m}**|**n 和 m 都是整数。匹配前面的字符至少n次，最多m次。如果 n 或者 m 的值是0， 这个值被忽略。**
  **[xyz]**|**一个字符集合。匹配方括号的中任意字符，包括转义序列。** 你可以使用破折号（-）来指定一个字符范围。对于点（.）和星号（*）这样的特殊符号在一个字符集中没有特殊的意义。他们不必进行转义，不过转义也是起作用的。<br>eg. [abcd] 和[a-d]是一样的。他们都匹配"brisket"中得‘b’,也都匹配“city”中的‘c’。/[a-z.]+/ 和/[\w.]+/都匹配“test.i.ng”中得所有字符。
  **[^xyz]**|**一个反向字符集。** 也就是说， 它**匹配任何没有包含在方括号中的字符**。你可以使用破折号（-）来指定一个字符范围。任何普通字符在这里都是起作用的。
  [\b]|匹配一个**退格**(U+0008)。（不要和\b混淆了。）
  \b|**匹配一个词的边界。** 一个词的边界就是一个词不被另外一个词跟随的位置或者不是另一个词汇字符前边的位置。注意，**一个匹配的词的边界并不包含在匹配的内容中。** 换句话说，**一个匹配的词的边界的内容的长度是0。**（不要和[\b]混淆了）<br>eg. /\bm/匹配“moon”中得‘m’；<br>eg. /oo\b/并不匹配"moon"中得'oo'，因为'oo'被一个词汇字符'n'紧跟着。<br>eg. /oon\b/匹配"moon"中得'oon'，因为'oon'是这个字符串的结束部分。这样他没有被一个词汇字符紧跟着。<br>eg. \w\b\w/将不能匹配任何字符串，因为一个单词中的字符永远也不可能被一个非词汇字符和一个词汇字符同时紧跟着。
  \B|**匹配一个非单词边界。** 他匹配一个**前后字符都是相同类型**的位置：都是单词或者都不是单词。一个字符串的开始和结尾都被认为是非单词。<br>eg. /\B../匹配"noonday"中得'oo', 而/y\B./匹配"possibly yesterday"中得’ye‘
  \cX|**当X是处于A到Z之间的字符的时候，匹配字符串中的一个控制符。** <br>eg./\cM/ 匹配字符串中的 control-M (U+000D)。
  **\d**|**匹配一个数字。** 等价于[0-9]。
  **\D**|**匹配一个非数字字符。** 等价于[^0-9]。
  \f|匹配一个换页符 (U+000C)。
  \n|匹配一个换行符 (U+000A)。
  \r|匹配一个回车符 (U+000D)。
  \s|匹配一个空白字符，包括空格、制表符、换页符和换行符。等价于[ \f\n\r\t\v\u00a0\u1680\u180e\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]。例如, /\s\w*/ 匹配"foo bar."中的' bar'。
  \S|匹配一个非空白字符。
  \t|匹配一个水平制表符 (U+0009)。
  \v|匹配一个垂直制表符 (U+000B)。
  **\w**|匹配一个单字字符（字母、数字或者下划线）。等价于[A-Za-z0-9_]。
  **\W**|匹配一个非单字字符。等价于[^A-Za-z0-9_]。
  **\n**|当 n 是一个正整数，一个返回引用到最后一个与有n插入的正则表达式(counting left parentheses)匹配的副字符串。比如 /apple(,)\sorange\1/ 匹配"apple, orange, cherry, peach."中的'apple, orange,' 。
  \0|匹配 NULL (U+0000) 字符， 不要在这后面跟其它小数，因为 \0\<digits> 是一个八进制转义序列。
  \xhh|与代码 hh 匹配字符（两个十六进制数字）
  \uhhhh|与代码 hhhh 匹配字符（四个十六进制数字）。
  \u{hhhh}|(仅当设置了u标志时) 使用Unicode值hhhh匹配字符 (十六进制数字).

- 使用正则表达式
    方法|描述
    |:---|:---|
    exec|一个在string中执行查找匹配的RegExp方法，它**返回一个数组（未匹配到则返回null）**。
    test|一个在string测试是否匹配的RegExp方法，它**返回true或false**。
    match|一个在string中执行查找匹配的String方法，它**返回一个数组或者在未匹配到时返回null**。
    search|一个在string中测试匹配的String方法，它**返回匹配到的位置索引，或者在失败时返回-1**。
    replace|一个在string中执行**查找匹配**的String方法，并且**使用替换字符串替换掉匹配到的子字符串**。
    split|一个使用正则表达式或者一个固定字符串分隔一个字符串，并**将分隔后的子字符串存储到数组中**的String方法。
- 正则表达式标志
  标志|描述
  |:---|:---|
  g|全局搜索
  i|不区分大小写搜索
  m|多行搜索
  y|执行”粘性“搜索，匹配从目标字符串的当位置开始，可以使用y标志
- [正则表达式参考(MDN)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Guide/Regular_Expressions)