### 0verview

> Spring是一个轻量级的以**控制反转**(IoC)和**面向切面编程**(AOP)思想为核心的容器框架。
>
> IOC：我们可以将所有对象的创建和依赖关系的维护交由Spring管理，方便解耦，简化开发；
>
> AOP：可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性；也可以在不修改源代码的情况下，对某些方法进行增强，例如日志记录，事务处理用aop来实现，和真正的业务逻辑分离开来，降低了耦合度。

- 一个简单的工厂模式

```java
public class BeanFactory {
    private static Properties props;
    // 创建一个存放对象的容器
    private static Map<String,Object> beans;
    //使用静态代码块为Properties对象赋值
    static {
        try {
            //实例化对象
            props = new Properties();
            //获取properties文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
            //实例化容器
            beans = new HashMap<String,Object>();
            Enumeration keys = props.keys();
            while (keys.hasMoreElements()){
                String key = keys.nextElement().toString();
                String beanPath = props.getProperty(key);
                Object value = Class.forName(beanPath).newInstance();
                beans.put(key,value);
            }
        }catch(Exception e){
            throw new ExceptionInInitializerError("初始化properties失败！");
        }
    }

    public static Object getBean(String beanName){
        return beans.get(beanName);
    }

}
```

### 1. HelloSpring

#### 1.1 快速入门

1. 创建Maven工程, 导入坐标;

```xml >folded
<!-- spring5.x版本要求junit4.x、tomcat8.5、jdk8以上 -->
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-context</artifactId>
	<version>5.0.2.RELEASE</version>
</dependency>
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-test</artifactId>
	<version>5.0.2.RELEASE</version>
    <scope>test</scope>
</dependency>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.13</version>
    <scope>test</scope>
</dependency>
```

2. 创建核心配置文件`bean.xml`;
3. 配置`<bean>`标签

```xml >folded
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--把对象的创建交给spring来管理-->
    <!--默认调用无参构造函数，没有找到会报错-->
    <!--bean标签的其他属性在后面有记录-->
	<bean id="student" class="site.henrykang.domain.Student"></bean>
</beans>
```

4. 步骤:

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:bean.xml")
public class TestGetBean {
    private ApplicationContext ac = null;
    
    @Before
    public void init() {
        //方式一：通过ApplicationContext获取容器对象
        ac = new ClassPathXmlApplicationContext("bean.xml");
    }
    
    @Test
    public void testGetBean() {
        Student stu = (Student) ac.getBean("student");
        System.out.println(stu);
    }

    @Test
    public void testGetBeanByBeanFactory() {
        //方式二：通过BeanFactory获取容器对象
        Resource resource = new ClassPathResource("bean.xml");
        BeanFactory factory = new XmlBeanFactory(resource);
        Student stu = (Student) factory.getBean("student");
        System.out.println(stu);
    }
}
```

- 核心容器接口

![mark](D:/OneDrive/_mine/docsify/_img/LRBc7Al25nRe.png)



| 接口               | 描述                       |
| :----------------- | :------------------------- |
| ApplicationContext | （读取完配置文件）立即加载 |
| BeanFactory        | 延迟加载                   |

- ApplicationContext 三个常用的实现类

| 实现类                             | 描述                         |
| :--------------------------------- | :--------------------------- |
| ClassPathXmlApplicationContext     | 加载**类路径**下的配置文件   |
| FileSystemXmlApplicationContext    | 加载磁盘任意路径下的配置文件 |
| AnnotationConfigApplicationContext | 用于读取**注解**创建容器     |

#### 1.2 Bean

##### 1.2.1 生命周期

- **单例对象:** 容器创建时创建, 没有外部引用的话容器销毁时释放, 否则容器销毁后由GC回收;
  - 即只要容器在，对象就在；
- **多例对象:** 使用对象时创建, GC回收;

##### 1.2.2 bean标签属性

- `id`：bean的唯一标识；
- `class`：bean的全类名；
  - 使用静态工厂方法创建对象时，指定静态工厂类；

- `scope`：控制单例或者多例；

| 名称      | 描述                    |
| :-------- | :---------------------- |
| singleton | 单例（默认）            |
| prototype | 多例                    |
| request   | 作用于web应用的请求范围 |
| session   | 作用于web应用的会话范围 |

- `init-method`、`destroy-method`：指定初始化、销毁时执行的方法；
- `factory-method`：使用某静态工厂的方法创建对象；
- `factory-bean`：当把工厂类交给spring管理时，使用该标签指定工厂id，再使用factory-method指定工厂中的方法。

### 2. 依赖注入

> 上面的测试用例会发现打印出来都是null，因为默认调用的是无参构造函数，接下来就要开始注入灵魂。

#### 2.1 构造函数注入

```xml
<bean id="student" class="site.henrykang.domain.Student">
    <constructor-arg name="stu_id" value="1003"></constructor-arg>
    <constructor-arg name="stu_name" value="张三"></constructor-arg>
    <constructor-arg name="stu_gender" value="男"></constructor-arg>
    <constructor-arg name="stu_birthday" ref="newDate"></constructor-arg>
