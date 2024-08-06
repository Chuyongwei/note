## 执行周期

查看sql的执行周期

1. 查看profiling

   ```mysql
   show VARIABLES LIKE '%profiling%';
   ```

2. 修改profiling

   ```mysql
   SET profiling = 1;
   ```

   


## 常见的存储引擎

### 创建表

```sql
CREATE TABLE `t1` ( 
    `id` int NOT NULL AUTO_INCREMENT, 
    `name` varchar(20) DEFAULT NULL, 
    PRIMARY KEY (`id`) 
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb3;

CREATE TABLE `t2` ( 
    `id` int NOT NULL AUTO_INCREMENT, 
    `name` varchar(20) DEFAULT NULL, 
    PRIMARY KEY (`id`) 
) ENGINE=MyISAM DEFAULT CHARSET=utf8mb3;
```



默认的搜索引擎

```sql
show variables like 'default_storage_engine'
```

设置引擎

```sql
create table t2( 
    id int primary key auto_increment, 
    name varchar(20) 
)ENGINE=MyISAM;
```

查看表的状态

```sql
show table status from demo;
```



|              | InnoDB             | MyISAM                 |
| ------------ | ------------------ | ---------------------- |
| 是否⽀持事务 | 是                 | 否                     |
| 锁的粒度     | 表锁, ⾏锁         | 表锁                   |
| 是否⽀持外键 | 是                 | 否                     |
| 数据存储⽅式 | 索引和数据存放⼀起 | 索引⽂件和数据⽂件分开 |
| 数据存储结构 | B+Tree             | B+Tree                 |



### B树和B+树

B树每个节点存储数据

B+树非叶子节点不存储数据，数据在叶子节点，在叶子节点之间存在双向链表



###   MyISAM

使用B+树作为索引结构，叶节点的data



# 用户

## 创建用户

```sql
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```



>说明： username：你将创建的用户名
>host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%
>password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器

## 创建权限

```sql
GRANT privileges ON databasename.tablename TO 'username'@'host' 
```

> 说明: privileges：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL
> databasename：数据库名 tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*

```sql
例子:
GRANT SELECT, INSERT ON test.user TO 'pig'@'%';
GRANT ALL ON *.* TO 'pig'@'%';
```

该用户可赋予其他用户权限

```sql
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
```



## 撤销权限

```sql
命令:
REVOKE privilege ON databasename.tablename FROM 'username'@'host';
说明:
privilege, databasename, tablename：同授权部分
例子:
REVOKE SELECT ON *.* FROM 'pig'@'%';
```

## 删除用户

```sql
命令:
DROP USER 'username'@'host';
```



## 修改密码

```sql
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
```

当前用户

```sql
SET PASSWORD = PASSWORD("newpassword");
例子:
SET PASSWORD FOR 'pig'@'%' = PASSWORD("123456");
```

## 更新表

```sql
flush privileges;
```

