### 0verview

　　HTML 定义了网页的**内容**，CSS 规划了内容的**布局**，而 JavaScript 体现了网页的**行为**；

　　JS 最早是为了用于表单校验，减轻服务器压力，而后来发展成了一门独立的客户端脚本语言。JavaScript 在1995年由 Netscape 公司的 Brendan Eich 开发，最初名叫 Mocha，后与 SUN 公司合作改名为 JavaScript， 1996 年Netscape 将 JS 提交到 ECMA（欧洲计算机制造商协会） 组织进行标准化，于1997 年被采纳，制定了 ECMAScript 标准，统一了客户端脚本语言。

　　标准包含的内容范围较小，而 JavaScript 等是对 ECMAScript 标准的实现，除了包含 ECMAScript 还包含了其他功能。就好比 SQL 标准与MySQL、Oracle等实现的关系，因此我们主要学习的是它的标准与一些特有功能，学会了标准再学习其他实现就会比较容易。　

---

### 1. ECMAScript

#### 1.1 语法

##### 1. 与html的结合方式

- 使用`<script></script>`包裹，可以放在 body 和 head 中的任何地方，通常将函数放在 head 或者 body 的尾部，因为放的位置会影响执行顺序，我们想要的是页面资源加载完成之后再使用JS；
- 外部引入方式`<script src="myScript.js"></script>`；

<br>

##### 2. 输出语句

| 方法             | 说明               |
| ---------------- | ------------------ |
| window.alert()   | 弹框               |
| document.write() | 将内容写入到html   |
| innerHTML        | 将内容写入html元素 |
| console.log()    | 写出到浏览器控制台 |

　　**注意：**若在页面加载完成后（例如函数中）使用 `document.write()`，会覆盖此页面。

<br>

##### 3. 变量与数据类型

- **关于变量：**

  - JS 是弱类型语言，声明变量可以使用使用`var`、`const`、`let`（ES6），区别如下：

    - var 可以**变量提升**（可以先使用后声明），const和let不行；
    - var 声明的全局变量属于 window 对象，可以通过 window.变量名 调用，let、const 不行；
    - var 可以在作用域内随时随地重新声明已声明的变量，且覆盖已声明的变量；
    - const 和 let 声明的变量都是作用域都是块级，在相同块内不能重新声明，在不同块中可以重新声明，但不会覆盖；
    - const 类似于 Java 中的 final，声明时必须初始化，指向引用的值，引用的值不可变，引用的内容可以变；

    ```js
    // var 可以随时重新声明已声明的变量，且覆盖
    // 当然在函数体内声明得变量属于局部变量，和当前正在讨论的无关，注意区别
    var i = 0;
    {
    	var i =1 ;
    	document.write("块内"+i+"<br>"); // 1
    }
    document.write("块外"+i); // 1
    //-----------------------------------------
    let i = 0;
    {
    	i =1 ;
    	document.write("块内"+i+"<br>"); // 1
    }
    document.write("块外"+i); // 1
    // -----------------------------------------
    let i = 0;
    {
    	let i =1 ;
    	document.write("块内"+i+"<br>"); // 1
    }
    document.write("块外"+i); // 0
    ```

    

  - 变量名可以以字母、`$`、`_`开始，且对大小写敏感；

  - 字符串使用单引号或双引号包裹；

- **关于数据类型：**

  - 基本类型：String、Number、Boolean、Null、Undefined、symbol（ES6）；
  - 引用数据类型：Object、Array、Function（方法也是一个对象）；

  ```js
  // typeof 运算符，用于判断对象时什么类型，注意下面不会得到我们想要的结果：
  // 所以，我们常用instanceof方法来判断；
  typeof 数组 		//object
  typeof null		 //object
  typeof NaN		 //number
  typeof undefined // undefined
  ```

  **注意：**

  - 只声明未赋值时，为 undefined 状态；
  - Boolean 只有true和false；
  - null和undefined的区别：可以理解为 null 也是一个值，只不过代表此处没有对象，而undefined则是缺少值，即此处什么都没有。还有就是下面会提到的与namber运算时的转换，null=0，undefined=NaN；

<br>

##### 4. 运算符

