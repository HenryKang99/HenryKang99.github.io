### 0verview

<img src="D:/OneDrive/_mine/docsify/_img/0CUlLFDFs64k.png" alt="mark" style="zoom: 33%;" />

> **注意区分三层架构与MVC模型的区别。**
>
> 三层架构：数据访问层，业务逻辑层，界面层。
> MVC：VC对应着界面层，M对应着业务逻辑+数据访问层。
>
> - Model 业务实现Bean+数据Bean；
>
> - View 可以理解为"真·页面层"，只负责展示数据；
>
> - Controller 负责流程的控制和业务的分发，响应用户的请求，调用业务逻辑层相关方法，获取结果转发给View层进行展示。切记Controller只负责业务流程的分发控制，不负责具体的业务处理。
>
> SpringMVC 有着清晰的角色划分：核心控制器、处理映射器、处理适配器、视图解析器…… 分工明确，扩展灵活，而且其强大的数据验证、格式化和绑定机制大大提高了我们的开发效率。

### 1. HelloSpringMVC

> 　　学习SpringMVC一定要联想到Servlet，**SpringMVC的入口就是一个Servlet**，所有的请求发过来首先都会经过核心控制器DispatcherServlet。

#### 1.1 环境搭建

0. 配置的思路：因为入口是DispatcherServlet，所以当服务器启动时最好对其进行初始化；与此同时让其帮我们读取配置文件，创建Spring的核心容器；

1. 创建Maven工程，导入坐标；

```xml pom.xml >folded
<properties>
	<spring.version>5.0.2.RELEASE</spring.version>
</properties>
<dependencies>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-context</artifactId>
		<version>${spring.version}</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-web</artifactId>
		<version>${spring.version}</version>
	</dependency>
	<dependency>
		<groupId>org.springframework</groupId>
		<artifactId>spring-webmvc</artifactId>
		<version>${spring.version}</version>
	</dependency>
	<dependency>
		<groupId>javax.servlet</groupId>
		<artifactId>servlet-api</artifactId>
		<version>2.5</version>
		<scope>provided</scope>
	</dependency>
	<dependency>
		<groupId>javax.servlet.jsp</groupId>
		<artifactId>jsp-api</artifactId>
		<version>2.0</version>
		<scope>provided</scope>
	</dependency>
</dependencies>
```

2. `web.xml`中配置核心控制器（SpringMVC的入口）；

```xml web.xml >folded
<servlet>
	<servlet-name>dispatcherServlet</servlet-name>
	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
	<!-- 配置核心控制器的初始化参数，让它读取配置文件，创建spring容器 -->
	<init-param>
		<param-name>contextConfigLocation</param-name>
		<param-value>classpath:SpringMVC.xml</param-value>
    </init-param>
    <!-- 配置servlet启动时创建核心控制器 -->
	<load-on-startup>1</load-on-startup>
</servlet>
<servlet-mapping>
	<servlet-name>dispatcherServlet</servlet-name>
	<!-- 请求发来都会先经过核心控制器 -->
	<url-pattern>/</url-pattern>
</servlet-mapping>
```

3. 编写`SpringMVC.xml`;

```xml SpringMVC.xml >folded
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
		xmlns:mvc="http://www.springframework.org/schema/mvc"
		xmlns:context="http://www.springframework.org/schema/context"
		xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
		xsi:schemaLocation="
		http://www.springframework.org/schema/beans
		http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/mvc
		http://www.springframework.org/schema/mvc/spring-mvc.xsd
		http://www.springframework.org/schema/context
		http://www.springframework.org/schema/context/spring-context.xsd">
	<!-- 配置spring创建容器时要扫描的包 -->
	<context:component-scan base-package="com.istudy"></context:component-scan>
	<!-- 配置视图解析器 -->
	<bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="prefix" value="/WEB-INF/pages/"></property>
		<property name="suffix" value=".jsp"></property>
	</bean>
	<!-- 配置spring开启注解mvc的支持-->
    <!-- 配置了此行，还会自动帮我们将处理映射器、处理适配器等一些必备模块注册到spring容器中-->
	<mvc:annotation-driven></mvc:annotation-driven>
</beans>

```

4. 编写jsp 与 HelloSpringMVC控制类;

```jsp index.jsp >folded
<body>
    <!-- <a href="hello">相对路径不要写斜杠</a> -->
	<a href="${pageContext.request.contextPath}/hello">点我跳转</a>
</body>
```

```jsp success.jsp >folded
<body>
	<h3>Hello SpringMVC！！</h3>
</body>
```

```java HelloSpringMVC.java >folded
@Controller
public class HelloSpringMVC {
	@RequestMapping(path="/hello")
	public String hello() {
		return "success";
	}
}
```

#### 1.2 流程分析

