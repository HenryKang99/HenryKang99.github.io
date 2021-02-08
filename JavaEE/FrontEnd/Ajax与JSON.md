### 0verview

　　**异步的 JavaScript 和 XML**（Asynchronous JavaScript and XML，AJAX），是一种在无需重新加载整个网页的情况下，能够更新部分网页的技术。减少了不必要的请求，提升了用户的体验。

　　**JavaScript对象表示法**（JavaScript Object Notation，JSON），是存储和交换文本信息的语法，类似 XML。早期就是JS用来表示对象的一种语法，由于JSON比XML更易传输和解析，后来使用的越来越广泛。回想JS创建对象的语法：

```js
var person={
	firstname : "k",
	lastname  : "hy",
	id        : 1234，
	talk : function(){ 
		return this.firstname+this.lastname+this.id;
	}
};
```

---

### 1. JQuery方式实现AJAX请求

> [菜鸟教程JQuery实现AJAX](https://www.runoob.com/jquery/jquery-ref-ajax.html)

- `$.ajax({键值对})`
  - url：请求路径；
  - type：请求方式；
  - dataType：响应结果的类型；
    - 如果不设置则浏览器会根据响应头的content-type自动设置；
  - data：请求参数，格式可以为键值对或 **JSON**；
  - success：响应后执行的函数，用于操作数据；
  - error：响应失败后执行的函数；
  - ……

```js
$.ajax({
    url:"ajaxServlet1111",
    type:"POST", 
    dataType:"text",
    //data: "username=hyk&age=20",
    data:{"username":"hyk","age":20},
    success:function (data) {
        alert(data);
    },
    error:function () {
        alert("error")
    }
});
```

- `$.get(url,data,callback,dataType)`
  - url：请求路径；
  - data：请求参数；
  - callback：回调函数；
  - dataType：响应结果的类型；

```js
$.get("ajaxServlet",{username:"rose"},function (data) {
	alert(data);
},"text");
```

- `$.post(url,data,callback,dataType)`

```js
$.post("ajaxServlet",{username:"rose"},function (data) {
	alert(data);
},"text");
```

　　

---

### 2. JSON

#### 2.1 语法

- **语法规则**

  数据以键值对保存，各个数据由逗号隔开，大括号保存对象，中括号保存数组。

  　　获取元素通过 `对象.key`、`对象["key"]`，数组对象通过下标获取 `数组[index]`。

  　　如下表示 sites 数组中有三个对象，对象的属性是name和url，sites本身是个对象所以也有大括号包裹。注意键值要加引号。

  ```js
  {
      "sites": [
      { "name":"菜鸟教程" , "url":"www.runoob.com" }, 
      { "name":"google" , "url":"www.google.com" }, 
      { "name":"微博" , "url":"www.weibo.com" }
      ]
  }
  // 在 JS 中等价于：
  var sites = [
      { "name":"runoob" , "url":"www.runoob.com" }, 
      { "name":"google" , "url":"www.google.com" }, 
      { "name":"微博" , "url":"www.weibo.com" }
  ];
  // 获取元素
  sites[0]["name"] = "菜鸟";
  sites[1].name = "谷歌";
  // 遍历打印对象
  // 使用let是因为for循环不是函数，使用var声明的都是全局变量
  for (let i = 0; i < sites.length; i++) {
  	let site = sites[i];
  	for(let key in site){
          // 注意这里不能使用site.key获取，因为key是一个字符串
  		document.write(key+":"+site[key]);
  		document.write("<br>");
  	}
  }
  ```

  

- **数据类型**

  可以是数字型（整数或浮点数）、字符串（在双引号中）、逻辑值（true 或 false）、数组（在中括号中）、对象（在大括号中）、null。

#### 2.2 与Java对象的转换

　　JavaScript 与 AJAX 天然的支持 JSON，而Java不支持，我们就需要对其进行转换。

- 使用Jackson解析器的步骤：

  - 导入jar包；

  - 创建Jackson的核心对象`ObjectMapper`；

  - 调用相关方法；

    - `om.readValue(jsonStr,Class)`：JSON → Java对象
    - `om.writeValue(parameter，obj)`：Java对象 → JSON
    - `om.writeValueAsString(obj)`：Java对象 → jsonStr

  - parameter：

    - Writer：将obj转换为JSON串并写入到字符输出流；

    - OutputStream：将obj转换为JSON串并写入到字节输出流；

    - File：将obj转换为JSON串并存储到指定文件；

      

