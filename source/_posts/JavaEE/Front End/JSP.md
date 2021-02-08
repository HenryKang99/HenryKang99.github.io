---
title: JSP
toc: true
share: true
donate: true
comment: true
tags: [前端]
categories: [JavaEE,Front End]
date: 2020-03-20
description:
top_img: false
cover: https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/2Dlr550xolFr.jpg
---

### 0verview

　　Java服务器端页面（JSP，Java Server Pages），是一种以 Java 作为脚本语言的运行在服务端的语言，为程序员响应用户请求而动态生成页面提供了便利。它在本质上是一个Servlet，当浏览器发起对JSP的请求时，服务器找到相应的JSP资源转换输出为Servlet，再编译成为class字节码文件加载进 JVM，创建servlet对象，调用service方法。

　　验证：Tomcat自带的启动默认访问的项目`ROOT`，其目录`\webapps\ROOT`下有一个`index.jsp`，启动Tomcat，观察`\work`目录，发现`\work\Catalina\localhost\ROOT`为空，浏览器访问localhost:8080端口后，`\work\Catalina\localhost\ROOT\org\apache\jsp`目录下生成两个文件`index_jsp.java`和`index_jsp.class`。

　　打开`index_jsp.java`文件，发现该类继承自`org.apache.jasper.runtime.HttpJspBase`：

```java
public final class index_jsp extends org.apache.jasper.runtime.HttpJspBase
    implements org.apache.jasper.runtime.JspSourceDependent,
                 org.apache.jasper.runtime.JspSourceImports
```

　　在Tomcat源码中找到该类，发现它继承自`HttpServlet`：

```java
public abstract class HttpJspBase extends HttpServlet implements HttpJspPage
```

　　即证明 Jsp 本质是一个Servlet。

　　除此之外，在`index_jsp.java`中还发现了大量的`out.write()`语句，这输出的其实就是写回到浏览器的页面，`out`是JSP内置对象中的一个，我们学习JSP除了语法之外学习的就是这几个内置对象。

<!-- more -->

<br>

----

### 1. JSP

#### 1.1 语法

- `<% 代码 %>`：包裹的代码将转换后将出现在service方法中；
- `<%! 代码 %>`：包裹的代码转换后将出现在类成员中；
- `<%= 代码 %>`：包裹的代码将输出到页面上;
- `<%@ 指令 属性=值 ... %>`：用于配置JSP的属性，指令如下：

```jsp
<%@ page contentType="text/html;charset=UTF-8" isErrorPage="true" language="java" %>
```

- page 指令：
  - contentType：等于 response.setContentType()；
  - import：导jar包；
  - errorPage：指定当前页面错误后是否跳转到错误页面；
  - isErrorPage：指定当前页面是否为错误页面；
  - include 指令：用于导入**页面**资源，用于页面组合；
  - taglib 指令：用于导入标签库；

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

- `<!-- 内容 -->`：只能注释html片段，会原样输出；
- `<%-- --%>`：代码级注释，不会输出到页面，推荐；

