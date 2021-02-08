---
title: JQuery
toc: true
share: true
donate: true
comment: true
tags: [前端]
categories: [JavaEE,Front End]
date: 2020-03-08
description:
top_img: false
cover: https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/PBjb02uvI67s.png
---

### 0verview

　　JQuery 是一个 JavaScript 框架，宗旨是“Write Less，Do More”，它优化了DOM操作、事件处理、Ajax、动画等操作。

　　回想 JS 的DOM操作：

```js
// 获取元素
getElementById()
getElementsByTagName()
getElementsByClassName()
getElementsByName()
...
// 设置属性
document.getElementById("p1").innerHTML="文本";
document.getElementById('id1').style.color='red'
...
// 绑定事件
window.onload = function(){...}
...
```

　　显然使用原生的 JS 进行 DOM 操作有点繁琐，操作层级元素更是不太直观。JQuery就是封装了这些操作，为我们提供了极大的便利，使用`$`符号就可以方便的获取对象、操作属性：

```js
// 元素获取与属性设置
$("#div1").html("HTML内容");
$("p").css({"background-color":"red","font-size":"20px"});

// 入口函数（可以定义多次，区别于window.onload()定义多次会覆盖）
$(document).ready(function(){});
// 等价于下面的简写形式
$(function () {...});

// 事件绑定
$("#b1").click(function(){
    alert("事件绑定");
});
```

<!-- more -->

　　

---

### 1. JQuery对象

#### 1.1 使用选择器获取JQ对象