</bean>
<!-- 配置一个日期对象加入容器 -->
<bean id="newDate" class="java.util.Date"></bean>
```

- `constructor-arg`属性说明

| 属性  | 说明                                         |
| ----- | -------------------------------------------- |
| name  | 构造函数参数列表中对应参数的名称（推荐）     |
| value | 仅能赋值基本型和String型                     |
| ref   | 赋值其他由 spring 管理的 bean 类型的 id      |
| index | 指定在构造函数参数列表中的索引位置（不推荐） |
| type  | 指定在构造函数参数列表中的数据类型（不推荐） |

#### 2.2 set方法注入

> 和上面大同小异，就标签名不同，属性只有 value 和 ref。

```xml
<bean id="student" class="site.henrykang.domain.Student">
    <property name="stu_id" value="1003"></property>
    <property name="stu_name" value="张三"></property>
    <property name="stu_gender" value="男"></property>
    <property name="stu_birthday" ref="newDate"></property>
</bean>
<bean id="newDate" class="java.util.Date"></bean>
```

#### 2.3 P名称空间注入

> 通过在 bean 标签内使用属性 p:propertyName 注入，本质还是set方法。
>
> 发现报红的话是因为没导入p标签的约束：
>
> ​	xmlns:p="http://www.springframework.org/schema/p"

```xml
<bean id="student" class="site.henrykang.domain.Student" 
      p:stu_id="1003" 
      p:stu_name="张三" 
      p:stu_gender="男" 
      p:stu_birthday-ref="newDate">
</bean>
<bean id="newDate" class="java.util.Date"></bean>
```



#### 2.4 集合类型注入

> 主要分两大类，同一大类的标签可以互用。

- List型使用`<list>`、`<array>`、`<set>`标签与`<value>`、`<ref>`配合;
- Map型使用`<map>`、`<props>`标签与`<entry>`、`<prop>`及`key(-ref)`、`value(-ref)`配合;

```xml
<bean id="student" class="site.henrykang.domain.Student">
	<property name="hobby">
		<set>
			<value>唱</value>
			<value>跳</value>
			<value>rap</value>
		</set>
	</property>
	
	<property name="career">
		<map>
			<entry key="2017" value="大一"></entry>
			<entry key-ref="nowDate" value-ref="xx"></entry>
		</map>
		<!-- 或 -->
		<props>
			<prop key="2017">
            	<value>大一</value>
            </prop>
			<prop key="2018">大二</prop>
		</props>
	</property>
