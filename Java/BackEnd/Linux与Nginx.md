### 0verview

> Linux常用命令；部署项目到Tomcat；Nginx部署静态资源，负载均衡。

### 1. Linux 常用命令

[菜鸟教程Linux命令大全](https://www.runoob.com/linux/linux-command-manual.html)

#### 1.0 Linux目录结构

> [摘自菜鸟教程](https://www.runoob.com/linux/linux-system-contents.html)
>
> - **/boot：**存放的是启动Linux时使用的一些核心文件，包括一些连接文件以及镜像文件；
>
> - **/etc：**存放所有的系统管理所需要的配置文件和子目录；
>
> - **/lib**：存放着系统最基本的动态连接共享库，其作用类似于Windows里的DLL文件。几乎所有的应用程序都需要用到这些共享库；
>
> - **/bin：**存放着最常用的程序和指令；
>
> - **/sbin：**只有系统管理员能使用的程序和指令；
>
> - **/dev ：**Device(设备)的缩写, 存放的是Linux的外部设备；
>
> - **/media**：linux系统会自动识别一些设备，例如U盘、光驱等等，当识别后，linux会把识别的设备挂载到这个目录下；
>
> - **/mnt**：系统提供该目录是为了让用户临时挂载别的文件系统的，我们可以将光驱挂载在/mnt/上，然后进入该目录就可以查看光驱里的内容了；
>
> - **/run**：是一个临时文件系统，存储系统启动以来的信息。当系统重启时，这个目录下的文件应该被删掉或清除；
>
> - **/lost+found**：一般情况下为空的，系统非法关机后，这里就存放一些文件；
>
> - **/tmp**：存放一些临时文件；
>
> - **/root**：系统管理员的用户主目录；
>
> - **/home**：用户的主目录，以用户的账号命名；
>
> - **/usr**：用户的很多应用程序和文件都放在这个目录下，类似于windows下的program files目录；
>
> - **/usr/bin：**系统用户使用的应用程序与指令；
>
> - **/usr/sbin：**超级用户使用的比较高级的管理程序和系统守护程序；
>
> - **/usr/src：**内核源代码默认的放置目录；
>
> - **/var**：存放经常被修改的数据，比如程序运行的日志文件（/var/log 目录下）；
>
> - **/proc**、**/sys**：是虚拟的目录，它是系统内存的映射，我们可以通过直接访问这个目录来获取系统信息；
>
> - **/opt**：额外安装软件所放的目录；
>
> - **/srv**：存放一些服务启动之后需要提取的数据。

#### 1.1 文件相关操作

- 切换目录

```bash
./  #当前目录
../ #当前父目录
cd path #进入path
cd ..   #返回上一层
cd /    #切换到根目录
cd ~    #切换到当前用户的主目录
pwd #显示当前所在路径
# ...
```

- 列出文件目录

```bash
ls    #列出该目录下文件
ls -a #列出该目录下所有文件，包括隐藏文件（文件名以 '.' 开头）
ls -l #列出该目录下文件的详细信息 缩写为ll
du –h #友好显示当前目录的大小
```

- 创建、移除文件夹

```bash
mkdir dirName #创建文件夹
mkdir -p dirName1/dirName2 #创建文件夹且自动创建父目录
rmdir  dirName #删除空的文件夹
rm -r dirName  #删除非空文件夹 -r意思是递归删除
```

- 打开文件

```bash
cat fileName  #显示文件内容
more fileName #显示长文件内容
less fileName #同上
tail fileName #默认显示最后几行
tail -10 fileName #显示最后10行
tail -f fileName  #动态查看文件（文件更新会自动变化）
```

- 文件操作

```bash
touch fileName #创建一个空文件

# ---删除操作---
rm fileName    #删除文件
rm -f fileName #直接删除文件不询问
rm -rf *       #清理缓存（滑稽） 

# ---复制剪切---
cp a.txt aaa/b.txt #复制a.txt到aaa路径下命名为b.txt
mv a.txt aaa/b.txt #剪切a.txt到aaa路径下命名为b.txt

# ---打包解压---
  # linux 中打包文件以.tar为后缀，压缩文件以.gz结尾
  #参数：
    #-c 创建一个新的tar文件 
    #-x 解开tar文件
    #-v 显示过程信息
    #-f 指定文件名
    #-z 调用gzip进行压缩

tar –cvf a.tar afile	 #打包afile文件为a.tar
tar –zcvf a.tar.gz afile #压缩afile文件为a.tar.gz

tar –xvf a.tar 	             #解包a.tar
tar –zxvf a.tar.gz  -C /path #解压缩a.tar.gz到指定位置
```

- 搜索查找

```bash
find . -name "*.c"     #查找当前目录下name以.c结尾的文件
grep xxx a.txt --color #在a.txt文件中查找xxx并高亮显示
```

- 重定向输出

```bash
cat a.log > b.txt  #将输出覆盖到b.txt
cat a.log >> b.txt #将输出追加到b.txt
```

- 管道

```bash
# 将一个命令的输出作为下一个命令的输入
cat index.html | more
```



#### 1.2 用户和文件权限相关

- 用户创建、删除、切换

```bash
who #显示当前登录的用户信息
useradd foo               #添加foo用户
groupadd test			  #创建一个名为test的组
groupdel test			  #删除test组，必须先删除其中的用户
useradd foo –g test       #创建用户并指定组
useradd foo -d /home/foo  #指定用户home目录
passwd foo                #为用户foo设置密码

su foo          #切换到foo用户
su - root       #切换用户，连同环境一起切换
userdel foo     #删除foo用户
userdel –r test #删除foo用户及其用户目录
sudo xxx     #以管理员权限执行命令
```

- 文件权限

```bash
# - --- --- --- 10字符分为四组
# d rwd rwd rwd 
  #第一组表示文件类型：
    #- 表示普通文件
    #d 表示文件夹
    #l 表示链接
    #...
  #其他三组分别表示当前用户权限，当前用户所属组的权限，其他用户的权限
    #r 可读　  4
    #w 可写　  2
    #d 可执行  1
    
# 改变文件权限：
chmod u=rwx,g=rx,o=rx a.txt
chmod 755 a.txt 
```



#### 1.3 其他命令

- 网络相关

```bash
ifconfig  #显示网络设备
netstat -an | grep 3306 #查询3306端口占用情况
```

- 系统相关

```bash
df   #显示磁盘信息
free #显示内存状态
top  #显示，管理执行中的程序
ps –ef  #查看所有进程
kill -9 3306  #强制杀死3306进程
hostname #查看当前主机名，在/etc/sysconfig/network中修改
uname -a #显示本机详细信息 
```



---

### 2. Linux 软件安装

#### 2.1 文件传输

- 使用SecureFX从左到右从未失手：

![mark](D:/OneDrive/_mine/docsify/_img/P8E3wEaeAqcq.png)



#### 2.2 软件安装

> 针对不同的情况有 rpm、yum、解压缩、手动编译等方式……
>
> rpm 不会自动安装依赖；
>
> yum会自动安装所需依赖；
>
> 有些软件已经是一个二进制包了，我们解压缩即可；
>
> 有些软件只会提供源码，所以需要我们手动编译打包。

##### 1. 安装 JDK

> 这个属于直接解压缩方式

- 先把自带的OpenJdk干掉

```bash
rpm -qa | grep java     # 查询所有和java有关的软件
rpm -e --nodeps xxx xxx #xxx 把要卸载的名字都粘过来
```

- 解压缩后记住路径，在/etc/profile目录下添加环境变量

```bash
#set java environment
JAVA_HOME=/usr/local/src/java/jdk1.8.0_251
CLASSPATH=.:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
```

- 执行命令使配置生效，验证是否成功

```bash
source /etc/profile
java -version
```

![mark](D:/OneDrive/_mine/docsify/_img/6QUjTl71UuoF.png)

##### 2. 安装 MySQL

> 这次使用rpm方式，下载相应的rpm包，需要安装server端和client端

- 检查卸载自带的mysql

```bash
 rpm -qa | grep mysql # mariadb
 rpm -e --nodeps xxx
 # CentOS 7 要删除mariadb，不然有可能会有问题，坑。 
```

- 安装并启动服务、修改root用户密码

```bash
service mysql start
mysqladmin -u root password 'new-password'
mysql -uroot -p pwd #成功
```

- 远程连接问题

```mysql
# 授权可以远程链接
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%'  WITH GRANT OPTION;
FLUSH PRIVILEGES;
```

##### 3. 安装 Tomcat

- 开启./startup.sh，访问8080端口看能不能出来Tomcat

- 访问不了的话要关闭防火墙

```bash
# CentOS 7 下关闭防火墙并关闭自启
systemctl stop firewalld.service 
systemctl disable firewalld.service
```

##### 4. 安装 redis

> 编译安装方式

- 安装

```bash
yum install gcc-c++ #安装gcc
wget http://download.redis.io/releases/redis-3.0.4.tar.gz #获取redis
tar -zxvf redis-3.0.4.tar.gz #解压
cd redis-3.0.4 #进入目录
make #编译
make PREFIX=/usr/local/src/redis install#安装到指定目录
```

- 启动

```bash
# 启动server
./redis-server redis.conf
# 切换一个窗口启动client
./redis-cli
```

##### 5. 安装nginx

> 源码编译安装，但是要先Makefile

```bash
# 一些依赖环境
yum install gcc-c++
yum install -y pcre pcre-devel
yum install -y zlib zlib-devel
yum install -y openssl openssl-devel

# 解压缩Nginx包后cd进去执行下面的命令创建Makefile
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi

# 编译安装
make #编译
make install#安装到指定目录
mkdir /var/temp/nginx/client #创建工作目录

# 启动，访问80端口验证
./nginx
./nginx -s stop #强制关闭
./nginx -s quit #正常退出
./nginx -s reload #重新加载配置文件
```



---

### 3. 部署项目到Tomcat

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

---

### 4. Nginx

#### 4.1 部署静态资源

- 核心配置文件 `conf/nginx.conf`

![mark](D:/OneDrive/_mine/docsify/_img/YtsLd228K1Me.png)

```bash
server {
    listen       80;		#监听的端口
    server_name  localhost; #监听的域名

    location / {			
        root   html;		#资源路径，相对路径为nginx/
        index  index.html index.htm;#响应的资源
    }
    
    error_page 500 502 503 504 /50x.html;
    location = /50x.html{
    	root html;
    }
}
```



#### 4.2 负载均衡

> 正向代理隐藏了真正的客户端，反向代理隐藏了真正的服务端。

```bash
upstream uniqueName{ # 名称随意
	server ip1:port weight=2;
    server ip2:port;
    server ip3:port;
# weight 默认为1，即上面三个server被访问到的概率为2:1:1
}
server {
    listen       80;		#监听的端口
    server_name  localhost; #监听的域名

    location / {		
        #root   html;		#这个不要了换成下面的一行
        proxy_pass: http://uniqueName;
        index  index.html index.htm;#默认响应的资源
    }
    
    error_page 500 502 503 504 /50x.html;
    location = /50x.html{
    	root html;
    }
}
```

- 几个负载均衡策略：
  - 轮询：默认方式，可以带权重；
  - ip_hash：使用 hash 算法使相同 IP 多次访问可以分配到相同服务器上；
  - fair：第三方，根据后台服务器响应时间动态地调整，响应时间短的优先分配；
  - url_hash：第三方，对访问的url进行hash以分配服务器，适合缓存服务器；
  - sticky session：第三方，根据cookie将同一用户的多次访问分配到同一服务器，和ip_hash相比更加均匀。