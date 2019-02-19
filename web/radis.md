# redis

## 1.操作

* **使用redis存取String类型的值**

```java
SET key value 保存一个值		key表示字符串的名称，value表示值
get key 获取一个值
MSET key value [key value ...]  保存多组值,[]表示可选值
MGET key [key ...]  获取多组值
APPEND key value 在原有的字符串上追加一段内容
strlen key 获取字符串的长度
```

* 使用redis往hash中存取值

```java
HSET key field value  向map中保存一个值，key表示map集合名称，field表示map集合中的key
HGET key field  根据键获取一个值
HMSET key field value [field value ...]  保存多组值
HMGET key field [field ...]  获取多组值
HGETALL key  获取所有的key和value值
HLEN key 获取所有的map集合长度
```

* 使用redis往list集合中存取值：

```java
LPUSH key value [value ...]  向集合的头部保存一个值  key表示集合名称
LRANGE key start stop  获取集合中所有的值 正数表示从左往右数，0开始，负数表示从右往左数，-1开始，-1表示最后一个值
LPOP key  弹出集合头部元素
LREM key count value  移除指定count个数的value值
LINDEX key index  根据索引获取值
```

* 其他操作：获取所有的key：keys*

```java
删除指定的key：del username; 
查看所有的key：keys*
查看key对应value的类型：type key
```

## 2.redis概述

* 什么是redis?

  Redis是一个开源的使用ANSI <u>C语言</u>编写、支持网络、可基于内存亦可持久化的日志型、Key-Value<u>数据库</u>，并提供多种语言的API。从2010年3月15日起，Redis的开发工作由VMware主持。从2013年5月开始，Redis的开发由Pivotal赞助。Redis是NoSQL技术阵营中的一员，它通过多种键值数据类型来适应不同场景下的存储要求，借助一些高层级的接口使用其可以胜任，如缓存、队列系统的不同角色

* redis的特性：

**Redis与其它key-value缓存产品有以下三个特点：**

①、Redis支持数据的持久化，可以将内存中的数据保存在磁盘中，重启的时候可以再次加载进行使用

②、Redis不仅仅支持简单的key-value类型的数据，同时还提供list、set、zset、hash等数据结构的存储

③、Redis支持数据的备份，即master-slave模式的数据备份

**Redis优势**

①、性能极高-Redis能读的速度是110000次/s，写的速度是81000次/s

②、丰富的数据类型-Redis支持二进制案例的Strings，Lists，Hashes，Sets及Ordered Sets数据类型操作

③、原子-Redis的所有操作都是原子性，同时Redis还支持几个操作全并后的原子性执行

④、丰富的特性-Redis还支持publish/subscribe。通知，key过期等等特性

**总结：**

​	关系型数据库与NoSQL数据库并非对立而是互补的关系，即通常情况下使用关系型数据库，在适合使用NoSQL的时候使用NoSQL数据库，让NoSQL数据库对关系型数据库的不足进行弥补

## 3.redis中key通用操作

**通用操作**

* KEYS pattern

  获取所有匹配pattern参数的Keys。需要说明的是，在我们的正常操作中应该尽量避免对该命令的调用，因为
  对于大型数据库而言，该命令是非常耗时的，对Redis服务器的性能打击也是比较大的。pattern支持globstyle的通配符格式，如*表示任意一个或多个字符，?表示任意字符，[abc]表示方括号中任意一个字母。 匹配
  模式的键列表

![1548227505888](D:\My Notes\1548227505888.png)

* DEL key[key ...]

  从数据库删除中参数中指定的keys，如果指定键不存在，则直接忽略。还需要另行指出的是，如果指定的
  Key关联的数据类型不是String类型，而是List、Set、Hashes和Sorted Set等容器类型，该命令删除每个键
  的时间复杂度为O(M)，其中M表示容器中元素的数量。而对于String类型的Key，其时间复杂度为O(1)。
  返回值：实际被删除的Key数量

  ![1548227617806](C:\Users\ADMINI~1\AppData\Local\Temp\1548227617806.png)

* EXISTS key

  判断指定键是否存在。
  返回值：1表示存在，0表示不存在。

* RENAME key newkey

  为指定的键重新命名，如果参数中的两个Keys的名字相同，或者是源Key不存在，该命令都会返回相关的错
  误信息。如果newKey已经存在，则直接覆盖。

## 4.redis持久化方案

* RBD方案：文件快照的方式，在一定的时间间隔内，有多少个key发生变化了就存储，优点：单独的开启一条
  线程存储数据，效率高，安全性略低；默认采用此方案
* AOF方案：文件追加方式，只要有一个key发生改变了，就追加到存储文件中，效率先对偏低，但是数据的安
  全性高

## 6.JedisUtils工具类编写

```java
public class JedisPoolUtils{
    private static JedisPool jedisPool;
    static{
        //读取配置文件
        InputStream is = JedisPoolUtils.class.getClassLoader().getResourceAsStream("jedis.properties");
        //创建Properties对象
        Properties pro = new Properties();
        //关联文件
        try{
            pro.load(is);
        } catch (IOException e) {
            e.printStackTrace();
        }
        //获取数据，设置JedisPoolConfig中
        JedisPoolConfig config = new JedisPoolConfig();
        config.setMaxTotal(Integer.parseInt(pro.getProperty("maxTotal")));
        config.setMaxIdle(Integer.parseInt(pro.getProperty("maxIdle")));
        //初始化JedisPool
        jedisPool = new JedisPool(config,pro.getProperty("host"),Integer.parseInt(pro.getProperty("port")));
    }
    //获取连接方法
    public static Jedis getJedis(){
        return jedisPool.getResource();
    }
}
```