</bean>
<bean id="nowDate" class="java.util.Date"></bean>
```

### 3. IOC注解开发

- 约束文件

```xml 注解开发的xml约束 >folded
<!-- IoC注解配置约束 -->
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
</beans>
```

- 在配置文件中告诉Spring要扫描的包

```xml
<context:component-scan base-package="site.henrykang"></context:component-scan>
```

#### 3.1 常用注解

1. `@Component` 相当于 `<bean>`

- 默认id为当前类名(首字母小写), 也可以通过value属性修改;
- 三个子类, 作用一样, 只是为了方便标识
  - 表现层: `@Controller`
  - 业务层: `@Service`
  - 持久层: `@Repository`

2. `@Autowired`相当于`<property>`

- 过程：在容器中，先根据类型查找, 有唯一匹配的注入成功; 若有多个类型匹配, 则再进一步根据变量名称查找, 找到与变量名相同的的beanId则注入; 其他情况都会报错.
- 使用`@Qualifier`与`@resource`解决上述问题:
  - @Qualifier("id") 要与@Autowired配合使用, 指明要注入的BeanId;
    - 在方法上可以单独使用;
  - @Resource(**name**="id") 可以单独使用, 作用也是指明要注入的BeanId;
    - 注意使用的是@Resource的name属性;
- 使用`@Value`注入基本型和String型数据;
  - 配置了@PropertySource注解的时候可以使用使用SPEL表达式：@Value("${jdbc.driver}")；

3. `@Scope` 指定bean的作用范围；

4. `@PreDestroy`与`@PostConstruct` 指定销毁与初始化方法。

#### 3.2 全注解配置

> 获取容器时需要使用AnnotationApplicationContext(注解配置类.class)。

1. `@Configuration`：指定当前类是一个配置类；
2. `@ComponentScan`：指定创建容器时要扫描的包；
3. `@Bean`：只能写在方法上，表示将当前方法的返回值作为bean存入容器；
   - name属性指定beanId，默认为方法名称；
4. `@Import`：导入其他配置类，value是配置类.class；
5. `@PropertySource`：指定要使用的properties文件位置；
   - 类路径下要写上前面`classpath:`

```java
@Configuration
@ComponentScan("site.henrykang")
@PropertySource("classpath:jdbc.properties")
public class SpringConfiguration {
	//    ...
}
```



#### 3.3 整合Junit注解

> 　　在执行测试方法时，JUnit根本不知道我们是不是使用了Spring框架，所以也就不会为我们读取配置文件、配置类去创建Spring核心容器，没有了容器，就算写了Autowired注解，也无法实现注入。
>
> 使用 spring 5.x版本的时候，要求 JUnit 的jar必须是4.12以上。

1. 导入jar包

```xml >folded
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-test</artifactId>
	<version>5.0.2.RELEASE</version>
    <scope>test</scope>
