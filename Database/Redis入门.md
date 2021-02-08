### 0verview

> 　　Redis（Remote Dictionary Server 远程字典服务） 是一个由C语言编写、开源、支持网络、可持久化、key-value形式的NoSQL数据库。

　　NoSQL（Not Only SQL）意为不仅仅是SQL，泛指非关系型数据库。传统的关系型数据库在互联网行业日新月异的发展下，显示出了一些难以克服的问题，例如海量数据的高效率存储、高并发的读写、高可扩展性等问题，NoSQL就是为了弥补这些问题而诞生的。
　　NoSQL按照其存储数据的格式可以分为：键值对存储、列存储、文档型存储、图形存储。具体可看下图：

![Redis 数据结构](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/EbbtlN0pOJ6f.png)
　　为什么说NoSql是对关系型数据库的弥补，因为他们是相互补充而不是对立的关系。我们从他们各自的优缺点就可以看出来：Sql（关系型数据库）提供对事务的支持，NoSql对事务不友好；Sql 可以实现多表之间的复杂查询，但也意味着表间关系复杂导致不容易扩展，NoSql 中的数据耦合性低非常容易扩展；Sql 读写效率不如NoSql，因为Sql数据存在硬盘中，而NoSql存在主存中，磁盘的IO对读写效率影响非常大。
　　Redis就是属于第一种key-value型，他支持的数据类型包含：String，Hash，List，Set，SortedSet。应用场景：缓存热门内容、排行榜、在线好友列表、任务队列、网站访问统计、数据过期处理、分布式集群架构中session的分离。


### 1. Hello Redis

> 更多命令参考https://www.runoob.com/redis/redis-commands.html 
> 下面只列出常用的
>
> 端口：6379

#### 1.0 Key 类型

- 列出所有符合条件的key ：keys 表达式
  - 例如 keys *  列出所有key
- 删除：del key
- 是否存在：exists key
- 获取某key的value的类型：type key


#### 1.1 String 类型

- 存储： set key value
- 获取： get key
- 删除： del key

#### 1.2 Hash 类型

> 是一个map格式

- 存储：hset key field value
- 获取：hget key field、hgetall key
- 删除：hdel key field1 field2 ...

#### 1.3 List 类型

> 有序可重复

- 存储
  - 存到首位：lpush key value
  - 存到末位：rpush key value
- 获取：lrange key start end
  - 获取所有：lrange key 0 -1
- 弹出
  - 弹出首位：lpop key
  - 弹出末位：rpop key

#### 1.4 Set 类型

> 无序，不可重复

- 存储：sadd key value
- 获取：smembers key
- 删除：srem key value

#### 1.5 SortedSet 类型

> 有序，不可重复，每一项都会关联一个double型的分数，通过分数从小到大排序

- 存储：zadd key score value
- 获取：srange key start end (withscores)
  - 可选项withscores 会打印出每一项的分数
- 删除：zrem key value

#### 1.6 中文问题

- 获取时加上参数`--raw`解决

### 2. 持久化

> 将主存中的数据按照配置持久化到硬盘中，redis 持久化提供了RDB和AOF两种机制。

1. RDB机制（default）：在一定的时间内监测key的变化，按照制定好的规则进行持久化操作，粒度较大。

   - 在`redis.windows.conf`文件中进行配置：
   - **注意**：重启服务时要指定配置文件名称；

```mysql
#   after 900 sec (15 min) if at least 1 key changed
save 900 1
#   after 300 sec (5 min) if at least 10 keys changed
save 300 10
#   after 60 sec if at least 10000 keys changed
save 60 10000
```



2. AOF机制：用日志记录每一条操作命令，粒度小。
   - 在`redis.windows.conf`文件中进行配置：

```mysql
appendonly no 改为 yes 开启
# appendfsync always ： 每一次操作都进行持久化
appendfsync everysec ： 每隔一秒进行一次持久化
# appendfsync no	 ： 不进行持久化
```



### 3. Jedis 使用

> API 名称和命令大同小异，靠着ide的提示就能写的出来，不再记录。
>
> jedis-2.7.0.jar
>
> commons-pool2-2.3.jar

- 不使用连接池：

```java
1. 获取连接
	Jedis jedis = new Jedis("localhost",6379);
2. 执行操作
	jedis.set("username","test")
3. 释放连接
	jedis.close();
```


- Jedis连接池配置

```java
public class JedisPoolUtils {

    private static JedisPool jedisPool;
  
    static{
        //读取配置文件
        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        //创建Properties对象
        Properties pro = new Properties();
        //关联文件
        try {
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        //获取数据，设置到JedisPoolConfig中
        JedisPoolConfig config = new JedisPoolConfig();
      config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
  
      //初始化JedisPool
        jedisPool = new JedisPool(config,pro.getProperty("host"),Integer.parseInt(pro.getProperty("port")));
  
    }
    public static Jedis getJedis(){
    	return this.jedisPoll.getResource();
    }
}
```

  