- 赋值、加、减、乘、除、自增、自减……和 Java 类似；
  - 有字符串参与运算都转字符串；
    - boolean、NaN、null 和 undefined 转相应的字面量；
  - 无字符串参与运算都转number；
    - true 等于 1，false、null 等于 0；
    - undefined等于NaN，运算结果也是NaN；
- 比较运算符：>、 <、 >=、 <=、 ==、 ===、!=、!==
  - 对于对象比较的都是地址；
  - 类型相同则按顺序比较，否则先转化类型再比较；
    - null==undefined 与 null==null 返回true；
    - NaN六亲不认，和自己比都返回false；
  - `===` 与`!==`先判断类型，类型不同直接false/true；
    - null === undefined，返回false
- 逻辑运算符：&& || !
  - 运算时都转 boolean
    - 0、NaN、空字符串`“ ”`、null、undefined 都为 false；
    - 所有对象都为 true；

<br>

##### 5. 流程控制

- if-else、switch、while、do while、for 与java大同小异；

- `for in` 用于遍历对象和数组，相当于forEach；

  ```js
  var x;
  var txt="";
  var person={fname:"Zhang",lname:"San",age:20}; 
  for (x in person){
      txt=txt + person[x];
  }
  ```

<br>

##### 6. 函数

- 创建函数的方式

```js
function 函数名(var1,var2...){
    // 函数体
    // return xxx;
    // return后面的代码不会执行；
}
var 函数名 = function(var1,var2...){
    //函数体
}
```

- 注意：

  - 函数实际是一个**Function**对象，重名方法会被覆盖重写，**没有重载**一说；
  - 每个函数内都内置了一个**arguments**数组对象，封装了传进来的参数；
  - 方法的定义不用写参数类型也不用写返回类型；

  - 声明在函数内的是**局部变量**，在函数运行以后被删除；
  - 声明在函数外的是**全局变量**，在页面关闭后被删除；

- 函数 VS 方法：方法特指我们自己创建的对象中的函数，通过对象.方法名调用（函数其实也是window对象的方法），看下面对象的创建就理解了；

#### 1.2 对象

　　JS 中一切都是对象，数字型、字符串、布尔、数组、函数……都是对象；

##### 1. 对象的创建

```js
// 1. 直接创建，键值对的形式
var person={
	firstname : "k",
	lastname  : "hy",
	id        : 1234，
	talk : function(){ 
		return this.firstname+this.lastname+this.id;
	}
};
// 调用属性的两种方式
name=person.lastname;
name=person["lastname"];
// 调用方法要加括号
name=person.talk(); 
name=person["talk"]();

// 构造方法创建
function Person(firstname,lastname,id)
{
    this.firstname=firstname;
    this.lastname=lastname;
    this.id=id;
	
	this.getId = function getId(){
		return this.id;
	}
}
var p1 = new Person("Li","Si",2345);
```

- **关于 this：**this写在谁的作用域里面就指代谁；

  - 对象方法中，this 指向调用它的对象；
  - 外部使用时，this 指向全局对象（Global，浏览器中就是window对象）；
  - 函数中，this 指向函数的所属者（浏览器就是window对象）；
    - 严格模式下 this 是 undefined；
  - 在 HTML 事件中，this 指向了发出事件的 HTML 元素（事件源）；

  ```js
  //------对象与外部使用this--------
  var obj = {
    fun: function () { console.log(this.bar) },
    value: 1
  };
  var fun = obj.fun;
  var value = 2;
  // this 指对象obj
  obj.fun() // 1
  // this 指全局对象
  fun() // 2
  //------事件中的this------
  <input type="button" id="我是button1" onclick="alert(this.id)" value="按我测试"/>
  <input type="button" id="我是button2" onclick="alert(this.id)" value="按我测试"/>
  ```

<br>

##### 2. 基本对象

