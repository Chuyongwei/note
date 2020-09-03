# 这是一个牛逼的Mybatis教程
### 项目介绍

+ day01
1. `mybatis`的使用方法
2. 使用注释的方法制作 ==mybatis==
3. 使用原始的方法编写代码
4. 手写代码

+ day02
1. 增删改查 ，别名，数据库的映射代理（properties）
2. 深入mybatis，可以进行修改



### 注意事项

1. 通过映射来把值一一匹配

   但是一定要有主键
   
2. 使用查询操作时一定要sql字段与类的值的名字一样（在Windows系统中mysql是不区分大小写的）

   一致的意思是sql用别名
   
3. 8版本的数据连接池的

   `url`需要给定时间参数`?useSSL=false&amp;serverTimezone=UTC`

   驱动器位置改为`com.sql.jdbc.Driver` 	-->	 `com.mysql.cj.jdbc.Driver`

4. 