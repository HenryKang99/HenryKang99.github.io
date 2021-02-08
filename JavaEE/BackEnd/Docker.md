### 0verview

> 　　Docker 是一个运行在Linux上的轻量级、可移植的**容器**，它允许我们根据镜像创建一个容器来运行我们的应用程序。

　　为什么要使用容器？容器最大的作用就是便于隔离、扩展与移植。隔离文件资源、硬件资源、网络资源等等。Docker允许我们可以方便的实时扩展或删除应用程序与服务，可以使用`Dockerfile`编辑镜像实现快速的修改移植。

　　我们应该都有过这样的体验，自己编译调试写好的代码，把工程文件拿到其他地方去运行有时候总会出现莫名其妙的问题。多数情况下可能都是软件版本的差异造成的不兼容，这种情况还比较好处理，而有时候可能就是系统环境导致的不兼容。试想一下开发过程中还要涉及到很多其他环节和人员，很容易出现彼此环境不一致的情况，而使用Docker就能很好地解决这个问题，开发测试等各环节的环境保持一致，大大减少了交流成本。

　　我们可能还会想到使用虚拟机，那么VM与容器的区别在哪里？如图：
![虚拟机 vs 容器](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/gef9KxVWdRfg.png)

　　我们知道操作系统包括内核与系统调用，内核负责管理硬件资源，而系统调用相当于是对外提供的接口。**左图中**：VM 在虚拟化的时候，也会对硬件资源进行虚拟化，每一个Guest OS都在Host OS上虚拟出了各自的内核与硬件资源。而Guest OS 在运行当中还是要依赖于虚拟机软件与Host OS，这么来看的话是不是虚拟就像是多下了一道功夫一样，为什么不直接申请使用Host OS的资源呢。容器就是这样做的；**右图中**：可以看到只有一个Host OS，容器相当于是一个进程，它可以直接申请使用Host OS的资源。简言之就是VM是在宿主机操作系统上又虚拟了一层操作系统，而Docker则是直接利用宿主机操作系统的资源。

### 1. Hello Docker

> 环境 CentOS 7.x

- 更新 yum 包

``` bash
sudo yum update 
```

- 安装依赖的包

``` bash
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

- 设置 yum源为aliyun

``` bash
sudo yum-config-manager  --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

- 安装docker

``` bash
sudo yum install docker-ce
```

- 验证安装

``` bash
docker -v
```

- 设置Docker镜像下载地址

``` bash
vi /etc/docker/daemon.json
添加如下内容：
{
"registry-mirrors": ["https://docker.mirrors.ustc.edu.cn"]
}
```

- 启动和停止Docker

``` bash
sudo systemctl start docker
sudo systemctl stop docker
sudo systemctl restart docker
自启动：sudo systemctl enable docker
```

### 2. 常用命令

#### 2.1 镜像

