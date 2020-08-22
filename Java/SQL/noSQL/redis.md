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