### 0verview

>　　__Tomcat__ 是由 Apache 软件基金会下属的 Jakarta 项目开发的一个 Servlet 容器，作为开源免费的一个中小型服务器，只支持少量的servlet、jsp等JavaEE规范。同类产品还有Oracle的webLogic，IBM的webSphere，JBoss的JBoss等，他们都支持所有的JavaEE规范。
>　　JavaEE十三大规范：JDBC、JNDI、EJB、RMI、JavaIDL/CORBA、JSP、Java Servlet、XML、JMS、JTA、JTS、JavaMail、JAF。

### 1. Hello Tomcat

　　安装：下载解压即可，路径下不要有中文。　　

#### 1.1 主要目录

* `/bin` 存放用于启动及关闭的文件，以及其他一些脚本；
* `/conf` 配置文件，其中server.xml是容器的主配置文件；
* `/logs` 日志文件的默认目录；
* `/webapps` 存放 Web 应用；
* `/work`存放临时资源文件（如jsp生成的servlet.java）。

#### 1.2 启动

* __启动:__双击 `./bin/startup.bat` 即可，(`.sh`用于Linux下，相当于win下的`.bat`).  访问`localhost:8080`会显示Tomcat 欢迎界面 . 
* __问题:__
  - 没有正确配置`JAVA_HOME`会导致黑窗口一闪而过；
  - 端口号占用解决：(默认端口号为8080)
    1. 在`/conf/server.xml`中修改关键字`Connector port`为其他值;
    2. cmd使用`netstat -ano`找到占用8080端口的进程,  记住他的`PID`在让任务管理器中停止运行.
  - 为什么通常将web服务器软件端口号设置为`80`?
    答:  因为 _http协议_ 的默认端口号为 80 ,  而浏览器默认请求时地址栏中url后80端口可以省略,  当我们将 tomcat 默认端口设置为 80 后,  访问时就不用在写端口号.
* **关闭:**点击`ctrl +ｃ`或者双击`/bin/shutdown.bat`.

<br>

----

### 2. Tomcat部署方式

1. 直接将项目放到`/webapps`下,  项目名称就是访问的虚拟路径,  通常会将项目打成一个`war`包放置在`/webapps`下.
2. 在`/conf/server.xml`的`<Host>`标签中配置**资源路径**和**虚拟路径**:

```xml
<Context docBase="D:/HelloTomcat" path="/hello" />
```

3. 在`/conf/Catalina/localhost`下创建以**虚拟路径命名**的xml文件,  文件内容为**资源路径**:

```xml
<Context docBase="D:/hello tomcat"/>
```

<br>

----

### 3. IDEA整合Tomcat

- 整合方式:  在IDEA设置中添加本地Tomcat路径即可;
- 需要注意的问题:
  1. IDEA 不使用上面/conf/server.xml中的配置文件,  而是为每一个使用到Tomcat的项目单独建立一份配置文件,  默认存放在`C:\Users\用户名\.IntelliJIdea版本\system\tomcat\项目名`路径下;
  2. 控制台乱码问题：在`tomcat\conf\logging.properties`中修改java.util.logging.ConsoleHandler.encoding = `GBK`
  3. Tomcat **无法直接**访问到项目`WEB-INF`目录下的资源.

<br>

----

### 4. Tomcat运行机制 ?

1. 首先必须实现`servlet`规范;
2. socket监听获取http请求, 封装请求，构造HttpServletRequest;
3. 把获取的用户请求字节流转换成java对象httprequest, 并构造httpResponse;
4. 创建/调用servlet,  调用`inti()`, 调用`service()`;
5. socket写回基于http协议的数据;
6. ......