1. 当启动Tomcat服务器的时候，因为配置了load-on-startup标签，所以会创建DispatcherServlet对象，就会加载SpringMVC.xml配置文件，创建spring核心容器并创建我们需要的类实例；
2. 开启了注解扫描，那么HelloSpringMVC对象就会被创建并加入到spring容器；
3. 从index.jsp发送请求，请求会先到达DispatcherServlet核心控制器，根据配置@RequestMapping注解找到执行的具体方法；
4. 根据执行方法的返回值，再根据配置的视图解析器，去指定的目录下查找指定名称的JSP文件；
5. Tomcat服务器渲染页面，做出响应。

　

----

### 2. 请求参数绑定

> 　　请求参数的绑定是自动的，但必须符合一定的规范；其次，前端发送过来的都是字符串类型，能够容易的封装是因为SpringMVC内置了很多类型转换器，基本上能够满足所有的需求，但有时我们还是需要定义类型转换器，如我们想要把日期转换成我们自定义的格式。

#### 2.1 参数绑定

- 基本型和String型：控制器方法的形参名称与所提交数据的name属性保持一致；
- pojo类型：所提交数据的name属性要与pojo类中的属性名保持一致，若pojo1关联到另一个pojo2中，则name属性应该这样写：`pojo1在pojo2中的名字.pojo1中的属性名`；
- 集合类型：name属性使用类似JS的写法，如：`accounts[0].name，accounts[0].money`；`accountMap['one'].name，accountMap['one'].money`。

总结：其实就是让方法的形参名、提交数据的name属性与定义的java类中的属性名一致。

#### 2.2 封装 json

- 见4.3节。

#### 2.3 配置过滤器解决乱码问题

```xml
<!-- 配置过滤器，解决中文乱码的问题 -->
<filter>
	<filter-name>characterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
	<!-- 指定字符集 -->
    <init-param>
    	<param-name>encoding</param-name>
    	<param-value>UTF-8</param-value>
	</init-param>			
</filter>
<!-- 过滤所有请求 -->
<filter-mapping>
	<filter-name>characterEncodingFilter</filter-name>
	<url-pattern>/*</url-pattern>
</filter-mapping>
<!-- 在SpringMVC.xml配置静态资源不过滤 -->
<mvc:resources location="/js/" mapping="/js/**"/>
<mvc:resources location="/css/" mapping="/css/**"/>
<mvc:resources location="/images/" mapping="/images/**"/>
```

#### 2.4 自定义类型转换器

1. 自定义一个转换类实现`Converter<Source,Target>`的接口；
2. 重写方法制定转换规则；
3. 注册自定义的类型转换器：

```xml
<bean id="converterService"
class="org.springframework.context.support.ConversionServiceFactoryBean">
	<!-- 给工厂注入一个新的类型转换器 -->
	<property name="converters">
		<list>
		<!-- 配置自定义类型转换器 -->
			<bean class="com.istudy.utils.StringToDateConverter"></bean>
		</list>
	</property>
</bean>
<!-- 在 annotation-driven 标签中引用配置的类型转换器 -->
<mvc:annotation-driven conversion-service="conversionService"/>
```

　

---

### 3. 常用注解

- `@RequestMapping`

  - value：指定请求的url；
  - method：指定请求方式；
  - params：指定请求参数的条件，可以使用简单的表达式；
  - headers：指定请求消息头的条件；

- `@RequestParam`：

  - value：指定请求参数的name属性；

  - required：指定该参数是否必须，默认为true；

- `@RequestBody`：获取请求体

  - required：指定是否必须有请求体，默认为true；
    - 默认情况下使用get请求会报错，改为false后使用get请求返回null；

- `@RequestHeader`

  - value：消息头名称；
  - required：是否必须有此消息头；

- `@PathVariable`：restful

  - value：指定url中的占位符名称；
  - required：是否必须提供占位符；

- `@CookieValue`

  - value：指定cookie名称;
  - required：是否必须有此cookie;

- `@SessionAttribute`：

  - value：指定存入的属性名称；
  - type：指定存入的数据类型；

- `@ModelAttribute`：

  - 出现在方法上，表示当前方法会在控制器的方法执行之前执行；
    - 有返回值，返回值会传递给控制器方法；
    - 无返回值，将值加入到一个map中，通过在控制器方法参数上注解绑定；
  - 出现在参数上，获取指定的数据给参数赋值；

  ```java >folded
  @ModelAttribute
  public void showModel(String username,Map<String,User> map) {
      //模拟去数据库查询
      User user = findUserByName(username);
      System.out.println("执行了 showModel 方法"+user);
      map.put("abc",user);
  }
  
  @RequestMapping("/updateUser")
  public String testModelAttribute(@ModelAttribute("abc")User user) {
      System.out.println("控制器中处理请求的方法：修改用户："+user);
      return "success";
  }
  ```

  　