| 命令                                             | 说明                                                         |
| :----------------------------------------------- | :----------------------------------------------------------- |
| docker images                                    | 查看镜像                                                     |
| docker search centos                             | 搜索镜像                                                     |
| docker pull id                                   | 拉取镜像                                                     |
| docker rmi id<br>docker rmi \`docker images -q\` | 删除一个镜像<br>删除所有镜像<br />无法删除有实例正在运行的镜像 |

#### 2.2 容器

| 命令                                                         | 说明                                                         |
| :----------------------------------------------------------- | :----------------------------------------------------------- |
| docker ps<br/>docker ps -a                                   | 查看正在运行<br/>查看所有容器                                |
| docker run<br/>参数：<br/>　　-i<br/>　　-t<br />　　--name<br />　　-v 宿主机目录:容器目录<br />　　-d<br />　　-p 宿主机端口:容器端口 | 创建容器<br/><br/>　　创建时运行容器<br />　　容器启动后进入命令行<br />　　容器命名<br />　　建立目录映射关系<br />　　创建一个守护式容器在后台运行<br />　　建立端口映射 |
| docker start id<br/>docker stop id                           | 启动与停止容器                                               |
| docker inspect id                                            | 查看容器信息                                                 |
| docker rm id                                                 | 删除容器<br />无法删除正在运行的容器                         |
| docker cp source dest                                        | 文件拷贝                                                     |

**举例：**

- 交互创建：exit后自动停止

```bash
docker run -it --name=mycentos centos:7 /bin/bash
```

- 守护创建：exit后不会停止

```bash
docker run -di --name=mycentos2 centos:7
docker exec -it mycentos2 /bin/bash
```

- 目录与端口映射：

```bash
docker run -di -v /user/local/vtest:/user/local/vtest --name=mycentos3 centos:7
docker run -di --name=my_redis -p 6379:6379 redis
```

- 文件拷贝

```bash
docker cp test.conf mycentos:/usr/local
docker cp mycentos/usr/local/test.conf test.conf
```


#### 2.3 备份与恢复

- 容器 --> 镜像

```bash
docker commit mycentos mycentos_2
```

- 镜像备份

```bash
docker save -o mycentos_3.tar mycentos_2
```

- 镜像恢复

```bash
docker load -i mycentos_3.tar
```



#### 2.4 常用环境部署

1. MySQL部署

   ```bash
   拉取镜像 docker pull centos/mysql-57-centos7
   创建容器 docker run -di --name=my_mysql -p 33306:3306 -e MYSQL_ROOT_PASSWORD=root centos/mysql-57-centos7
   ```

2. tomcat部署

   ```bash
   docker pull tomcat:7-jre7
   docker run -di --name=my_tomcat -p 9090:8080 -v /user/local/webapps:/user/local/tomcat/webapps tomcat:7-jre7
   ```

3. nginx部署

   ```bash
   docker pull nginx
   docker run -di --name=my_nginx -p 80:80 nginx
   ```

4. redis部署

   ```bash
   docker pull redis
   docker run -di --name=my_redis -p 6379:6379 redis
   ```

   

### 3. Dockerfile

>  是一系列命令和参数构成的脚本。



**常用参数：**

| 参数                 | 说明                                |
| -------------------- | ----------------------------------- |
| FROM image_name:tag  | 定义使用哪个基础镜像启动构建流程    |
| MAINTAINER user_name | 声明镜像的创建者                    |
| ENV key value        | 设置环境变量                        |
| RUN command          | dockerfile核心部分                  |
| ADD source dest      | 将宿主机文件复制到容器内,会自动解压 |
| COPY source dest     | 和ADD类似,但不会自动解压            |
| WORKDIR path_dir     | 设置工作目录                        |

- **部署JDK8为例**

1. 创建一个文件夹并把 jdk8 压缩包拷贝进去

  ```bash
mkdir -p /usr/local/dockerjdk8
  ```

2. 创建一个名为Dockerfile的文件

```bash
vi Dockerfile
```

3. 开始输入命令：

```bash
FROM centos7 指定基础镜像
MAINTAINER khy 指定镜像的创建者
WORKDIR /usr 设置工作目录
RUN mkdir /usr/local/java
ADD jar包 /usr/local/java/

配环境变量：
	ENV JAVA_HOME /usr/local/java/jdkbalabala
	ENV JRE_HOME $JAVA_HOME/jre
	ENV CLASSPATH $JAVA_HOME/bin.dt.jar:$JAVA_HOME/lib/tools.jar: $JAVA_HOME/lib:$CLASSPATH
	ENV PATH $JAVA_HOME/bin:$PATH

开始构建：
docker build -t='jdk1.8' . 
	-t 指定镜像名称，`.` 代表指定Dockerfile在当前文件夹
使用docker images命令验证是否成功
```




### 4. 私有仓库

> 仓库本身也是一个镜像

- 使用步骤

1. 拉取镜像

   ```bash
   docker pull registry
   ```

2. 启动仓库容器

   ```bash
   docker run -di --name=myregistry -p 5000:5000
   ```

3. 访问端口下的 /v2/_catalog 能看到一个json串则成功

4. 修改配置文件daemon.json让docker信任私有仓库地址

  ```bash
vi etc/docker/daemon.json 
添加一段配置：
"insecure-registries":["ip:端口号(5000)"]
  ```

5. 重启服务 

```bash
systemctl restart docker
```

6. 将镜像上传到私有仓库

```
docker tag jdk1.8 ip:端口号/jdk1.8
docker push ip:端口号/jdk1.8
访问端口下/v2/_catalog 验证
```

7. 从仓库下载
   首先在daemon.json中添加信任，然后

  ```bash
docker pull ip:端口号/jdk1.8
  ```

  