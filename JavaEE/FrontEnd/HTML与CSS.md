### 0verview

　　**超文本标记语言**（HyperText Markup Language，HTML），**标记**体现在通过一些标签将网络上的文本格式统一化，便于传播；**超文本**一方面体现在内容不限于文字还包括图片视频表格等，另一方面是通过超链接的方式将分布在互联网各个角落的资源连接成为一个整体，便于访问。

　　**CSS**（Cascading Style Sheets），层叠样式表。如果将HTML比作图书，那CSS就是书架，用于规划HTML的样式。虽然HTML也可以通过标签的一些属性设置样式，但是为了便于统一维护（解耦），通常只用HTML来体现内容，用CSS体现样式。

　　由于标记语言并不是编程语言，只有HTML与CSS制作的页面还欠缺与用户的动态交互性，所以后来就有了**JavaScript**。

### 1. HTML

[HTML菜鸟教程链接](https://www.runoob.com/html/html5-intro.html)

#### １.１ Hello HTML

```html
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title>Hello HTML</title>
	</head>
	<body>
		Hello HTML!
	</body>
</html>
```

- \<!DOCTYPE html> 声明此文档是html文档；
- \<html></html\> 称为根标签；
- \<head></head\> 称为头标签，用于指定文档的一些属性和引入外部资源；
- \<title></title\> 标题标签，指定标签页的名字；
- \<body> 称为体标签，用于定义我们的文档内容；



#### 1.2 文本标签

- 标签

| 标签             | 说明                     |
| ---------------- | ------------------------ |
| \<!--  -->       | 注释                     |
| \<h1> 到 \<h6>   | 标题标签，h1-h6 逐渐缩小 |
| \<p>             | 声明一个段落             |
| \<br/>           | 换行                     |
| \<hr/>           | 分割线                   |
| \<b>、\<i>、\<u> | 斜体粗体下划线           |
| \<font>          | 字体标签                 |
| \<center>        | 内容居中                 |
| ……               | ……                       |

- 说明：

  - 标签按是否自动换行可分为块级标签、行内标签；
  - 标签属性大小写都行，属性值用单引号双引号都行；

  

- 描述文本的属性

| 属性        | 说明                                              |
| ----------- | ------------------------------------------------- |
| color       | 颜色，表示为rgb(255,255,255) 或 color = "#FFFFFF" |
| width、size | 宽度、高度，默认单位是像素，可以使用百分比        |
| align       | 对齐方式                                          |
| ……          | ……                                                |



#### 1.3 图片、链接标签

**1. 图片标签** 

```html
<img src="./xxx.png" alt="美丽的图片" weight=" "  height=" ">
```

- 属性：

  - src 指定图片路径；
  - alt 指定图片加载失败显示的文字；
  - weight、height 指定宽高，注意与描述文本宽度的属性size区分；
  - ……

  

**2. 链接标签**

```html
<a href="https://www.henrykang.site" target="_self">链接\</a>
```

- 属性：
  - target：
    - _self 在本页打开
    - _blank 在新标签页打开



#### 1.4 列表、表格标签

**1. 无序列表**

```html
<ul type="circle">
	<li>张三</li>
	<li>李四</li>
</ul>
```

- 属性：
  - type：circle、square、disc(默认)

**2. 有序列表**

```html
<ol type="A" start="2">
	<li>张三</li>
	<li>李四</li>
</ol>
```

- 属性：
  - type：A、a、……
  - start：开始的编号；

**3. 自定义列表**

```html
<dl>
    <dt>张三</dt>
    <dd>- 100分</dd>
    <dt>李四</dt>
    <dd>- 99分</dd>
</dl>
```



**4. 表格**

```html
<table border="2" weight="100" height="100" align="center" cellpadding="10" cellspacing="0">
    <caption>表格名称</caption>
    <tr>
    	<th align="center">标签</th>
    	<th colspan="2" align="center">属性 || 说明</th>
    </tr>
    <tr>
        <td rowspan="6">table</td>
        <td>border</td>
        <td>边界</td>
    </tr>
    <tr>
        <td>width、height</td>
        <td>宽高</td>
    </tr>
    <tr>
        <td>bgcolor</td>
        <td>颜色</td>
    </tr>
    <tr>
        <td>align</td>
        <td>对齐方式</td>
    </tr>
    <tr>
        <td>cellpandding</td>
        <td>表格内边距</td>
    </tr>
    <tr>
        <td>cellspacing</td>
        <td>表格外边距</td>
    </tr>
    <tr>
        <td>caption</td>
        <td colspan="2">声明表格标题</td>
    </tr>
    <tr>
        <td>th</td>
        <td colspan="2">声明表头</td>
    </tr>
    <tr>
        <td>tr</td>
        <td colspan="2">声明行</td>
    </tr>
    <tr>
        <td>td</td>
        <td colspan="2">声明列（单元格）</td>
    </tr>
    <tr>
        <td>rowspan</td>
        <td colspan="2">合并行</td>
    </tr>
    <tr>
        <td>colspan</td>
        <td colspan="2">合并列</td>
    </tr>
</table>
```

- **效果如下：**

<table border="2" weight="100" height="100" align="center" cellpadding="10" cellspacing="0">
<caption>表格名称</caption>
<tr>
<th align="center">标签</th>
<th colspan="2" align="center">属性 || 说明</th>
</tr>
<tr>
<td rowspan="6">table</td>
<td>border</td>
<td>边界</td>
</tr>
<tr>
<td>width、height</td>
<td>宽高</td>
</tr>
<tr>
<td>bgcolor</td>
<td>颜色</td>
</tr>
<tr>
<td>align</td>
<td>对齐方式</td>
</tr>
<tr>
<td>cellpandding</td>
<td>表格内边距</td>
</tr>
<tr>
<td>cellspacing</td>
<td>表格外边距</td>
</tr>
<tr>
<td>caption</td>
<td colspan="2">声明表格标题</td>
</tr>
<tr>
<td>th</td>
<td colspan="2">声明表头</td>
</tr>
<tr>
<td>tr</td>
<td colspan="2">声明行</td>
</tr>
<tr>
<td>td</td>
<td colspan="2">声明列（单元格）</td>
</tr>
<tr>
<td>rowspan</td>
<td colspan="2">合并行</td>
</tr>
<tr>
<td>colspan</td>
<td colspan="2">合并列</td>
</tr>
</table>



#### 1.5 表单标签

> 表单通常用于提交数据，要提交的数据必须有name属性，因为name将作为键值对的键。

```html
<form action="" method="get">
    <table>
        <tr>
            <td><label for="username">用户名</label></td>
            <td><input type="text" name="username" id="username" placeholder="请输入用户名" /></td>
        </tr>
        <tr>
            <td><label for="pwd">密码</label></td>
            <td><input type="password" name="pwd" id="pwd" placeholder="请输入密码"></td>
        </tr>
        <tr>
            <td>邮箱</td>
            <td><input type="email" name="email" placeholder="请输入邮箱"></td>
        </tr>
        <tr>
            <td>性别</td>
            <td>
                男<input type="radio" name="gender" value="male" checked>
                女<input type="radio" name="gender" value="female">
            </td>
        </tr>
        <tr>
            <td>简介</td>
            <td>
                <textarea rows="20" cols="24" name="introduction">

                </textarea>
            </td>
        </tr>
        <tr>
            <td>爱好</td>
            <td>
                唱<input type="checkbox" name="hobby" value="sing" />
                跳<input type="checkbox" name="hobby" value="dance" />
                rap<input type="checkbox" name="hobby" value="rap" />
                篮球<input type="checkbox" name="hobby" value="basketball" />
            </td>
        </tr>

        <tr>
            <td>地址</td>
            <td>
                <select name="addr">
                    <option value="null" checked>--请选择--</option>
                    <option value="beijing">北京</option>
                    <option value="shanghai">上海</option>
                    <option value="guangzhou">广州</option>
                </select>
            </td>
        </tr>
        <tr>
            <td>出生日期</td>
            <td><input type="date" name="birth"></td>
        </tr>
        <tr>
            <td>验证码</td>
            <td>
                <input type="text" name="verifycode" placeholder="请输入验证码">
                <img src="img/xxx">
            </td>

        </tr>
        <tr>
            <td colspan="2" align="center"><input type="submit" value="注册"></td>
        </tr>
    </table>
</form>
```

- **效果如下：**

<form action="" method="get">
<table>
<tr>
<td><label for="username">用户名</label></td>
<td><input type="text" name="username" id="username" placeholder="请输入用户名" /></td>
</tr>
<tr>
<td><label for="pwd">密码</label></td>
<td><input type="password" name="pwd" id="pwd" placeholder="请输入密码"></td>
</tr>
<tr>
<td>邮箱</td>
<td><input type="email" name="email" placeholder="请输入邮箱"></td>
</tr>
<tr>
<td>性别</td>
<td>
男<input type="radio" name="gender" value="male" checked>
女<input type="radio" name="gender" value="female">
</td>
</tr>
<tr>
<td>简介</td>
<td>
<textarea rows="20" cols="24" name="introduction">
</textarea>
</td>
</tr>
<tr>
<td>爱好</td>
<td>
唱<input type="checkbox" name="hobby" value="sing" />
跳<input type="checkbox" name="hobby" value="dance" />
rap<input type="checkbox" name="hobby" value="rap" />
篮球<input type="checkbox" name="hobby" value="basketball" />
</td>
</tr>
<tr>
<td>地址</td>
<td>
<select name="addr">
<option value="null" checked>--请选择--</option>
<option value="beijing">北京</option>
<option value="shanghai">上海</option>
<option value="guangzhou">广州</option>
</select>
</td>
</tr>
<tr>
<td>出生日期</td>
<td><input type="date" name="birth"></td>
</tr>
<tr>
<td>验证码</td>
<td>
<input type="text" name="verifycode" placeholder="请输入验证码">
<img src="img/xxx">
</td>
</tr>
<tr>
<td colspan="2" align="center"><input type="submit" value="注册"></td>
</tr>
</table>
</form>



**说明：**

- \<form> 声明表单范围：
  - action 指定提交的url；
  - method 指定提交的方式；
- \<input> 声明输入：
  - type 指定输入样式：
    - text 文本输入框
    - password 密码输入框
    - radio 单选框，name属性一致可保证单选
    - checkbox 复选框，每一项都要指定 value 作为选中时提交的值
    - file 文件选择框
    - submit 提交按钮
    - image 提交图片按钮
    - button 普通按钮
- \<textarea>：文本域
  - rows 指定行数
  - cols 指定列数
- \<label> 声明一个标签：
  - for 属性与 input 的 id 属性保持一致，可以点击 label 使 input 获得焦点，提升用户体验；

- \<select> 下拉列表：
  - option 指定下拉列表的每一项；
- ……



---

### 2. CSS

[CSS菜鸟教程链接](https://www.runoob.com/css/css-tutorial.html)

#### 2.1 HTML 结合 CSS 的三种方式

1. 内联样式

   在标签内使用`style`声明css代码，多个属性用分号隔开；

   ```html
   <font style="color: red;">Hello CSS!</font>
   ```

2. 与HTML写在同一个文件

   在 \<head> 标签内定义 \<style> 标签，书写css样式，分号隔开；

   ```html
   <head>
       <meta charset="utf-8">
       <title>Hello CSS</title>
       <style>
           font{
               color: blue;
           }
       </style>
   </head>
   <body>
       <font>Hello CSS!</font>
   </body>
   ```

3. 外部引入

   在 \<head> 标签内定义\<link>标签引入外部css文件，此时外部引入的文件内部不需要写style标签；

   ```html
   <link rel="stylesheet" href="css/hello_css.css">
   ```



#### 2.2 常用选择器

- id选择器

  ```css
  #id{}
  ```

- 类选择器

  ```css
  .className{}
  ```

- 元素选择器

  ```css
  element{}
  ```

- 并集元素选择器

  ```css
  element1,element2 ...{}
  ```

- 所有元素选择器

  ```css
  *{}
  ```

- 子类选择器

  ```css
  element element{}
  ```

- 父类选择器

  ```css
  element > element{}
  ```

- [详见此处](https://www.runoob.com/cssref/css-selectors.html)



#### 2.3 盒子模型

- 布局问题，注意找准参考系：
  - margin 外边距
  - padding 内填充
    - 会将盒子“撑开”
    - 使用`box-sizing: border-box` 设置盒子的属性，让盒子固定为设置的宽高；
- 浮动问题：
  - \<div>是块级元素会自动换行，可以通过 float 属性设置左右浮动，从而不换行；