　　JQuery让我们可以像使用CSS的选择器一样方便的获取对象，只需要在`${}`中传入一个选择器即可。
　　[菜鸟教程JQ选择器](https://www.runoob.com/jquery/jquery-ref-selectors.html)

| 选择器                                                       | 说明                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| $("#id")                                                     | id选择器                                                     |
| $(".class")                                                  | 类选择器                                                     |
| $("element")                                                 | 元素选择器                                                   |
| $("选择器1,选择器2....")                                     | 并集选择器，逗号隔开                                         |
| $("Father Son")                                              | 后代选择器，选取Father所有的Son子孙                          |
| $("F > S")                                                   | 儿子选择器，只选取Father的Son儿子                            |
| $("[属性名]")                                                | 属性选择器，按名称                                           |
| $("[属性名='值']")<br />$("div[id!='123']")<br />$("div[id^='a']")<br />$("div[id$='z']")<br />$("div[id*='abc']") | 属性选择器，按属性值<br />选择div中不包含id=123的属性，包括没有id属性的<br />选择以 a 开头的<br />选择以 z 结尾的<br />选择包含 abc 的 |
| $("\[属性名='值'][属性名='值']...")                          | 属性选择器，取交集                                           |
| ---过滤选择器---                                             | ---过滤选择器---                                             |
| :first                                                       | 获取选择的第一个元素                                         |
| :last                                                        | 获取选择的最后一个元素                                       |
| :not(选择器)                                                 | 不包括指定内容的元素                                         |
| :even                                                        | 偶数选择器，从0开始                                          |
| :odd                                                         | 奇数选择器，从0开始                                          |
| :eq(index)                                                   | 等于指定索引的元素                                           |
| :gt(index)                                                   | 大于指定索引的元素                                           |
| :lt(index)                                                   | 小于指定索引的元素                                           |
| :header                                                      | 标题选择器                                                   |
| :enabled                                                     | 可用元素                                                     |
| :disabled                                                    | 不可用元素                                                   |
| :checked                                                     | 单选框、复选框选中元素                                       |
| :selected                                                    | 下拉框选中元素，<br />下拉框配置multiple属性可多选           |

#### 1.2 JQ对象与JS对象的转换

　　使用`$`获取的对象和使用JS的DOM操作获取的对象含义相同，但是本质上还是有所区别，它们的方法和属性都不通用，所以我们有时需要对二者进行转换：

- JS 对象 → JQ对象
  - $(JS对象)

- JQ 对象 → JS对象
  - JQ对象[index] 
  - JQ对象.get(index)
- JQ对象可以看做是对JS对象的一层封装。

　　

---

### 2. DOM操作

#### 2.1 内容操作

- `text()`：设置或返回所选元素的文本内容；

  ```html
  <p id="test"><span>这是<b>Test</b>文本</span></p>
  <!--获取："这是Test文本" -->
  $("#test").text()
  <!--设置：将<p>标签清空，包括里面的标签，设置内容为"text" -->
  $("#test").text("text")
  ```

- `html()`：设置或返回所选元素的内容（包括 HTML 标记）；

- `val()`：设置或返回value属性的值；

#### 2.2 属性操作

- **获取属性**
  - `prop()`：HTML固有的属性使用此方法；没有找到属性会返回 “ ”（空字符串）；
  - `attr()`：自定义的DOM属性使用此方法，没有找到属性会返回undefined；
- **删除属性**
  - `removeProp()`
  - `removeAttr()`
- **class属性操作**
  - `addClass()`：添加class属性值；
  - `removeClass()`：删除class属性值；
  - `toggleClass()`：切换class属性，若有则添加，若没有则删除；

#### 2.3 元素节点的增删操作

- **删除**

  - `元素.move()`：删除该元素及其子元素；
  - `元素.empty()`：删除该元素的子元素；

- **新增**

  第一类：**在元素内部加入**

  - `元素A.append(元素B)`：在元素A结尾插入元素B；
  - `元素A.prepend(元素B)`： 在元素Ａ开头插入元素Ｂ；

  ```html
  <p>
    <span class="s1">s1</span>
  </p>
  $("p").append('<span class="s2">s2</span>');
  <!--执行结果 -->
  <p>
    <span class="s1">s1</span>
    <span class="s2">s2</span>
  </p>
  ```

  第二类：**在元素外面加入**

  - `A.after(B)`：在A元素之后插入B元素
  - `A.before(B)`：在A元素之前插入B元素

  ```html
  <p>
    <span class="s1">s1</span>
  </p>
  $("p").after('<span class="s2">s2</span>');
  <!--执行结果 -->
  <p>
    <span class="s1">s1</span>
  </p>
  <span class="s2">s2</span>
  ```

　　

#### 2.4 JQ 的遍历

- **JS 遍历**

  - for 循环
  - for in 循环

  ```js
  var x;
  var txt="";
  var person={fname:"Zhang",lname:"San",age:20}; 
  for (x in person){
      txt=txt + person[x];
  }
  ```

- **JQ 遍历**

  - `元素.each(callback)`  或 `$.each(元素, callback)`： 为每个匹配元素执行函数；

  ```js
  $(选择器).each(function(index,element){
      return true; // =continue
      return false;// =break
  });
  $.each(元素, function(index,element){})
  /*
  	index:元素在集合中的索引
  	element：集合中的每一个元素对象
  	this：= element
  */
  ```

  - `for...of(元素 of 容器)`（JQuery3.0以上）

　　

#### 2.5 事件

- **常用事件**

| 鼠标事件                                                     | 键盘事件                                                     | 表单事件                                                  | 文档/窗口事件                                             |
| :----------------------------------------------------------- | :----------------------------------------------------------- | :-------------------------------------------------------- | :-------------------------------------------------------- |
| [click](https://www.runoob.com/jquery/event-click.html)      | [keypress](https://www.runoob.com/jquery/event-keypress.html) | [submit](https://www.runoob.com/jquery/event-submit.html) | [load](https://www.runoob.com/jquery/event-load.html)     |
| [dblclick](https://www.runoob.com/jquery/event-dblclick.html) | [keydown](https://www.runoob.com/jquery/event-keydown.html)  | [change](https://www.runoob.com/jquery/event-change.html) | [resize](https://www.runoob.com/jquery/event-resize.html) |
| [mouseenter](https://www.runoob.com/jquery/event-mouseenter.html) | [keyup](https://www.runoob.com/jquery/event-keyup.html)      | [focus](https://www.runoob.com/jquery/event-focus.html)   | [scroll](https://www.runoob.com/jquery/event-scroll.html) |
| [mouseleave](https://www.runoob.com/jquery/event-mouseleave.html) |                                                              | [blur](https://www.runoob.com/jquery/event-blur.html)     | [unload](https://www.runoob.com/jquery/event-unload.html) |
| [hover](https://www.runoob.com/jquery/event-hover.html)      |                                                              |                                                           |                                                           |

- **绑定事件**
  - `元素.事件名(回调函数)`
  - `元素.("事件名",回调函数)`
- **解除事件**
  - `元素.off("事件名")`：解除该元素的该事件；
    - 参数为空，则解除该元素的所有事件；
- **切换事件**
  - `元素.toggle(fn1,fn2...)`：事件每发生一次，执行方法列表的下一个方法；
    - JQuery1.9将此方法删除，需要引入`JQuery Migrate `插件方能使用。

　　

---

### 3. 动画

- **显示/隐藏/切换**
  - `show(speed,easing,callback)`
    - speed：毫秒值表示动画完成的时间
      - 默认值：slow，nomal，fast
    - easing：动画的效果
      - swing：平滑
      - linear：匀速
    - callback：动画完成后执行的函数
  - `hide(...)`
  - `toggle(...)`
- **淡入淡出**
  - `fadeIn(...)`
  - `fadeOut(...)`
  - `fadeToggle(...)`
  - `fadeTo(spead,opacity,callback)`：渐变为给定的不透明度
    - opacity ∈ [0,1]；
- **滑动**
  - `fadeIn(...)`
  - `fadeOut(...)`
  - `fadeToggle(...)`