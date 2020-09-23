# 安装redis

## Linux

> 好像只有Linux的-_-||

### 0.依赖操作

> `gcc -v`注意安装redis6.0需要gcc5.3以上

```shell
yum -y install gcc tcl
yum -y install centos-release-scl
yum -y install devtoolset-9-gcc devtoolset-9-gcc-c++ devtoolset-9-binutils
echo "source /opt/rh/devtoolset-9/enable" >> /etc/profile
```

转载自https://www.jianshu.com/p/de745254e325

### 1.下载Redis

下载地址：https://redis.io/

```shell
 wget http://download.redis.io/releases/redis-6.0.3.tar.gz
 wget http://download.redis.io/redis-stable.tar.gz # 下载redis
 tar -xzvf redis-stable.tar.gz # 解压
 cd redis-stable # cd到上一部解压的目录
 make # 编译
 cd src # cd到编译后的目录
 make install PREFIX=/software/redis # 安装，PREFIX后面是目标目录
```

### 2. 移动配置文件到安装目录下

```powershell
# cd /software/redis // cd到redis安装目录
# mkdir etc // 创建etc目录
# cd /usr/download/redis-stable // cd到redis解压的目录，里面有 redis.conf
# mv redis.conf /software/redis/etc // 移动配置文件到安装目录
```

#### 配置redis为后台启动



```cpp
# vi /software/redis/ect/redis.conf // 编辑配置文件
```

`daemonize yes` 这里改为yes，然后保存退出

#### 将redis加入开启启动



```cpp
# vi /etc/rc.local // 编辑这个文件
```

添加`/software/redis/bin/redis-server /software/redis/etc/redis.conf` 意思就是开机调用这段开启redis的命令

#### 手动启动redis



```bash
# /software/redis/bin/redis-server /software/redis/etc/redis.conf
```

#### 查看redis是否启动



```bash
# ps aux|grep redis
```

