## 准备

1. ### maven

   ```xaml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>com.ithema</groupId>
       <artifactId>day01_eesay_01mybatis</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <dependencies>
           <dependency>
               <groupId>org.mybatis</groupId>
               <artifactId>mybatis</artifactId>
               <version>3.5.4</version>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.19</version>
           </dependency>
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.12</version>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>3.8.2</version>
               <scope>test</scope>
           </dependency>
       </dependencies>
   
   </project>
   ```

2. 扫描文档

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
       <!-- 配置环境 -->
       <environments default="mysql">
           <!-- 配置mysql环境 -->
           <environment id="mysql">
               <!--配置事务类型 -->
               <transactionManager type="JDBC"></transactionManager>
               <dataSource type="POOLED">
                   <!-- 配置四个信息 -->
                   <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/db_mybatis?useSSL=false&amp;serverTimezone=UTC"/>
                   <property name="username" value="root"/>
                   <property name="password" value="654321"/>
               </dataSource>
           </environment>
       </environments>
       <!-- 映射地址 -->
       <!--
       指定
        -->
       <mappers>
           <mapper resource="com/itheima/dao/IUSerDao.xml"/>
       </mappers>
   </configuration>
   ```

3. 建立工作间

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper
           PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <mapper namespace="com.ithema.dao.IUserDao">
       <!-- 配置所有 -->
       <select id="findAll" resultType="com.ithema.domain.User">
   select * from user
       </select>
    </mapper>
   ```

4. 建立类

   > 使用查询操作时一定要sql字段与类的值的名字一样（在Windows系统中mysql是不区分大小写的）
   >
   > 一致的意思是sql用别名

   

5. 运行

   ```java
   package com.ithema.dao.test;
   
   import com.ithema.dao.IUserDao;
   import com.ithema.domain.User;
   import org.apache.ibatis.io.Resources;
   import org.apache.ibatis.session.SqlSession;
   import org.apache.ibatis.session.SqlSessionFactory;
   import org.apache.ibatis.session.SqlSessionFactoryBuilder;
   
   import java.io.IOException;
   import java.io.InputStream;
   import java.util.List;
   
   public class MybatisTest {
       /**
        * 入门
        */
       public static void main(String[] args) throws IOException {
   //        1.读取文件
           InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
   
   //        2.创建SqlSessionFactory工厂
           SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
           SqlSessionFactory factory = builder.build(in);
   //        3.使用工厂生产SqlSessioin对象
           SqlSession session = factory.openSession();
   //        4.使用SqlSession创建Dao接口的代理对象
           IUserDao userDao = session.getMapper(IUserDao.class);
   //        5.使用代理对象执行方法
           List<User> users = userDao.findAll();
           for (User user:users){
               System.out.println(user);
           }
   //        6.释放资源
           session.close();
           in.close();
   
       }
   }
   
   ```

   

   

   ![mybatis of operating principle](D:\baidu\MarkDown\Java\SSM\image\SSM\mybatis of operating principle.png)

<center>图1.1 mybatis的运行原理</center>

### 数据库

```sql
DROP TABLE IF EXISTS `user`;

CREATE TABLE `user` (
  `id` int(11) NOT NULL auto_increment,
  `username` varchar(32) NOT NULL COMMENT '用户名称',
  `birthday` datetime default NULL COMMENT '生日',
  `sex` char(1) default NULL COMMENT '性别',
  `address` varchar(256) default NULL COMMENT '地址',
  PRIMARY KEY  (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



insert  into `user`(`id`,`username`,`birthday`,`sex`,`address`) values (41,'老王','2018-02-27 17:47:08','男','北京'),(42,'小二王','2018-03-02 15:09:37','女','北京金燕龙'),(43,'小二王','2018-03-04 11:34:34','女','北京金燕龙'),(45,'传智播客','2018-03-04 12:04:06','男','北京金燕龙'),(46,'老王','2018-03-07 17:37:26','男','北京'),(48,'小马宝莉','2018-03-08 11:44:00','女','北京修正');





DROP TABLE IF EXISTS `account`;

CREATE TABLE `account` (
  `ID` int(11) NOT NULL COMMENT '编号',
  `UID` int(11) default NULL COMMENT '用户编号',
  `MONEY` double default NULL COMMENT '金额',
  PRIMARY KEY  (`ID`),
  KEY `FK_Reference_8` (`UID`),
  CONSTRAINT `FK_Reference_8` FOREIGN KEY (`UID`) REFERENCES `user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



