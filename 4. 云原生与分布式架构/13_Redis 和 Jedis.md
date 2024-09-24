## 1. 概念

redis是一款高性能的NOSQL系列的非关系型数据库。

### 1. 非关系型数据库

关系型数据库可以描述数据之间的关联关系，数据存储在硬盘文件上；而非关系型数据库数据之间没有关联关系，数据存储在内存中。

> 举例：登录微信时，微信服务端要去检索user表（数据上亿条），如果是关系型数据库，会非常耗时。

经常查询一些不太经常发生变化的数据，可以采用缓存思路解决：将第一次从数据库获取的数据进行缓存，之后每次都去缓存获取（redis缓存）

### 2. NOSQL

NoSQL意即“不仅仅是SQL”，是一项全新的数据库理念，泛指非关系型的数据库。

NoSQL数据库的产生就是为了解决大规模数据集合多重数据种类带来的挑战，尤其是大数据应用难题。



## 2. 命令操作

### 1. redis的数据结构

redis存储的是：key、value格式的数据，其中key都是字符串，value有5种不同的数据结构：

1> 字符串类型 String

2> 哈希类型 hash：map格式

3> 列表类型 list

4> 集合类型 set：元素不重复的列表，但不保证顺序

5> 有序集合类型 sortedset



### 2. 操作字符串类型

1> 存储：`set key value`

2> 获取：`get key`

3> 删除：`del key`



### 3. 操作哈希类型

（map键值对格式，field值得是键或值）

1> 存储：`hset key field value`

2> 获取：

​    `hget key field`：获取指定的field对应的值

​    `hgetall key`：获取所有的field和value

3> 删除：`hdel key field`



### 4. 列表类型list

可以添加一个元素到列表的头部（左边）或者尾部（右边）

（key 表示列表的名称）

1> 添加：

​    `lpush key value`：将元素加入列表左边

​    `rpush key value`：将元素加入列表右边

2> 获取：

​    `lrange key start end`：获取范围

3> 删除：

​    `lpop key`：删除列表最左边的元素并将元素返回

​    `rpop key`：删除列表最右边的元素并将元素返回



### 5. 集合类型set

不允许重复，但不保证顺序（key 表示set的名称）

1> 存储：`sadd key value` 

2> 获取：`smembers key` ：获取set集合中所有元素

3> 删除：`srem key value` ：删除set集合中的某个元素



### 6. 有序集合类型sortedset

不允许重复元素，且元素有顺序（key表示集合名称，score表示从小到大的排序序列）

1> 存储：`zadd key score value`

2> 获取：`zrange key start end`

3> 删除：`zrem key value`



### 7. 通用命令

1> keys * ：获取所有的key（目前存储的数据结构名称）

2> type key ： 获取键对应的value类型（知道类型后可以通过对应命令进行操作）

3> del key：删除指定的key value



## 3. 持久化

redis是一个内存数据库，当redis服务器重启，或者电脑重启数据会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中。

### redis持久化机制

（持久化后的文件会保存在redis安装根目录下）

​    1> RDB：默认方式，不需要进行配置，默认就使用这种机制。（在一定的间隔时间中，检测key的变化情况，然后持久化数据）

​        \* 编辑redis安装目录下的`redis.windows.conf`文件：

​        save 900 1 // 900秒中至少有一个key发生改变，就会自动持久化一次

​        save 300 10 // 300秒中至少有10个key发生改变，就会自动持久化一次

​        save 60 10000 // 10000秒中至少有60个key发生改变，就会自动持久化一次

​        

​        \* 修改配置文件后，要重新启动redis服务器，并指定配置文件名称

​        在redis目录下：`redis-server.exe redis.windows.conf`



​    2> AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据。

​        \* 编辑redis.windows.conf 配置文件

​        appendonly on （关闭AOF） -> appendonly yes（开启aof）

​        appendsync always：每一次操作都进行持久化

​        appendfsync everysec：每隔一秒进行一次持久化

​        appendfsync no：不进行持久化



## 4. Java客户端 Jedis

Jedis: 一款java操作redis数据库的工具。

使用步骤：

### 1. 下载jedis的jar包

​    `commons-pool.jar、jedis.jar`



### 2. 使用

```
// 获取连接

Jedis jedis = new Jedis(host: 'localhost', port: 6379)

// 操作

jedis.set('username': 'zhangsan')

// 关闭连接

jedis.close()
```



### 3. Jedis操作各种redis中的数据结构

1> 字符串

```
jedis.set() 

jedis.get()

// 可以使用setex()方法存储可以指定过期时间的key value

jedis.setex(key: 'activecode', seconds: 20, value: 'hehe') // 将acitvecode:hehe键值对存入reids，20秒后自动删除键值对
```



2> 哈希

```
jedis.hset(key: 'user', field: 'name', value: 'lini')

jedis.hset(key: 'user', field: 'age', value: '23')

jedis.hget(key: 'user', field: 'name')

jedis.hgetAll(key: 'user') // 获取hash中所有map中的数据
```



3> 列表

```
jedis.lpush(key: 'mylist', ...strings: 'a', 'b', 'c') // 从左边存

jedis.rpush() // 从右边存

jedis.lrange(key: 'mylist', start:0, end: -1)

jedis.lpop(key: 'mylist') // c

jedis.rpop() // 从右边弹出
```



4> 集合

```
jedis.sadd(key: 'myset', ...members: 'java', 'php', 'c++')

jedis.smembers(key: 'myset')
```



5> 有序集合类型

```
jedis.zadd(key: 'mysortedset', score: 3, member: '亚瑟')

jedis.zrange(key: 'mysortedset', start:0, end:-1) // 获取所有
```



## 5. jedis连接池：JedisPool

```
// 1. 创建一个配置对象

JedisPoolConfig config = new JedisPoolConfig()

config.setMaxTotal(50) // 最大活动对象数

config.setMaxIdle(10) // 最大能够吧保持idel状态的对象数

// 2. 创建JedisPool连接池对象

JedisPool jedisPool = new JedisPool(config, host: 'localhost', port: 6379)

// 3. 调用方法 getResource() 方法获取Jedis连接

Jedis jedis = jedisPool.getResource()

// 4. 使用

jedis.set('hehe', 'haha')

// 5. 关闭 归还到连接池

jedis.close()
```