![img](https:////upload-images.jianshu.io/upload_images/8701093-40670739e638cfa3.png?imageMogr2/auto-orient/strip|imageView2/2/w/1200/format/webp)

启动成功

#### 简单使用



```cpp
# cd software/redis/bin // cd 到bin目录
# ./redis-cli // 进入redis
127.0.0.1:6379> set name text // 存数据
OK
127.0.0.1:6379> get name // 取数据
"text"
```

#### 常用命令



```cpp
# /software/redis/bin/redis-server /software/redis/etc/redis.conf // 启动redis
# ps aux|grep redis // 查看是否启动成功
# kill [端口号] // 关闭redis
# pkill redis // 关闭redis
# rm -rf /usr/local/redis // 删除安装目录
# rm -rf /usr/bin/redis-* // 删除所有redis相关命令脚本
# rm -rf /root/download/redis-4.0.4 // 删除redis解压文件夹
```

后话：推荐阅读以下配置文件`/software/redis/etc/redis.conf`原文，里面有关于一些命令和配置的说明，例如



```bash
# Redis configuration file example.
#
# Note that in order to read the configuration file, Redis must be
# started with the file path as first argument:
#
# ./redis-server /path/to/redis.conf
```



### 参考

1.[redis官网](https://links.jianshu.com/go?to=https%3A%2F%2Fredis.io%2Fdownload)
 2.[make install 报错 server.c:5166:39: error: ‘str](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fxixiyuguang%2Farticle%2Fdetails%2F106612841)
 3.[CentOS 7.7系统安装Redis 6.0.3](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fsanduzxcvbnm%2Fp%2F12955145.html)
 4.[linux安装redis 完整步骤](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.cnblogs.com%2Fjohn-xiong%2Fp%2F12098827.html)
 5.[linux安装redis一，超详细说明与图解！！](https://links.jianshu.com/go?to=https%3A%2F%2Fblog.csdn.net%2Fqq_30764991%2Farticle%2Fdetails%2F81564652)
 6.https://www.cnblogs.com/chsoul/p/11937399.html

# 起步

## 开机

1. 安装目录下的`bin\redis-server etc/redis.conf`
2. 运行redis-cli
3. 关机在运行环境下输入shutdown

## 运行

### 数据结构

1. 数据结构

   （key，value）

   其中key是字符串，value有5中不同的数据结构

   - 字符串
   - 哈希
   - 列表
   - 集合
   - 有序集合

2. 字符串类型 string

   1. 存储:`set key value`
   2. 获取:`get key`
   3. 删除:`del key`

3. 哈希类型hash

   1. 存储：`hset key field value` 

   2. 获取：

      获取单个值：`hget key field`

      获取所有值：`hgetall key `

   3. 删除：`hdel key field`

4. 列表类型list

   1. 添加

      1. `lpush key value`
      2. `rpush key value`

   2. 获取

      `lrange key start end`:范围获取

   3. 删除

      1. `lpop key`
      2. `rpop key`

   ```powershell
   127.0.0.1:6379> lpush mylist a
   (integer) 1
   127.0.0.1:6379> lpush mylist b
   (integer) 2
   127.0.0.1:6379> rpush mylist c
   (integer) 3
   127.0.0.1:6379> lrange mylist 0 -1
   1) "b"
   2) "a"
   3) "c"
   127.0.0.1:6379> lpop mylist
   "b"
   127.0.0.1:6379> lrange mylist 0 -1
   1) "a"
   2) "c"
   ```

5. 集合类型set：不允许重复元素

   1. 存储：`sadd key vlue`
   2. 获取：`smembers key`获取set集合中所有元素
   3. 删除：`srem key value  `删除摸一个元素

6. 有序集合sortedset：不允许重复元素，且有序

   1. 存储：`zadd key score value`
   2. 获取：
      1. `zrange key start end`
      2. `zrange key start end withscores`
   3. 删除：`zrem key value`

7. 通用命令

   `keys *`查看所有键

   `type key`查看类型

   `del ksy`删除key

### 持久化

1. redis是一个内存数据库，当redis服务器重启，获取电脑重启，数据会丢失，我们可以将redis内存中的数据持久化保存到硬盘的文件中。

2. redis持久化机制：

    1. RDB：默认方式，不需要进行配置，默认就使用这种机制

        在一定的间隔时间中，检测key的变化情况，然后持久化数据

        1. 编辑redis.windwos.conf文件

           ```
           #   after 900 sec (15 min) if at least 1 key changed
           save 900 1
           #   after 300 sec (5 min) if at least 10 keys changed
           save 300 10
           #   after 60 sec if at least 10000 keys changed
           save 60 10000
           ```

        2. 重新启动redis服务器，并指定配置文件名称

           ```powershell
           D:\JavaWeb2018\day23_redis\资料\redis\windows-64\redis-2.8.9>redis-server.exe redis.windows.conf	
           ```

    2. AOF：日志记录的方式，可以记录每一条命令的操作。可以每一次命令操作后，持久化数据

        1. 编辑redis.windwos.conf文件
            				appendonly no（关闭aof） --> appendonly yes （开启aof）

            ```powershell
             # appendfsync always ： 每一次操作都进行持久化
             appendfsync everysec ： 每隔一秒进行一次持久化
             # appendfsync no	 ： 不进行持久化
            ```

## 远程连接redis

[远程连接redis]()

## 使用jedis

Jedis: 一款java操作redis数据库的工具.

使用步骤：

1. 下载jedis的jar包

2. 使用

    ```java
    //1. 获取连接
    	Jedis jedis = new Jedis("localhost",6379);
        //2. 操作
        jedis.set("username","zhangsan");
        //3. 关闭连接
        jedis.close();
    ```

3. Jedis操作各种redis中的数据结构

    1.  字符串类型 string

        ```java
             //1. 获取连接
             Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
             //2. 操作
             //存储
             jedis.set("username","zhangsan");
             //获取
             String username = jedis.get("username");
             System.out.println(username);
             
             //可以使用setex()方法存储可以指定过期时间的 key value
             jedis.setex("activecode",20,"hehe");//将activecode：hehe键值对存入redis，并且20秒后自动删除该键值对
             
             //3. 关闭连接
             jedis.close();
        ```

    2.  哈希类型 hash ： map格式  

        ```java
        public void test{
            hset
                hget
                hgetAll
                //1. 获取连接
                Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
            //2. 操作
            // 存储hash
            jedis.hset("user","name","lisi");
            jedis.hset("user","age","23");
            jedis.hset("user","gender","female");
            
            // 获取hash
            String name = jedis.hget("user", "name");
            System.out.println(name);
            
            // 获取hash的所有map中的数据
            Map<String, String> user = jedis.hgetAll("user");
            
            // keyset
            Set<String> keySet = user.keySet();
            for (String key : keySet) {
                //获取value
                String value = user.get(key);
                System.out.println(key + ":" + value);
            }
            
            //3. 关闭连接
            jedis.close();
        }
        ```

    3.  列表类型 list ： linkedlist格式。支持重复元素

        lpush / rpush

        lpop / rpop
        lrange start end : 范围获取

        ```java
        public void test(){
            //1. 获取连接
            Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
            //2. 操作
            // list 存储
            jedis.lpush("mylist","a","b","c");//从左边存
            jedis.rpush("mylist","a","b","c");//从右边存
            
            // list 范围获取
            List<String> mylist = jedis.lrange("mylist", 0, -1);
            System.out.println(mylist);
            
            // list 弹出
            String element1 = jedis.lpop("mylist");//c
            System.out.println(element1);
            
            String element2 = jedis.rpop("mylist");//c
            System.out.println(element2);
            
            // list 范围获取
            List<String> mylist2 = jedis.lrange("mylist", 0, -1);
            System.out.println(mylist2);
            
            //3. 关闭连接
            jedis.close();
        }
        ```

    4. 集合类型 set  ： 不允许重复元素

        sadd

        smembers:获取所有元素

        ```java
        //1. 获取连接
        		        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        		        //2. 操作
        		        // set 存储
        		        jedis.sadd("myset","java","php","c++");
        		
        		        // set 获取
        		        Set<String> myset = jedis.smembers("myset");
        		        System.out.println(myset);
        		
        		        //3. 关闭连接
        		        jedis.close();
        ```

    5.  有序集合类型 sortedset：不允许重复元素，且元素有顺序
        				zadd
        				zrange

        ```java
        			//1. 获取连接
        	        Jedis jedis = new Jedis();//如果使用空参构造，默认值 "localhost",6379端口
        	        //2. 操作
        	        // sortedset 存储
        	        jedis.zadd("mysortedset",3,"亚瑟");
        	        jedis.zadd("mysortedset",30,"后裔");
        	        jedis.zadd("mysortedset",55,"孙悟空");
        	
        	        // sortedset 获取
        	        Set<String> mysortedset = jedis.zrange("mysortedset", 0, -1);
        	
        	        System.out.println(mysortedset);
        	        	      //3. 关闭连接
        	        jedis.close();
        ```



## jedis连接池： JedisPool

* 使用：
     1. 创建JedisPool连接池对象
     	
     2. 调用方法 getResource()方法获取Jedis连接

          ```java
          void test(){
              //0.创建一个配置对象
                  JedisPoolConfig config = new JedisPoolConfig();
                  config.setMaxTotal(50);
                  config.setMaxIdle(10);
                  //1.创建Jedis连接池对象
              JedisPool jedisPool = new JedisPool(config,"localhost",6379);
          
              //2.获取连接
              Jedis jedis = jedisPool.getResource();
              //3. 使用
              jedis.set("hehe","heihei");    
              //4. 关闭 归还到连接池中
              jedis.close();
          ```

### 连接池工具类

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
    /**
			     * 获取连接方法
			     */
			    public static Jedis getJedis(){
			        return jedisPool.getResource();
			    }
			}		    
```

## 案例：
	案例需求：
		1. 提供index.html页面，页面中有一个省份 下拉列表
		2. 当 页面加载完成后 发送ajax请求，加载所有省份


	* 注意：使用redis缓存一些不经常发生变化的数据。
		* 数据库的数据一旦发生改变，则需要更新缓存。
			* 数据库的表执行 增删改的相关操作，需要将redis缓存数据情况，再次存入
			* 在service对应的增删改方法中，将redis数据删除。