```jsp
<%@taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

<br>

#### 1.2 内置对象

　　内置对象不用创建，直接使用，实则是JSP转换为Servlet过程中帮我们创建了：

| JSP中对象   | 对应对象                        | 说明                                                 |
| ----------- | ------------------------------- | ---------------------------------------------------- |
| request     | HttpServletRequest              | 请求对象                                             |
| response    | HttpServletResponse             | 响应对象                                             |
| application | ServletContext                  | 全局共享对象                                         |
| page        | 当前Servlet                     | 即this                                               |
| pageContext | pageContext（jsp的）            | 可以在当前页面内共享数据，<br />可以获取其他内置对象 |
| config      | ServletConfig                   | servlet配置对象                                      |
| session     | HttpSession                     | 会话对象                                             |
| out         | JspWriter（继承java.io.Writer） | 输出对象                                             |
| exception   | Throwable                       | 异常对象，只有声明为IsErrorPage才有                  |

- 上述对象都声明在service方法内，即作用域在service方法内，只能在`<% %>`中调用。
- `out.write()`和`resp.getWriter().write()`的区别：两者有不同的缓冲区，resp的缓冲区优先输出，然后输出out的缓冲区，所以建议只使用out输出，html的body和`<%= %>`也会转化为out输出。

<br>

---

### 2. EL 表达式

　　表达式语言（Expression Language，EL），帮助我们简化JSP中Java代码的书写。

　　JSP 默认支持EL表达式，使用`<%@page isELIgnore="true"%>`全局忽EL表达式，也可以在表达式前使用转义字符`\`。

- **语法：`${表达式}`**

- **运算：**

  - 算数运算、比较运算、逻辑运算不用多说；
  - 空运算`empty`和非空运算`not  empty`：用于判断对象是否为null，若不为null同时判断长度是否为0；

  ```jsp
  ${empty str}
  ${not empty str}
  ```

- **获取值：`${域名称.键名}`**

| EL域名城         | 对应JSP域名称                 |
| ---------------- | ----------------------------- |
| pageScope        | pageContext                   |
| requestScope     | request                       |
| sessionScope     | session                       |
| applicationScope | application（ServletContext） |

　　注意：上述域的范围从小到大，取值的时候可以省略域名称，会自动按照域的范围从小到大查找；没找到也不会报错，只是不输出而已，这就很方便；

　　另外，获取对象的本质是调用对象的get方法，所以boolean类型要注意更改get方法将`isXxx()`修改为`getXxx()`，或者声明布尔值为包装类Boolean也可以。

　　获取 List：`${域名称.键名称[index]}`

　　获取 Map：`${域名称.键名称.key名称}` 或 `${域名称.键名称["key名称"]}`

- **内置对象**

  上面几个域就是EL的内置对象，还有一个最关键的内置对象`PageContext`，它和JSP中的pageContext可以理解为一个对象，可以用它获取JSP其他的内置对象，如获取虚拟目录：

  ```jsp
  <@-- 本质是连环调用get方法 --@>
  $pageContext.request.contextPath;
  ```

<br>

---

### 3. JSTL 标签库

> [菜鸟JSTL教程](https://www.runoob.com/jsp/jsp-jstl.html)

　　JSP标准标签库（JavaServer Pages Tag Library，JSTL），用于更进一步简化、替换JSP中Java代码的书写，它支持通用的、结构化的业务操作，如循环、条件判断、SQL标签。

　　需要导入jar包和在JSP中使用taglib指令引入JSTL标签：

```jsp
<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
```

- 常用标签

| 标签                                                         | 描述                                               |
| :----------------------------------------------------------- | :------------------------------------------------- |
| [c:out](https://www.runoob.com/jsp/jstl-core-out-tag.html)   | 相当于<%= ... >                                    |
| [c:set](https://www.runoob.com/jsp/jstl-core-set-tag.html)   | 用于保存数据                                       |
| [c:remove](https://www.runoob.com/jsp/jstl-core-remove-tag.html) | 用于删除数据                                       |
| [c:catch](https://www.runoob.com/jsp/jstl-core-catch-tag.html) | 用来处理产生错误的异常状况，并且将错误信息储存起来 |
| [**c:if**](https://www.runoob.com/jsp/jstl-core-if-tag.html) | if逻辑                                             |
| [**c:choose**](https://www.runoob.com/jsp/jstl-core-choose-tag.html) | switch逻辑                                         |
| [c:when](https://www.runoob.com/jsp/jstl-core-choose-tag.html) | c:choose的子标签，相当于case                       |
| [c:otherwise](https://www.runoob.com/jsp/jstl-core-choose-tag.html) | c:choose的子标签，相当于default                    |
| [c:inport](https://www.runoob.com/jsp/jstl-core-import-tag.html) | 检索一个绝对或相对 URL，然后将其内容暴露给页面     |
| [**c:forEach**](https://www.runoob.com/jsp/jstl-core-foreach-tag.html) | for逻辑                                            |
| [c:forTokens](https://www.runoob.com/jsp/jstl-core-foreach-tag.html) | 根据指定的分隔符来分隔内容并迭代输出               |
| [c:forTokens](https://www.runoob.com/jsp/jstl-core-param-tag.html) | 用来给包含或重定向的页面传递参数                   |
| [c:redirect](https://www.runoob.com/jsp/jstl-core-redirect-tag.html) | 重定向至一个新的URL.                               |
| [c:url](https://www.runoob.com/jsp/jstl-core-url-tag.html)   | 使用可选的查询参数来创造一个URL                    |

- if 标签

  ```jsp
  <c:if test="<boolean>" var="<string>" scope="<string>">
     ...
  </c:if>
  ```

  - test：条件；
  - var：存储条件结果的变量；
  - scope：var的作用域，默认为page；

- choose 标签

  ```jsp
  <c:choose>
      <c:when test="<boolean>">
          ...
      </c:when>
      <c:when test="<boolean>">
          ...
      </c:when>
     		...
      	...
      <c:otherwise>
          ...
      </c:otherwise>
  </c:choose>
  ```

- forEach 标签

  ```jsp
  <c:forEach
      items="<object>"
      begin="<int>"
      end="<int>"
      step="<int>"
      var="<string>"
      varStatus="<string>">
  	...
  </c:forEach>
  ```

  - items：要遍历的对象；
  - var：遍历到的当前元素的名称；
  - begin：起始索引，默认为0；
  - end：终止索引，默认为最后一个元素；
  - step：步长；
  - varStatus：代表循环状态的对象的名称
    - count属性表示当前遍历到第几次。

