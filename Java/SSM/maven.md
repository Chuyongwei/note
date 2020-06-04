RDB：默认记录

1.编辑redis.windows.conf

>   In the example below the behaviour will be to save:
>   after 900 sec (15 min) if at least 1 key changed
>   after 300 sec (5 min) if at least 10 keys changed
>   after 60 sec if at least 10000 keys changed

```
save 900 1
save 300 10
save 60 10000
```

设置完后在`cmd`中使用`redis-server.exe redis.windows.conf`开启服务器

AOF：日志记录

1.编辑redis.windows.conf

​	`appendonly no` 改成`yes`

	# appendfsync always//每一次保存
	appendfsync everysec//每一秒保存

Jedis(redis的连接方法)

> commons-pool2.2.4.2.jar
>
> jedis.jar



```java
    public void test1() {
        //链接
        Jedis jedis = new Jedis("localhost",6379);
        //建立
        jedis.set("username","zhangsan");
        //关闭
        jedis.close();
    }
```