----

### 4. 响应数据处理

#### 4.1 返回值分类

- **字符串**：作为逻辑视图的名称传递给视图解析器；

- **void**：默认寻找和访问路径同名的页面，可以通过转发或者重定向避免默认情况的发生，还可以直接使用response写回数据。
- **ModelAndView**：SpringMVC为我们提供的一个对象，用于封装数据加入到request域，原理和使用形参+返回String相同；有两个主要方法：`addObject(key,value)`和`setViewName(str)`;

#### 4.2 转发和重定向

- 转发：`return "forward:/WEB-INF/pages/success.jsp"`;
- 重定向：`return "redirect:/user/findAll"`;
  - 重定向到本服务器，则前缀可以不写，即只写uri。

#### 4.3 响应json数据

> 导包：jackson-annotations、jackson-databind、jackson-core；
>
> 使用 @RequestBody、@ResponseBody注解

1. 封装Json数据：

　　使用`@RequestBody`可以获取到请求体内容，注解到String类型参数上可以直接获取json串，若参数是pojo类，pojo中的属性名称和json串中的key对应，可以自动封装；

2. 响应Json数据：

　　使用`@ResponseBody`注解修饰方法返回值即可；

```java
@RequestMapping("/testResponseJson")
@ResponseBody
public Account testResponseJson(@RequestBody Account account) {
	System.out.println("异步请求："+account);
	return account;
}
```

　

----

### 5. 异常处理器和拦截器

#### 5.1 异常处理器

> 　　SpringMVC中异常层层上抛，若抛到核心控制器层还没有捕获，则会继续抛出到页面上，非常不友好，我们需要注册一个异常处理器，当核心控制器接到异常时会转交给异常处理器，在异常处理器中我们可以跳转到用户友好页面。

0. 自定义异常和错误页面；

```java
public class CustomException extends Exception {
    private String message;
    public CustomException(String message) {
    	this.message = message;
    }
    public String getMessage() {
    	return message;
    }
}
```

1. 自定义异常处理器，继承自`HandlerExceptionResolver`；

```java
public class CustomExceptionResolver implements HandlerExceptionResolver{
    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {
        ex.printStackTrace();
        CustomException e = null;
        // 获取到异常对象
        if(ex instanceof CustomException) {
            // 向下转型
        	e = (CustomException) ex;
        }else {
        	e = new CustomException("请联系管理员");
        }
        ModelAndView mv = new ModelAndView();
        // 存入错误的提示信息
        mv.addObject("message", e.getMessage());
        // 跳转的Jsp页面
        mv.setViewName("error");
        return mv;
    }
}
```

2. 注册异常处理器；

```java
<!-- 配置异常处理器 -->
<bean id="sysExceptionResolver" class="com.istudy.exception.CustomExceptionResolver"/>
```

#### 5.2 拦截器

> 　　区别于Servlet的过滤器，过滤器可以过滤对任何资源的请求；而SpringMVC的拦截器只能拦截对控制器中方法的请求，它利用了AOP思想。

1. **自定义拦截器，实现`HandlerInterceptor`接口；**

```
public class LoginInterceptor implements HandlerInterceptor{
    @Override
    Public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        //如果是登录页面则放行
        if(request.getRequestURI().indexOf("login.action")>=0){
        	return true;
        }
        HttpSession session = request.getSession();
        //如果用户已登录也放行
        if(session.getAttribute("user")!=null){
        	return true;
        }
        //用户没有登录跳转到登录页面
        request.getRequestDispatcher("/WEB-INF/jsp/login.jsp").forward(request,response);
        return false;
    }
}
```

2. **重写方法：**

- `Boolean preHandle()`：再控制器方法执行之前执行；
  - return true 则放行；
- `void postHandle()`：在控制器方法执行之后**未返回之前**执行；
  - 可以转发、重定向到其他资源，原控制器的return不生效；
- `void afterCompletion()`：在控制器方法执行且**返回之后**执行；
  - 无法转发、重定向，因为控制器方法已经返回了。

3. **注册拦截器：**

```xml
<mvc:interceptors>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean id="handlerInterceptorDemo1" class="com.istudy.web.interceptor.HandlerInterceptorDemo1"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

#### 5.3 拦截器顺序说明

　　设多个拦截器**按配置顺序**为：HandlerInterceptor A，HandlerInterceptor B，则执行顺序如下，

　　A.preHandle() >> B.preHandle() >> 控制器方法 >> B.postHandle() >> A.postHandle() >> B.afterCompletion() >> A.afterCompletion() >> View；其中若A.preHandle()返回了false，则直接 >> View，若B.preHandle()返回了false，则返回到A.afterCompletion()执行。

　　总结：这个过程类似于中断调用与返回。

　