</dependency>
```

2. 测试类添加下面两个注解

  - @RunWith(SpringJUnit4ClassRunner.class)：声明替换程序入口为spring提供的；

  - @ContextConfiguration

    - @ContextConfiguration(classes = SpringConfiguration.class)：指定注解配置类的位置；

    - @ContextConfiguration(locations = "classpath: bean.xml")：指定xml配置文件的位置。

### 4. AOP

> Aspect Oriented Programming 面向切面编程。

#### 4.0 为什么要AOP

**了解一些名词：**

| 名称        | 描述                                                         |
| :---------- | :----------------------------------------------------------- |
| Joinpoint   | 连接点：程序运行过程中的某一点                               |
| Pointcut    | 切入点：一组我们要拦截切分的连接点                           |
| Advice      | 通　知：拦截之后要做的增强                                   |
| Target      | 目　标：目标对象就是我们需要增强的业务对象                   |
| Proxy       | 代　理：为增强目标对象而产生的一个新的代理对象               |
| **Aspect**  | 切　面：将 Advice 抽取出来形成的一个模块                     |
| **Weaving** | 织　入：将 Aspect 加入到(拦截器)方法中为对象增加额外功能的过程 |

**说明：**

　　Spring的AOP其实就是**让Spring帮我们完成动态代理**的过程。我们都知道想要对原方法进行增强而又不能修改其源码的时候可以选择使用动态代理，而AOP其实不光是为了增强而存在的，它更是为了将一些重复的业务逻辑代码进行抽取形成一个模块（即**切面**），在需要的地方进行复用（即**织入**）。而有的人就要问了，抽取重复代码不是抽取出一个公共方法就可以了吗？有些时候抽取一个公共方法出来其实并不是那么的容易，而且别忘了使用SpringAOP同时还达到了**解耦**的目的。

**一个关于数据库事务控制的例子：**

```java
//xxxServiceImpl的伪码
doSth1(){
	beginTransaction();
	doSth1...
    if success then commit();
    else rollback();
    finally endTransaction();
}
doSth2(){
	beginTransaction();
	doSth2...
    if success then commit();
    else rollback();
    finally endTransaction();
}
doSth3(){...}
```

　　会发现相同的业务逻辑造成了大量的重复代码，而且因为每次doSth都不同，所以不好抽取重复代码；不仅如此，试想一下假设beginTransaction()变成了startTransaction()，那使用到它的每一个地方都要更改，这种耦合可以想办法消除一下，为此我们就可以想到使用动态代理。

#### 4.1 动态代理

##### 4.1.1 Proxy

```java
public class Client {
	public static void main(String[] args) {
		final Producer producer = new Producer();

		IProducer proxyProducer = (IProducer) Proxy.newProxyInstance(producer.getClass().getClassLoader(),
		producer.getClass().getInterfaces(),
		new InvocationHandler() {
		/**
		* 作用：执行被代理对象的任何接口方法都会经过该方法
		* 方法参数的含义
		* @param proxy   代理对象的引用
		* @param method  当前执行的方法
		* @param args    当前执行方法所需的参数
		* @return        和被代理对象方法有相同的返回值
		* @throws Throwable
		*/
			@Override
			public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
				//提供增强的代码
				Object returnValue = null;
				//1.获取方法执行的参数
				Float money = (Float)args[0];
				//2.判断当前方法是不是销售
				if("saleProduct".equals(method.getName())) {
					returnValue = method.invoke(producer, money*0.8f);
				}
				return returnValue;
			}
		});
		proxyProducer.saleProduct(10000f); //输出 8000
	}
}
```

- 基于接口的代理：**被代理类最少实现一个接口**
- `newProxyInstance()`方法的参数：
  - ClassLoader：类加载器，用于加载代理对象字节码的，和被代理对象使用相同的类加载器；
  - Interfaces：和被代理对象具有相同的行为，实现相同的接口；
  - InvocationHandler：用于提供增强的代码。

##### 4.1.2 cglib

```java
public class Client {
	 public static void main(String[] args) {
        final Producer producer = new Producer(); 
        Producer cglibProducer = (Producer)Enhancer.create(producer.getClass(), 
			new MethodInterceptor() {
            /**
             * 执行被代理对象的任何方法都会经过该方法
             * @param proxy
             * @param method
             * @param args
             *    以上三个参数和基于接口的动态代理中invoke方法的参数是一样的
             * @param methodProxy ：当前执行方法的代理对象
             * @return
             * @throws Throwable
             */
            @Override
            public Object intercept(Object proxy, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {
                //提供增强的代码
                Object returnValue = null;
                //1.获取方法执行的参数
                Float money = (Float)args[0];
                //2.判断当前方法是不是销售
                if("saleProduct".equals(method.getName())) {
                    returnValue = method.invoke(producer, money*0.8f);
                }
                return returnValue;
            }
        });
        cglibProducer.saleProduct(12000f);
    }
}	
```

- 基于子类的代理：**被代理类不能是最终类**
- `create()`方法的参数：
  - Class：字节码，用于指定被代理对象的字节码；
  - Callback：用于提供增强的代码，一般写其实现类MethodInterceptor。

#### 4.2 AOP的XML配置

　　使用动态代理解决了我们的麻烦，但也得手工的为一个一个的被代理类制定增强规则，即便是大量的被代理类具有相同的增强规则，例如记录日志的工作。这时就轮到SpringAOP出场了，它将相同的增强规则抽取为一个**切面**，在**切入点**进行**织入**。

##### 4.2.1 基本步骤

```xml bean.xml约束 >folded
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">
</beans>
```

0. 导入aspectjweaver.jar

1. 将增强Bean交由Spring容器;
2. `<aop:config>`声明aop配置;
3. `<aop:aspect>`声明切面, 属性:
   - id，切面id
   - ref，增强BeanId
4. 在`<aop:aspect>`内部织入
   - `<aop:before>`表示前置增强
     - method：指定使用增强Bean中的哪个方法作为增强方法
     - pointcut：指定**切入点**
   - 切入点表达式写法: execution(表达式)
     - 格式：`访问修饰符  返回值  包名...类名.方法名(参数列表)`
     - 访问修饰符可以省略，返回值、包名、类名、方法名可以使用通配符`*`；
     - 几层包就要写几个`*`, 可以使用`..`来表示当前包及其子包；
     - 参数列表写法：
       - 基本型直接写，引用型写全类名；
       - 通配符`*`表示至少一个参数， `..`表示任意个参数；
     - 全通配写法: `* *..*.*(..)`

```xml
<!-- 配置增强Logger类 -->
<bean id="logger" class="com.istudy.utils.Logger"></bean>
<!--配置AOP-->
<aop:config>
<!--配置切面 -->
	<aop:aspect id="logAdvice" ref="logger">
		<!-- 配置增强的类型，并且建立增强方法和切入点方法的关联-->
		<aop:before
method="printLog"pointcut="execution(*com.istudy.service.impl.*.*(..))">		</aop:before>
	</aop:aspect>
</aop:config>
```

##### 4.2.2 几类通知（4+1）

```xml
<aop:config>
<!-- 配置切入点在切面内, 只能供一个切面使用 -->
<!-- 配置切入点在切面外, 可以使所有切面共用, 但是必须写在切面前面 -->
	<aop:pointcut id="pt1" expression="execution(* com.istudy.service.impl.*.*(..))"></aop:pointcut>
	<!--配置切面 -->
	<aop:aspect id="logAdvice" ref="logger">
		<!-- 配置前置增强：在切入点方法执行之前执行-->
		<aop:before method="beforePrintLog" pointcut-ref="pt1" ></aop:before>
		
