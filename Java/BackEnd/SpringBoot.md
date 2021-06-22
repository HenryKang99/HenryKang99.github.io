## Features

- Create stand-alone Spring applications
- Embed Tomcat, Jetty or Undertow directly (no need to deploy WAR files)
- Provide opinionated 'starter' dependencies to simplify your build configuration
- Automatically configure Spring and 3rd party libraries whenever possible
- Provide production-ready features such as metrics, health checks, and externalized configuration
- Absolutely no code generation and no requirement for XML configuration
- [SpringBoot文档](https://docs.spring.io/spring-boot/docs/current/reference/)

---

## Hello World

### pom

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.4.0</version>
    <relativePath/>
</parent>
<dependencies>
    <!-- starter：场景启动器，根据不同的场景，导入所需的依赖 -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>

    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
</dependencies>

<build>
    <plugins>
        <!-- 该插件用于将应用打成jar包，简化部署，可以用 java -jar 直接运行在内置的 servelet 容器 -->
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

　　\<parent> 的 parent 项目为：`spring-boot-dependencies`，这是springboot真正管理依赖的地方。

```xml
<groupId>org.springframework.boot</groupId>
<artifactId>spring-boot-dependencies</artifactId>
<version>2.4.0</version>
<packaging>pom</packaging>

<dependencyManagement>
    <!-- 约定了大量的依赖 -->
    <dependencies>
      <dependency>
        <groupId>org.apache.activemq</groupId>
        <artifactId>activemq-amqp</artifactId>
        <version>${activemq.version}</version>
      </dependency>
        ......
    </dependencies>
</dependencyManagement>
```

　　

### 主启动类

```java
@SpringBootApplication
public class HelloWorldApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloWorldApplication.class,args);
    }
}
```

　　`@SpringBootApplication`说明该类是此项目的主启动类/主配置类，SpringBoot 会将该类的Main方法作为入口。

　　

### 配置文件

#### YAML写法

- 字符串不用加引号。

  - 加单引号：将会转义特殊字符，如  '\n' 会原样输出。
  - 加双引号：不会转义特殊字符，如  “\n” 会换行。

- 驼峰和短横杠链接效果一样。

- 对象写法：

  ```yaml
  # 写法一
  person:
    firstName: xx
    lastName: xx
  # 写法二
  person: {firstName: xx, lastName: xx}
  ```

- 数组写法：

  ```yml
  # 写法一
  person:
    - zhangsan
    - lisi
    - wangwu
  # 写法二
  person: [zhangsan,lisi,wangwu]
  ```

- 占位符与默认值：（也适用于 properties）

  ```yaml
  person:
    first-name: zhang
    last-name: san
    id: ${random.uuid}
  People:
    name: ${person.first-name:xxx}_${person.last-name}
  # 获取不到值，就原样输出表达式。
  # 可以使用一些特殊的表达式，如 ${random.uuid}。
  # 其中${person.first-name:xxx} 冒号后面（不带空格）指定的就是默认值。
  ```

  

#### 相关注解

##### @PropertySourse

　　`@PropertySourse("classpath:xxx")` 声明加载指定配置文件。

##### @ImportResource

　　`@ImportResource(location = {"classpath: bean.xml"})` 导入自己编写的  Spring 配置文件，放在配置类头上

##### @Configuration

　　放类头上，声明该类是一个配置类。

```java
@Configuration
public class MyConfig {
    //将 id = bean 的 Bean 添加到容器中。
    @Bean
    public Bean bean(){
        return new Bean();
    }
}
```

　　

#### 多配置文件

> 格式：application-xxx.yml

##### 配置文件中指定

1. 多文件

　　在默认配置文件中添加 `spring.profiles.active=xxx` 指定加载哪个配置文件。

2. yml 分块

```yml
# 使用三个短横杆分块
server:				# 第一块相当于主配置文件
  port: 8081
spring:
  profiles:
    active: prod	# 声明激活哪一个块
‐‐‐
server:
  port: 8083
spring:
  profiles: dev		# 声明该块的名字
‐‐‐
server:
  port: 8084
spring:
  profiles: prod
```

　　

##### 命令行参数指定

```bash
java -jar xxx.jar --spring.profile.activev=xxx
```

　　

##### 配置文件加载规则

　　**规则：**按照优先级顺序加载配置文件，配置“冲突”时，高优先级会覆盖低优先级，而优先级取决于配置文件放的位置。

　　**位置：**spring boot 会加载下面路径中的<u>所有</u> application.yml/properties，并且优先级从高到低。

0. 使用命令行参数指定参数

   ```bash
   # 指定要加载的配置文件位置
   # 多个属性空格隔开
   java -jar xxx.jar --spring.config.location=xxx --server.port=8080
   ```

1. `jar包外部的默认配置文件`
2. `../config/`
3. `../`
4. `classpath:/config/`
5. `classpath:/`
6. ……
7. @Configuration 指定的配置类

　　**小结：**按优先级（jar包由外向内）加载所有默认配置文件，配置互补，冲突覆盖。

#### Bean注入

##### @ConfigurationProperties

　　`@ConfigurationProperties(prefix="xxx")`加在Bean上，注意别忘了把Bean加入容器。

　　支持松散绑定，即可以识别驼峰或短斜杠，不支持 SpEL 表达式，支持 JSR-303 校验，支持复杂类型。

　　导入配置文件处理器，用于写配置时有提示。

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring‐boot‐configuration‐processor</artifactId>
    <optional>true</optional>
</dependency>
```

##### @Value

　　`@Value("xx")`加Bean的成员上。不支持松散绑定，支持 SpEL 表达式，不支持 JSR-303 校验，不支持复杂类型。

----

## 日志模块

> 常见的日志接口：JCL、SLF4J、JBoss-Logging
>
> 常见的日志实现：Log4J、Log4J2、JUL、Logback
>
> springboot 默认使用：SLF4J + Logback

### 统一日志框架

> 项目中的某些组件可能依赖于其他日志框架，我们想要全部转换为 slf4j + logback。

**步骤：**

1. 排除其他日志框架。
2. 使用中间包替换被排除的日志框架。
3. 导入slf4j 实现。

　　springboot 已经帮我们做好了，我们需要做的是：当我们引入了一个依赖其他日志框架的组件时，手动在pom文件中，将该日志框架排除掉，如：

```xml
<!-- 排除 spring 依赖的 commons-logging -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring‐core</artifactId>
    <exclusions>
        <exclusion>
            <groupId>commons‐logging</groupId>
            <artifactId>commons‐logging</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```



### 日志使用

#### HelloLogger

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
public class HelloLog {
  public static void main(String[] args) {
    // 获取日志记录器 logger
    Logger logger = LoggerFactory.getLogger(HelloLog.class);
    
    // 记录日志
    logger.trace("Hello World");
    logger.debug("Hello World");
    // 默认级别
    logger.info("Hello World");
    logger.warn("Hello World");
    logger.error("Hello World");
  }
}
```

#### 配置

```properties
# 指定某个包下的日志级别
# 没有配置的还是使用默认级别 info
logging.level.site.henrykang.xxx=trace 

# 指定日志输出的文件名，默认只会输出到控制台
# 也可以指定路径
logging.file=mylog.log

# 指定日志存放的目录，文件名使用默认的 spring.log
# 该配置和 logging.file 冲突，二选一使用即可
logging.path=./log

# 指定控制台输出格式
logging.pattern.console=%d{yyyy/MM/dd-HH:mm:ss} [%thread] %-5level %logger- %msg%n 
# 指定文件记录格式
logging.pattern.file=%d{yyyy/MM/dd-HH:mm} [%thread] %-5level %logger- %msg%n

# 日志输出格式控制：
# %d 			日期时间      
# %thread		表示线程名     
# %‐5level		级别从左显示5个字符宽度        
# %logger{50} 	表示logger名字最长50个字符，否则按照句点分割     
# %msg			日志消息        
# %n			换行符
# %d{yyyy‐MM‐dd HH:mm:ss.SSS} [%thread] %‐5level %logger{50} ‐ %msg%n
```

----

## Web 模块

### 静态资源问题

**两种方式：**

1. [webjars](https://www.webjars.org/): 以 jar 包的方式在 pom 中引入静态资源，这种方式下静态资源会被映射到 `META-INF/resources/webjars/`目录下。

   ```url
   # 这样访问静态资源
   http://localhost/项目path/webjars/xxx
   ```

2. 普通方式: 下面路径都会映射到 `/**`，直接在根路径`/项目path/xxx`下访问静态资源。

   ```yml
   classpath:/META‐INF/resources/
   classpath:/resources/
   classpath:/static/
   classpath:/public
   /:当前项目的根
   ```

   **注意:** 工程中的 java、resources 目录都是根路径，打包到 classes 文件夹下。

　　

### SpringMVC配置

> 自动配置文件：
>
> org/springframework/boot/autoconfigure/web/servlet/WebMvcAutoConfiguration.java

#### 自动配置

> SpringBoot 对 SpringMVC 进行了哪些自动配置：
>
> **Spring MVC Auto-configuration**
> Spring Boot provides auto-configuration for Spring MVC that works well with most applications.
> The auto-configuration adds the following features on top of Spring’s defaults:
>
> - Inclusion of ContentNegotiatingViewResolver and BeanNameViewResolver beans.
> - Support for serving static resources, including support for WebJars.
> - Automatic registration of Converter, GenericConverter, and Formatter beans.
> - Support for HttpMessageConverters.
> - Automatic registration of MessageCodesResolver.
> - Static index.html support.
> - Automatic use of a ConfigurableWebBindingInitializer bean.



#### 扩展配置

> 我们如何新增自定义的配置：
>
> If you want to keep those Spring Boot MVC customizations and make more MVC customizations(interceptors, formatters, view controllers, and other features), you can add your own `@Configuration` class of type `WebMvcConfigurer` but without `@EnableWebMvc`.
>
> If  you  want  to  provide  custom  instances  of  RequestMappingHandlerMapping,
> RequestMappingHandlerAdapter, or ExceptionHandlerExceptionResolver, and still keep the Spring Boot MVC customizations, you can declare a bean of type WebMvcRegistrations and use it to provide 99 custom instances of those components.

```java
// 例如新增访问路径mapping
@Configuration
public class MyMvcConfig extends WebMvcConfigurerAdapter {
    @Override
    public void addViewControllers(ViewControllerRegistry registry) {
        registry.addViewController("/xxx").setViewName("xxx");
    }
}
```



#### 屏蔽自动配置

> 屏蔽自动配置，自己配：
>
> If you want to take complete control of Spring MVC, you can add your own `@Configuration` annotated with `@EnableWebMvc`, or alternatively add your own @Configuration-annotated DelegatingWebMvcConfiguration as described in the Javadoc of @EnableWebMvc.

　　启动类头上加上 `@EnableWebMvc` 注解，屏蔽自动配置，仅使用自己的配置。

　　

### 错误处理

#### 定制错误页面

　　按照静态资源访问规则，在 /error 文件夹下面定义 {errorCode}.html 。如404.html、4xx.html，优先精确匹配。

<br>

#### 统一异常处理

[统一异常处理](http://blog.didispace.com/springbootexception/)

---

### Servlet容器配置

#### 切换嵌入式容器

#### 注册三大组件

#### 使用外置容器

---

### MyBatis配置

```yml
spring:
  application:
    name: cloud-payment-service
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: org.gjt.mm.mysql.Driver
    url: jdbc:mysql://localhost:3306/spring_cloud_study?useUnicode=true&characterEncoding=utf-8&useSSL=false
    username: root
    password: root
mybatis:
  mapperLocations: classpath:mapper/*.xml
  type-aliases-package: site.henrykang.springcloud.entity
```

</br>

---

### Actuator

> The spring-boot-actuator module provides all of Spring Boot’s **production-ready** features.

- 添加依赖

  ```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-actuator</artifactId>
    </dependency>
</dependencies>
  ```

- 打开/关闭相关功能：

  ```properties
  # management.endpoint.<id>.enabled
  management.endpoint.shutdown.enabled=true
  ```

- Web暴露相关功能：

  ```properties
  # 默认只有 health、info 打开
  management.endpoints.jmx.exposure.exclude=xxx
  management.endpoints.jmx.exposure.include=*
  management.endpoints.web.exposure.exclude=xxx
  management.endpoints.web.exposure.include=health,info
  ```

  

---

## 其他

### Swagger2

[文档](https://springfox.github.io/springfox/docs/current/#getting-started)

#### 快速使用

- 引入依赖：

  ```xml
  <dependency>
      <groupId>io.springfox</groupId>
      <artifactId>springfox-boot-starter</artifactId>
      <version>3.0.0</version>
  </dependency>
  ```

- 配置类：

  ```java
  @Configuration
  @EnableSwagger2
  @ComponentScan(basePackages = "site.henrykang.controller")
  public class Swagger2SpringBoot {
      
      private ApiInfo apiInfo() {
          return new ApiInfoBuilder()
                  .title("xxAPI文档")
                  .description("这里是描述……")
                  .version("1.0.0")
                  .build();
      }
      
      @Bean
      public Docket xxApi() {
          return new Docket(DocumentationType.SWAGGER_2)
                  .enable(true)
                  .apiInfo(apiInfo())
                  .groupName("xx模块")
                  .select()
                	.apis(RequestHandlerSelectors.basePackage("site.henrykang.controller.user"))
                  .build();
      }
  }
  ```

- Controller：

  ```java
  @Api(tags = "模块描述")
  @Xxx
  public class XxxController {
  
      @ApiOperation("方法描述")
      @PostMapping
      public Result postSth(
              @ApiParam("参数描述") Param param1,
           	@ApiParam("参数描述") Param param2,
  	        @ApiParam("参数描述") Param param3
      ) {
         
      }
  }
  ```

- Bean：

  ```java
  @ApiModel
  public class Bean {
  
      @ApiModelProperty(value = "描述", position = 1, hidden = true)
      private Long para1;
  
      @ApiModelProperty(value = "提问者ID", position = 2)
      private Long para2;
  }
  ```

----

### JSR-303

#### 快速使用

- 引入依赖：

  ```xml
  <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-validation</artifactId>
  </dependency>
  ```

- 使用

  ```java
  // 注意包
  import javax.validation.constraints.NotBlank;
  
  // 实体属性上加
  @NotNull		// 验证对象是否非Null
  @NotBlank		// 验证String是否为空、
  @NotEmpty		// 验证容器是否为空
  @Pattern		// 验证自定义正则表达式
  @AssertTrue		// 验证Boolean
  @Min			// 验证数值或String类型
  @Max
  @Size			// 验证容器或String的长度范围
  @Past			// 验证时间戳是否在当前时间之前
  @Future	
  @Email			// 验证是否邮箱格式
  ……
      
  // 接口上需要验证的参数前加
  @Valid 或 @Validated
  // eg
  @PostMapping("/post")
  @ApiOperation("post操作")
  public People post(
          @ApiParam("people") @Valid @RequestBody People people,
          BindingResult bindingResult // 这个参数封装了校验结果
  ) {
      if (bindingResult.hasErrors()) {
          bindingResult.getFieldErrors().forEach((item) -> {
              log.info(item.getField() + item.getDefaultMessage());
          });
      }
  
      return people;
  }
  ```

</br>

#### 分组校验

> 　　目的是给一个属性加上多种校验规则，在不同的条件下，使用不同的规则。例如自增的ID属性，插入时需要为NULL，修改时需要为NotNull。

- 发现这些个校验注解都有一个分组的属性，且是一个数组

  ```java
  Class<?>[] groups() default {};
  ```

- 创建标记接口

  ```java
  public interface AddGroup {}
  public interface UpdateGroup {}
  ```

- 加注解时传入指定的标记

  ```java
  @NotNull(message = "Update时要非空ID",groups = {UpdateGroup.class})
  @Null(message = "Add时要空ID",groups = {AddGroup.class})
  @ApiModelProperty(value = "id", position = 0, hidden = true)
  private String id;
  ```

- 在期望校验的地方也要传入标记

  ```java
  @ApiParam("people") @Validated({AddGroup.class}) @RequestBody People people
  ```

- 问题：发现 Bean 中其他没添加分组的属性，都不会被校验。

  ```java
  // 解决：让我们的标记接口继承 Default 接口
  // javax.validation.groups.Default
  
  public interface AddGroup extends Default {}
  public interface UpdateGroup extends Default {}
  
  // 分组特别多的话，为了统一写成内部接口也行，就是注解稍微长点
  public interface XxxGroup{
      interface AddGroup extends Default{};
      interface UpdateGroup extends Default{};
  }
  ```

</br>

#### 统一处理

```java
// 不需要在每个接口后面接收BindingResult参数了
@RestControllerAdvice(basePackages = "site.henrykang.controller")
public class GlobalExceptionControllerAdvice {

    @ExceptionHandler(value= MethodArgumentNotValidException.class)
    public R handleValidException(Exception e){
        MethodArgumentNotValidException ex = null;
        if (e instanceof MethodArgumentNotValidException) {
            ex = (MethodArgumentNotValidException) e;
        }
        BindingResult bindingResult = ex.getBindingResult();
        Map<String,Object> map = new HashMap<>();
        bindingResult.getFieldErrors().forEach((item)->
                map.put(item.getField(),item.getDefaultMessage())
        );
        return R.error().setCode(400).setMessage("参数异常").setData(map);
    }
}
```

#### 非Object属性的校验

　　例如想对get请求的某个参数进行校验。

1. 在相应Controller上添加@Validated；

2. 再在参数前使用相应注解即可。

3. 会抛出 `javax.validation.ConstraintViolationException` 异常。

4. 统一异常处理

   ```java
   @ExceptionHandler(ConstraintViolationException.class)
       public R handleValidationException(ConstraintViolationException e){
           Map<String, Object> map = new HashMap<>();
           Set<ConstraintViolation<?>> constraintViolations = e.getConstraintViolations();
           constraintViolations.forEach((item) -> {
               map.put(String.valueOf(item.getPropertyPath()), item.getMessage());
           });
           return R.error().setCode(400).setMessage("参数异常").setData(map);
       }
   ```

   

----

### AOP统一日志

[AOP统一处理Web请求日志](http://blog.didispace.com/springbootaoplog/)