[w3cschool](https://www.w3cschool.cn/jsref/jsref-obj-date.html)

- **Array** 数组对象：

  - JS 中数组的元素和类型都是可变的；
  - 创建数组的方式：
    - `var arr = new Array(元素列表)`;
    - `var arr = new Array(默认长度)`;
    - `var arr = [元素列表]`;
  - 方法：
    - toString( )，将数组转化为字符串；
    - join(分隔符)，将数组中的元素按分隔符（默认为逗号）拼接为字符串；
    - push(元素)，向数组的末尾添加一个或更多元素，返回新的长度；
    - pop()，返回并删除最后一个元素；
    - shift()，返回并删除第一个元素；
    - ……

- **Date** 日期对象：

  - 创建：

    ```js
    new Date() // 当前日期和时间        
    new Date(milliseconds) //返回从 1970 年 1 月 1 日至今的毫秒数        
    new Date(dateString)        
    new Date(year, month, day, hours, minutes, seconds, milliseconds)
    ```

  - 方法：

    - getFullYear() 获取年份；
    - getTime() 返回毫秒数；
    - toLocalString() 返回当地格式的时间字符串；
    - ……

  - 实例：显示时钟

    ```html
    <head>
    <script>
    function startTime(){
    	var today=new Date();
    	var h=today.getHours();
    	var m=today.getMinutes();
    	var s=today.getSeconds();// 在小于10的数字钱前加一个‘0’
    	m=checkTime(m);
    	s=checkTime(s);
    	document.getElementById('txt').innerHTML=h+":"+m+":"+s;
    	t=setTimeout(function(){startTime()},500);
    }
    function checkTime(i){
    	if (i<10){
    		i="0" + i;
    	}
    	return i;
    }
    </script>
    </head>
    <body onload="startTime()">
    	
    <div id="txt"></div>
    	
    </body>
    ```

- **Math** 对象：

  类似Java中的一个静态工具类；

  - 方法：通过 `Math.方法名` 调用
    - abs(x) 绝对值
    - pow(x,y)  求 X<sup>y</sup>
    - random( ) 返回 [0,1) 的随机数
    - max()、min() 求最值
    - round(x) 四舍五入
    - ……

- **RegExp** 对象：

  - 创建：

    ```js
    var reg = new RegExp("\\w+");
    var reg = /\w+/;
    var name = "zhangsan";
    var flag = reg.test(name);
    ```

  - 规则：

    - `[]`表示单个字符，例如 [a]、[a-z]
    - `\d` 表示单个数字；
      - `\w` 表示单个字符；
    - 起止：
      - `^`表示以某个字符开始；
      - `$` 表示以某个字符结束；
    - 量词：
      - `?` 表示0或1次；
      - `*`表示0或多次；
      - `+`表示至少1次；
      - `{a,b}` 表示[a,b]次；

- **Global** 对象：

  - 方法：

| 函数                 | 说明                                                         |
| :------------------- | :----------------------------------------------------------- |
| decodeURI()          | 解码URI为字符串                                              |
| decodeURIComponent() | 解码范围更广的URI为字符串                                    |
| encodeURI()          | 把字符串编码为 URI                                           |
| encodeURIComponent() | 把范围更广的字符串编码为 URI                                 |
| escape()             | 对字符串进行编码                                             |
| unescape()           | 对由 escape() 编码的字符串进行解码                           |
| eval()               | 计算 JavaScript 字符串，并把它作为脚本代码来执行             |
| isFinite()           | 检查某个值是否为有穷大的数                                   |
| isNaN()              | 检查某个值是否为NaN                                          |
| Number()             | 把对象的值转换为数字                                         |
| parseFloat()         | 逐一解析一个字符串并返回一个浮点数直到不是数字停止           |
| parseInt()           | 逐一解析一个字符串中的字符直到不是数字停止<br />开头就是非数字字符则转为NaN |
| String()             | 把对象的值转换为字符串                                       |



----

### 2. BOM

　　BOM（Browser Object Model），浏览器对象模型。将浏览器的各个部分封装成为对象，拥有一些属性和方法，便于我们与浏览器“交流”；

主要包含下面几个对象：

| 对象       | 说明                                          |
| ---------- | --------------------------------------------- |
| Navigator  | 浏览器对象，包含有关浏览器的信息              |
| Screen     | 显示器对象，包含有关用户屏幕的信息            |
| **Window** | 窗口对象，可以用来获取其他对象（包括DOM对象） |
| History    | 历史记录对象，包含用户在当前窗口访问过的 URL  |
| Location   | 地址栏对象，包含有关当前 URL 的信息           |

<br>

#### 2.1 window 对象

- 常用属性（获取其他对象）

| 属性      | 说明             |
| --------- | ---------------- |
| navigator | 获取浏览器对象   |
| screen    | 获取屏幕对象     |
| history   | 获取历史记录对象 |
| location  | 获取地址栏对象   |
| document  | 获取dom对象      |

- 常用方法：

| 方法                         | 描述                                             |
| :--------------------------- | :----------------------------------------------- |
| alert()                      | 显示带有一段消息和一个确认按钮的警告框           |
| confirm()                    | 显示带有一段消息以及确认按钮和取消按钮的对话框   |
| prompt()                     | 显示可提示用户输入的对话框                       |
| open()                       | 打开一个新的浏览器窗口，可以传一个url            |
| close()                      | 关闭当前浏览器窗口，谁调用关谁                   |
| setInterval(方法str，毫秒值) | 按照指定的周期（以毫秒计）来调用函数或计算表达式 |
| setTimeout(方法str，毫秒值)  | 在指定的毫秒数后调用函数或计算表达式             |
| clearInterval()              | 取消由 setInterval() 设置的 timeout              |
| clearTimeout()               | 取消由 setTimeout() 方法设置的 timeout           |

#### 2.2 history 对象

- 常用属性：

| 属性   | 说明                           |
| :----- | :----------------------------- |
| length | 返回当前窗口历史列表中的网址数 |

- 常用方法：

| 方法      | 说明                            |
| :-------- | :------------------------------ |
| back()    | 加载 history 列表中的前一个 URL |
| forward() | 加载 history 列表中的下一个 URL |
| go()      | 前进后退具体个数的历史页面      |

#### 2.3 location 对象

- 常用属性：

| 属性     | 描述                          |
| :------- | :---------------------------- |
| hash     | 返回一个URL的锚部分           |
| host     | 返回一个URL的主机名和端口     |
| hostname | 返回URL的主机名               |
| href     | 返回完整的URL                 |
| pathname | 返回URL路径名                 |
| port     | 返回一个URL服务器使用的端口号 |
| protocol | 返回一个URL协议               |
| search   | 返回一个URL的查询部分         |

- 常用方法：

| 方法      | 说明                                     |
| :-------- | :--------------------------------------- |
| assign()  | 载入一个新的文档，可以后退到上一页       |
| reload()  | 重新载入当前文档                         |
| replace() | 用新的文档替换当前文档，不能后退到上一页 |



----

### 3. DOM

#### 3.1 概念

![DOM数](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/i4wjO7c4hlUL.png)

　　DOM（Document Object Model），文档对象模型。将文档的各个部分封装成为对象，使用这些对象可以动态的对文档的元素、属性、样式等进行操作；

　　DOM 包含document对象、element对象、attribute对象、text对象……这些对象都被看做是一个个节点，父子关系就对应着html文件中的标签及属性，例如 tr 是 td 的父节点，href 是 a 标签的一个子节点。

　　可以通过`appendChild()`来添加节点，`removeChild()`来删除节点，属性`parentNode`来获取父节点；

- element（标签）对象的方法：

| 方法                     | 说明                     |
| ------------------------ | ------------------------ |
| getElementById()         | 根据 id 属性获取，要唯一 |
| getElementsByTagName()   | 根据元素（标签）名获取   |
| getElementsByClassName() | 根据 class 属性获取      |
| getElementsByName()      | 根据 name 属性获取       |
| setAttribute()           | 设置属性                 |
| removeAttribute()        | 删除属性                 |

```js
var x = document.getElementById("test01");
```

- 创建节点对象的方法：

| 方法                  | 说明             |
| --------------------- | ---------------- |
| createElement()       | 创建一个标签对象 |
| createAttribute(name) | 创建一个属性对象 |
| createTextNode()      | 创建一个文本对象 |

- DOM HTML 与 DOM CSS 属性：

  - innerHTML 属性

  ```js
  document.getElementById("p1").innerHTML="文本";
  ```

  - style 属性

  ```js
  document.getElementById('id1').style.color='red'
  ```

　　

#### 3.2 DOM 事件

> [菜鸟教程](https://www.runoob.com/jsref/dom-obj-event.html)

- 绑定事件的两种方法举例：

```html
window.onload = function(){...}
```

```html
<button onclick="getElementById('demo').innerHTML=Date()">现在的时间是?</button>
```