		<!-- 配置后置增强：在切入点方法正常执行之后 -->
		<aop:after-returning method="afterReturningPrintLog" pointcut-ref="pt1"></aop:after-returning>

		<!-- 配置异常增强：在切入点方法执行产生异常之后执行 -->
		<aop:after-throwing method="afterThrowingPrintLog" pointcut-ref="pt1"></aop:after-throwing>
		
		<!-- 配置最终增强：无论切入点方法是否正常执行它都会在其后面执行 -->
		<aop:after method="afterPrintLog" pointcut-ref="pt1"></aop:after>

		<!-- 配置环绕增强 -->
		<aop:around method="aroundPrintLog" pointcut-ref="pt1"></aop:around>
	</aop:aspect>
</aop:config>
```

- 环绕增强需要特别明确切入点的调用 

```java
public Object aroundPringLog(ProceedingJoinPoint pjp){
	Object rtValue = null;
	try{
		Object[] args = pjp.getArgs();//得到方法执行所需的参数

		System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。前置");

		rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）

		System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。后置");

		return rtValue;
	}catch (Throwable t){
		System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。异常");
		throw new RuntimeException(t);
	}finally {
		System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。最终");
	}
}
```

#### 4.3 AOP的注解配置

```xml bean.xml约束 >folded
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
</beans>

<!-- 配置spring创建容器时要扫描的包-->
<context:component-scan base-package="com.istudy"></context:component-scan>

<!-- 配置spring开启注解AOP的支持 -->
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

| 注解                                       | 描述                                                         |
| :----------------------------------------- | :----------------------------------------------------------- |
| @Aspect                                    | 表示当前类是一个切面类                                       |
| @Pointcut<br />private void pt1(){}        | 配置切入点表达式，<br />在一个方法上面声明，方法名就是表达式id |
| `@Before("切入点表达式()") `方法括号必须带 | 在切入点方法执行之前执行                                     |
| @AfterReturning("pt1()")                   | 在切入点方法正常执行之后执行                                 |
| @AfterThrowing("pt1()")                    | 在切入点方法发生异常后执行                                   |
| @After("pt1()")                            | 在切入点方法执行之后执行                                     |
| @Around("pt1()")                           | 环绕增强                                                     |

**注意：**

　　当使用@Before、@After...四个注解进行增强配置时，**最终通知会在后置/异常通知前执行**；而使用@Around环绕增强时，因为我们显示的声明了何时调用切入点方法，所以不会出现这个问题。

　　不使用bean.xml，即全注解配置时，需要在配置类前加上注解：`@EnableAspectJAutoProxy`。

#### 4.4 Spring 中的事务控制

> 事务控制很重要，而且也非常能体现AOP的特点，Spring当然也为我们提供了事务控制的相关方法。

- 导包或坐标：`spring-tx...`、`spring-jdbc...`
- tx 相关的约束：`xmlns:tx="http://www.springframework.org/schema/tx"`

##### 4.4.1 相关的类与接口

- `PlatformTransactionManager`接口：事务管理器

