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