insert  into `account`(`ID`,`UID`,`MONEY`) values (1,41,1000),(2,45,1000),(3,41,2000);



DROP TABLE IF EXISTS `role`;

CREATE TABLE `role` (
  `ID` int(11) NOT NULL COMMENT '编号',
  `ROLE_NAME` varchar(30) default NULL COMMENT '角色名称',
  `ROLE_DESC` varchar(60) default NULL COMMENT '角色描述',
  PRIMARY KEY  (`ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;



insert  into `role`(`ID`,`ROLE_NAME`,`ROLE_DESC`) values (1,'院长','管理整个学院'),(2,'总裁','管理整个公司'),(3,'校长','管理整个学校');





DROP TABLE IF EXISTS `user_role`;

CREATE TABLE `user_role` (
  `UID` int(11) NOT NULL COMMENT '用户编号',
  `RID` int(11) NOT NULL COMMENT '角色编号',
  PRIMARY KEY  (`UID`,`RID`),
  KEY `FK_Reference_10` (`RID`),
  CONSTRAINT `FK_Reference_10` FOREIGN KEY (`RID`) REFERENCES `role` (`ID`),
  CONSTRAINT `FK_Reference_9` FOREIGN KEY (`UID`) REFERENCES `user` (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;

insert  into `user_role`(`UID`,`RID`) values (41,1),(45,1),(41,2);



```

==注意==：

1. 使用查询操作时一定要sql字段与类的值的名字一样（在Windows系统中mysql是不区分大小写的）

   一致的意思是sql用别名

> 自动补全变量名称 : Ctrl + Alt + v
>
> 自动补全属性名称 : Ctrl + Alt + f

## 开始

### 匹配

>通过映射来把值一一匹配
>
>但是一定要有主键
>
>

```xml
<!-- 配置 查询结果的列名和实体类的属性名的对应关系 -->
    <resultMap id="userMap" type="com.itheima.domain.User">
        <!-- 主键字段的对应 -->
        <id property="userId" column="id"></id>
        <!--非主键字段的对应-->
        <result property="userName" column="username"></result>
        <result property="userAddress" column="address"></result>
        <result property="userSex" column="sex"></result>
        <result property="userBirthday" column="birthday"></result>
    </resultMap>
```





![](D:\baidu\MarkDown\Java\SSM\image\SSM\connect1.png)







![connect2](D:\baidu\MarkDown\Java\SSM\image\SSM\connect2.png)





### 构造语句

#### if语句



```xml
    <select id="findUserByCondition" resultMap="userMap" parameterType="user">
        select * from user where 1=1
        <if test="userName != null">
          and   username = #{userName}
        </if>
    </select>
```



> 如果发现参数username不为空就加上这个语句 `select * from user where  1 = 1 and username = #{userName}`
>
> 否则为`select * from user where 1=1`





添加默认语句

```xml
    <!-- 了解 -->
    <sql id="defaultUser">
        select * from user
    </sql>
    <!-- 配置所有 -->
    <select id="findAll" resultMap="userMap">
	<!--  select * from user -->
<include refid="defaultUser"></include>
    </select>
```





## 注解

```java
        //1.获取字节输入流
        InputStream in = Resources.getResourceAsStream("SqlMapConfig.xml");
        //2.根据字节输入流构建SqlSessionFactory
        SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(in);
        //3.根据SqlSessionFactory生产一个SqlSession
        SqlSession session = factory.openSession();
        //4.使用SqlSession获取Dao的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);
        //5.执行Dao的方法
        List<User> users = userDao.findAll();
        for(User user : users){
            System.out.println(user);
        }
        //6.释放资源
        session.close();
        in.close();
```

### 匹配

```java
@Select("select * from user") 
@Results(id="userMap",value={
            @Result(id=true,column = "id",property = "userId"),
            @Result(column = "username",property = "userName")
 })

List<User> findAll();
```

多对一

```java
    @Results(id="accountMap",value = {
            @Result(id=true,column = "id",property = "id"),
            @Result(column = "uid",property = "uid"),
            @Result(column = "money",property = "money"),
            @Result(property = "user",column = "uid",one=@One(select="com.itheima.dao.IUserDao.findById",fetchType= FetchType.EAGER))
    })
```



## 未学习内容

1. 第二天的最后的深入教程
2. 第三天的一对多，多对一,一对一
3. 第四天的缓存和注解的缓存