  - getTransaction(**TransactionDefinition** definititon)

  - getTransaction(**TransactionStatus** status)

  - rollback(TransactionStatus status)

  - 我们使用Spring为我们提供好的实现类：

    `org.springframework.jdbc.datasource.DataSourceTransactionManager`

- `TransactionDefinition`接口：定义事务

  - getPropagationBehavior()：获取事务传播行为
    - PROPAGATION_REQUIRED：默认，如果当前没有事务就创建一个，若有则加入该事务；
    - PROPAGATION_SUPPORTS：支持当前事务，若没有事务就以非事务的方式执行；
    - PROPAGATION_MANDATORY：使用当前的事务，若当前没有事务，抛异常；
    - PROPAGATION_REQUIRES_NEW：挂起当前事务，新建一个；
    - PROPAGATION_NOT_SUPPORTED：挂起当前事务，以非事务的方式执行下面的操作；
    - PROPAGATION_NEVER：以非事务的方式运行，若当前有事务，抛异常；
    - PROPAGATION_NESTED：嵌套事务执行；
  - getIsolationLevel()：获取事务隔离级别
    - ISOLATION_DEFAULT = -1：默认与所用数据库隔离级别一致；
    - ISOLATION_READ_UNCOMMITTED：读未提交；
    - ISOLATION_READ_COMMITTED：读已提交，解决脏读；
    - ISOLATION_REPEATABLE_READ：可重复度，解决不可重复读，MySQL默认；
    - ISOLATION_SERIALIZABLE：可串行化，解决幻读；
  - getTimeout()：获取事务超时时间
    - int 秒值，默认 -1 永不超时；
  - isReadOnly()：事务是否只读
  - getName()：获取事务对象名称

- `TransactionStatus`接口：获取事务状态

  - isNewTransaction()：是否为新事务；
  - hasSavepoint()：是否存在存储点；
  - setRollbackOnly()：设置事务回滚；
  - isRollbackOnly()：是否回滚；
  - flush()：刷新事务；
  - isCompleted()：是否完成；

##### 4.4.2 XML配置步骤

```xml
<!-- 1.配置事务管理器，相当于增强Bean -->
<bean  id="transactionManager"
class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<property name="dataSource" ref="dataSource"></property>
</bean>

<!-- 2.配置事务增强和增强Bean绑定 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <!-- 3.配置事务属性：
		- read-only：是否只读，默认false；
		- isolation：隔离级别，默认和数据库一致；
		- propagation：传播行为；
		- timeout：超时时间；
		- rollback-for：指定一个异常，当其发生时，事务回滚；产生其他异常时，不回滚；
		- no-rollback-for：和上面相反，指定一个异常，只有当该异常发生时，事务才回滚；
	-->
    <tx:attributes>
    	<tx:method name="*" read-only="false" propagation="REQUIRED"/>
		<tx:method name="find*" read-only="true" propagation="SUPPORTS"/>
    </tx:attributes>
</tx:advice>

<!-- 4.配置AOP -->
<aop:config>
	<!-- 配置切入点表达式 -->
	<aop:pointcut  expression="execution(*  com.istudy.service.impl.*.*(..))"
id="pt1"/>
    <!-- 将事务的配置与切入点绑定 -->
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pt1"/>
</aop:config>
```



