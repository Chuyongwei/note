## 导入

<!-- 2020.07.05 -->

### ORM思想

主要目的：操作实体类就相当于操作数据库

建两个映射关系

- 实体类和表的映射关系
- 实体类中属性和表中字段的映射关系

不再关注sql语句

### JPA规范

关于jdbc8和5

8版本的数据连接池的`url`需要

给定时间参数`?useSSL=false&amp;serverTimezone=UTC`

驱动器位置改为`com.mysql.cj.jdbc.Driver`不是`com.sql.jdbc.Driver`

### sql文件

```sql
 /*创建客户表*/
    CREATE TABLE cst_customer (
      cust_id bigint(32) NOT NULL AUTO_INCREMENT COMMENT '客户编号(主键)',
      cust_name varchar(32) NOT NULL COMMENT '客户名称(公司名称)',
      cust_source varchar(32) DEFAULT NULL COMMENT '客户信息来源',
      cust_industry varchar(32) DEFAULT NULL COMMENT '客户所属行业',
      cust_level varchar(32) DEFAULT NULL COMMENT '客户级别',
      cust_address varchar(128) DEFAULT NULL COMMENT '客户联系地址',
      cust_phone varchar(64) DEFAULT NULL COMMENT '客户联系电话',
      PRIMARY KEY (`cust_id`)
    ) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```



## 操作

>  以hibernate为例

### 搭建环境

1. 创建工程导入包

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <project xmlns="http://maven.apache.org/POM/4.0.0"
            xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
       <modelVersion>4.0.0</modelVersion>
   
       <groupId>nuc.wcy</groupId>
       <artifactId>jpa-day01</artifactId>
       <version>1.0-SNAPSHOT</version>
   
       <properties>
           <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
           <project.hibernate.version>5.0.7.Final</project.hibernate.version>
       </properties>
   
       <dependencies>
           <!-- junit -->
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.12</version>
               <scope>test</scope>
           </dependency>
   
           <!-- hibernate对jpa的支持包 -->
           <dependency>
               <groupId>org.hibernate</groupId>
               <artifactId>hibernate-entitymanager</artifactId>
               <version>${project.hibernate.version}</version>
           </dependency>
   
           <!-- c3p0 -->
           <dependency>
               <groupId>org.hibernate</groupId>
               <artifactId>hibernate-c3p0</artifactId>
               <version>${project.hibernate.version}</version>
           </dependency>
   
           <!-- log日志 -->
           <dependency>
               <groupId>log4j</groupId>
               <artifactId>log4j</artifactId>
               <version>1.2.17</version>
           </dependency>
   
           <!-- Mysql and MariaDB -->
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>5.1.6</version>
           </dependency>
       </dependencies>
   </project>
   ```

   

2. 配置jpa的核心配置文件

   > 在java工程的src路径下创建一个名为META-INF的文件夹，在此文件夹下创建一个名为persistence.xml的配置文件

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <persistence xmlns="http://java.sun.com/xml/ns/persistence"
                xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
                xsi:schemaLocation="http://java.sun.com/xml/ns/persistence
       http://java.sun.com/xml/ns/persistence/persistence_2_0.xsd"
                version="2.0">
       <!--配置持久化单元
   		name：持久化单元名称
   		transaction-type：事务类型
   		 	RESOURCE_LOCAL：本地事务管理
   		 	JTA：分布式事务管理 -->
       <persistence-unit name="myJpa" transaction-type="RESOURCE_LOCAL">
           <!--配置JPA规范的服务提供商 -->
           <provider>org.hibernate.jpa.HibernatePersistenceProvider</provider>
           <properties>
               <!-- 数据库驱动 -->
               <property name="javax.persistence.jdbc.driver" value="com.mysql.jdbc.Driver" />
               <!-- 数据库地址 -->
               <property name="javax.persistence.jdbc.url" value="jdbc:mysql://localhost:3306/ssh" />
               <!-- 数据库用户名 -->
               <property name="javax.persistence.jdbc.user" value="root" />
               <!-- 数据库密码 -->
               <property name="javax.persistence.jdbc.password" value="111111" />
   
               <!--jpa提供者的可选配置：我们的JPA规范的提供者为hibernate，所以jpa的核心配置中兼容hibernate的配 -->
   			 <!--配置jpa实现方(hibernate)的配置信息
                   显示sql           ：   false|true
                   自动创建数据库表    ：  hibernate.hbm2ddl.auto
                           create      : 程序运行时创建数据库表（如果有表，先删除表再创建）
                           update      ：程序运行时创建表（如果有表，不会创建表）
                           none        ：不会创建表
   
               -->
               <property name="hibernate.show_sql" value="true" />
               <property name="hibernate.format_sql" value="true" />
               <property name="hibernate.hbm2ddl.auto" value="create" />
           </properties>
       </persistence-unit>
   </persistence>
   ```

   ==注意==：以上使用的是以mysql5为准的若要使用mysql8则用

   ```xml
   <dependency>
       <groupId>mysql</groupId>
       <artifactId>mysql-connector-java</artifactId>
       <version>8.0.20</version>
   </dependency>
   ```

   | 属性   |                              值                              |
   | ------ | :----------------------------------------------------------: |
   | Driver |                   com.mysql.cj.jdbc.Driver                   |
   | url    | jdbc:mysql://localhost:3306/itheima?useSSL=false&amp;serverTimezone=UTC |

   

3. 编写客户类和表

   客户的实体类
        配置映射关系
     1.实体类和表的映射关系
        `@Entity`:声明实体类
        `@Table `: 配置实体类和表的映射关系
            `name` : 配置数据库表的名称
     2.实体类中属性和表中字段的映射关系

   实体类的属性

   `@Id`：声明主键的配置
   `@GeneratedValue`:配置主键的生成策略
        strategy:

   - `GenerationType.IDENTITY` ：自增，mysql

     底层数据库必须支持自动增长（底层数据库支持的自动增长方式，对id自增

   - `GenerationType.SEQUENCE `: 序列，oracle
      底层数据库必须支持序列
     
   - `GenerationType.TABLE `: jpa提供的一种机制，通过一张数据库表的形式帮助我们完成主键自增
     
   - `GenerationType.AUTO` ： 由程序自动的帮助我们选择主键生成策略

   `@Column`:配置属性和字段的映射关系
                  name：数据库表中字段的名称

   ```java
   @Entity
   @Table(name = "cst_customer")
   public class Customer {
   
       @Id
       @GeneratedValue(strategy = GenerationType.IDENTITY)
       @Column(name = "cust_id")
       private Long custId; //客户的主键
   
       @Column(name = "cust_name")
       private String custName;//客户名称
   
       @Column(name="cust_source")
       private String custSource;//客户来源
   
       @Column(name="cust_level")
       private String custLevel;//客户级别
   
       @Column(name="cust_industry")
       private String custIndustry;//客户所属行业
   
       @Column(name="cust_phone")
       private String custPhone;//客户的联系方式
   
       @Column(name="cust_address")
       private String custAddress;//客户地址
   
       public Long getCustId() {
           return custId;
       }
   
       public void setCustId(Long custId) {
           this.custId = custId;
       }
   
       public String getCustName() {
           return custName;
       }
   
       public void setCustName(String custName) {
           this.custName = custName;
       }
   
       public String getCustSource() {
           return custSource;
       }
   
       public void setCustSource(String custSource) {
           this.custSource = custSource;
       }
   
       public String getCustLevel() {
           return custLevel;
       }
   
       public void setCustLevel(String custLevel) {
           this.custLevel = custLevel;
       }
   
       public String getCustIndustry() {
           return custIndustry;
       }
   
       public void setCustIndustry(String custIndustry) {
           this.custIndustry = custIndustry;
       }
   
       public String getCustPhone() {
           return custPhone;
       }
   
       public void setCustPhone(String custPhone) {
           this.custPhone = custPhone;
       }
   
       public String getCustAddress() {
           return custAddress;
       }
   
       public void setCustAddress(String custAddress) {
           this.custAddress = custAddress;
       }
   
       @Override
       public String toString() {
           return "Customer{" +
                   "custId=" + custId +
                   ", custName='" + custName + '\'' +
                   ", custSource='" + custSource + '\'' +
                   ", custLevel='" + custLevel + '\'' +
                   ", custIndustry='" + custIndustry + '\'' +
                   ", custPhone='" + custPhone + '\'' +
                   ", custAddress='" + custAddress + '\'' +
                   '}';
       }
   }
   ```

   

4. 类

   persist:保存

   merge：更新

   remove：删除

   find/getRefrence：查询

   

5. 保存客户数据

   ```java
   public class JpaTest {
   
       @Test
       public void testSave(){
           //1.通过工具类获取entityManager
           EntityManagerFactory factory = Persistence.createEntityManagerFactory("myJpa");
           //2.开启事务
           EntityManager em = factory.createEntityManager();
           //3.获取事务对象，开启事务
            EntityTransaction tx = em.getTransaction();//获取事务对象
           tx.begin();//开启事务
           //4.完成增删改查操作：保存一个客户到数据库中
           Customer customer = new Customer();
           customer.setCustName("传智播客");
           customer.setCustIndustry("教育");
           //保存，
           em.persist(customer);
           //5.提交事务
           tx.commit();
           //6.释放资源
           em.close();
           factory.close();
       }
   }
   ```

6. 查询数据

   ```java
    @Test
    public void testFind(){
        EntityManagerFactory factory = Persistence.createEntityManagerFactory("myJpa");
        EntityManager manager = factory.createEntityManager();
        EntityTransaction transaction = manager.getTransaction();
        transaction.begin();
        Customer customer = manager.find(Customer.class,1l);
        System.out.print(customer);
        transaction.commit();
        manager.close();
    }
    @Test
    public void testRefernce(){
        EntityManagerFactory factory = Persistence.createEntityManagerFactory("myJpa");
        EntityManager manager = factory.createEntityManager();
        EntityTransaction transaction = manager.getTransaction();
        transaction.begin();
        Customer customer = manager.getReference(Customer.class,1l);
        System.out.print(customer);
        transaction.commit();
        manager.close();
    }
   ```

   两种方式的不同

   - find方法

     查询的对象就是当前客户对象本身

     在调用find方法的时候就会发送sql

   - Reference

     获取对象是一个动态代理对象

     调用方法不会立即发送sql语句查询数据库

     当调用查询结果时才会发送查询的sql语句

7. 删除

   ```java
   @Test
   public  void testRemove() {
       //1.通过工具类获取entityManager
       EntityManager entityManager = JpaUti
       //2.开启事务
       EntityTransaction tx = entityManager
       tx.begin();
       //3.增删改查 -- 删除客户
       //i 根据id查询客户
       Customer customer = entityManager.fi
       //ii 调用remove方法完成删除操作
       entityManager.remove(customer);
       //4.提交事务
       tx.commit();
       //5.释放资源
       entityManager.close();
   }
   ```

8. 更新

   ```java
   @Test
   public  void testUpdate() {
       //1.通过工具类获取entityManager
       EntityManager entityManager = JpaUtils.getEntityManager();
       //2.开启事务
       EntityTransaction tx = entityManager.getTransaction();
       tx.begin();
       //3.增删改查 -- 更新操作
       //i 查询客户
       Customer customer = entityManager.find(Customer.class,1l);
       //ii 更新客户
       customer.setCustIndustry("it教育");
       entityManager.merge(customer);
       //4.提交事务
       tx.commit();
       //5.释放资源
       entityManager.close();
   }
   ```

<!-- 2020.07.06 -->

### JPQL语句

> JPQL（Java Persistence Query Language），JPQL是EJB QL的一种扩展，它是针对实体的一种查询语言，操作对象是实体，而不是关系数据库的表，而且能够支持批量更新和修改、JOIN、GROUP BY、HAVING 等通常只有 SQL 才能够提供的高级查询特性，甚至还能够支持子查询。

#### 使用&实例

```java
/*
查询全部
     jqpl：from cn.itcast.domain.Customer
     sql：SELECT * FROM cst_customer
*/
@Test
public void TestFindALL(){
    //1.获取entityManager对象
    EntityManager em = JpaUtils.getEntityManager();
    //2.开启事务
    EntityTransaction tx = em.getTransaction();
    tx.begin();
    //3.查询全部
    String jpql = "from Customer ";
    Query query = em.createQuery(jpql);//创建Query查询对象，query对象才是执行jqpl的对象
    //发送查询，并封装结果集
    List list = query.getResultList();
    for (Object obj : list) {
        System.out.print(obj);
    }
    //4.提交事务
    tx.commit();
    //5.释放资源
    em.close();
}
```



```java
/*
排序查询： 倒序查询全部客户（根据id倒序）
     sql：SELECT * FROM cst_customer ORDER BY cust_id DESC
     jpql：from Customer order by custId desc
*/
@Test
public void testOrders() {
    //1.获取entityManager对象
    EntityManager em = JpaUtils.getEntityManager
    //2.开启事务
    EntityTransaction tx = em.getTransaction();
    tx.begin();
    //3.查询全部
    String jpql = "from Customer order by custId
    Query query = em.createQuery(jpql);//创建Query
    //发送查询，并封装结果集
    List list = query.getResultList();
    for (Object obj : list) {
        System.out.println(obj);
    }
    //4.提交事务
    tx.commit();
    //5.释放资源
    em.close();
}
```



```java
/**
 * 使用jpql查询，统计客户的总数
 *      sql：SELECT COUNT(cust_id) FROM cst_customer
 *      jpql：select count(custId) from Customer
 */
@Test
public void testCount() {
    //1.获取entityManager对象
    EntityManager em = JpaUtils.getEntityManager();
    //2.开启事务
    EntityTransaction tx = em.getTransaction();
    tx.begin();
    //3.查询全部
    //i.根据jpql语句创建Query查询对象
    String jpql = "select count(custId) from Customer";
    Query query = em.createQuery(jpql);
    //ii.对参数赋值
    //iii.发送查询，并封装结果
    /**
     * getResultList ： 直接将查询结果封装为list集合
     * getSingleResult : 得到唯一的结果集
     */
    Object result = query.getSingleResult();
    System.out.println(result);
    //4.提交事务
    tx.commit();
    //5.释放资源
    em.close();
}
/**
 * 分页查询
 *      sql：select * from cst_customer limit 0,2
 *      jqpl : from Customer
 */
@Test
public void testPaged() {
    //1.获取entityManager对象
    EntityManager em = JpaUtils.getEntityManager();
    //2.开启事务
    EntityTransaction tx = em.getTransaction();
    tx.begin();
    //3.查询全部
    //i.根据jpql语句创建Query查询对象
    String jpql = "from Customer";
    Query query = em.createQuery(jpql);
    //ii.对参数赋值 -- 分页参数
    //起始索引
    query.setFirstResult(0);
    //每页查询的条数
    query.setMaxResults(2);
    //iii.发送查询，并封装结果
    /**
     * getResultList ： 直接将查询结果封装为list集合
     * getSingleResult : 得到唯一的结果集
     */
    List list = query.getResultList();
    for(Object obj : list) {
        System.out.println(obj);
    }
    //4.提交事务
    tx.commit();
    //5.释放资源
    em.close();
}
/**
 * 条件查询
 *     案例：查询客户名称以‘传智播客’开头的客户
 *          sql：SELECT * FROM cst_customer WHERE cust_name LIKE  ?
 *          jpql : from Customer where custName like ?
 */
@Test
public void testCondition() {
    //1.获取entityManager对象
    EntityManager em = JpaUtils.getEntityManager();
    //2.开启事务
    EntityTransaction tx = em.getTransaction();
    tx.begin();
    //3.查询全部
    //i.根据jpql语句创建Query查询对象
    String jpql = "from Customer where custName like ? ";
    Query query = em.createQuery(jpql);
    //ii.对参数赋值 -- 占位符参数
    //第一个参数：占位符的索引位置（从1开始），第二个参数：取值
    query.setParameter(1,"传智播客%");
    //iii.发送查询，并封装结果
    /**
     * getResultList ： 直接将查询结果封装为list集合
     * getSingleResult : 得到唯一的结果集
     */
    List list = query.getResultList();
    for(Object obj : list) {
        System.out.println(obj);
    }
    //4.提交事务
    tx.commit();
    //5.释放资源
    em.close();
}
```



## SpringJPA

> Spring Data JPA 是 Spring 基于 ORM 框架、JPA 规范的基础上封装的一套JPA应用框架，可使开发者用极简的代码即可实现对数据库的访问和操作。它提供了包括增删改查等在内的常用功能，且易于扩展！学习并使用 Spring Data JPA 可以极大提高开发效率！
>
> Spring Data JPA 让我们解脱了DAO层的操作，基本上所有CRUD都可以依赖于它来实现,在实际的工作工程中，推荐使用Spring Data JPA + ORM（如：hibernate）完成操作，这样在切换不同的ORM框架时提供了极大的方便，同时也使数据库层操作更加简单，方便解耦

![内部结构](data:image/png;base64,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)

### JPA与spring的关系

> JPA是一套规范，内部是有接口和抽象类组成的。hibernate是一套成熟的ORM框架，而且Hibernate实现了JPA规范，所以也可以称hibernate为JPA的一种实现方式，我们使用JPA的API编程，意味着站在更高的角度上看待问题（面向接口编程）
>
> Spring Data JPA是Spring提供的一套对JPA操作更加高级的封装，是在JPA规范下的专门用来进行数据持久化的解决方案。



### 搭建环境

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>cn.itcast</groupId>
    <artifactId>jpa-day2</artifactId>
    <version>1.0-SNAPSHOT</version>

    <properties>
        <spring.version>5.0.2.RELEASE</spring.version>
        <hibernate.version>5.0.7.Final</hibernate.version>
        <slf4j.version>1.6.6</slf4j.version>
        <log4j.version>1.2.12</log4j.version>
        <c3p0.version>0.9.1.2</c3p0.version>
        <mysql.version>8.0.20</mysql.version>
    </properties>

    <dependencies>
        <!-- junit单元测试 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!-- spring beg -->
        <dependency>
            <groupId>org.aspectj</groupId>
            <artifactId>aspectjweaver</artifactId>
            <version>1.6.8</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-aop</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context-support</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- spring对orm框架的支持包-->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-beans</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-core</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- spring end -->

        <!-- hibernate beg -->
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-core</artifactId>
            <version>${hibernate.version}</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-entitymanager</artifactId>
            <version>${hibernate.version}</version>
        </dependency>
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.2.1.Final</version>
        </dependency>
        <!-- hibernate end -->

        <!-- c3p0 beg -->
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>${c3p0.version}</version>
        </dependency>
        <!-- c3p0 end -->

        <!-- log end -->
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>${log4j.version}</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
        </dependency>

        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-log4j12</artifactId>
            <version>${slf4j.version}</version>
        </dependency>
        <!-- log end -->


        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>

        <!-- spring data jpa 的坐标-->
        <dependency>
            <groupId>org.springframework.data</groupId>
            <artifactId>spring-data-jpa</artifactId>
            <version>1.9.0.RELEASE</version>
        </dependency>

        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>${spring.version}</version>
        </dependency>

        <!-- el beg 使用spring data jpa 必须引入 -->
        <dependency>
            <groupId>javax.el</groupId>
            <artifactId>javax.el-api</artifactId>
            <version>2.2.4</version>
        </dependency>

        <dependency>
            <groupId>org.glassfish.web</groupId>
            <artifactId>javax.el</artifactId>
            <version>2.2.4</version>
        </dependency>
        <!-- el end -->
    </dependencies>

</project>
```



### 配置文件文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:jdbc="http://www.springframework.org/schema/jdbc" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:jpa="http://www.springframework.org/schema/data/jpa" xmlns:task="http://www.springframework.org/schema/task"
       xsi:schemaLocation="
		http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
		http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
		http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
		http://www.springframework.org/schema/jdbc http://www.springframework.org/schema/jdbc/spring-jdbc.xsd
		http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
		http://www.springframework.org/schema/data/jpa
		http://www.springframework.org/schema/data/jpa/spring-jpa.xsd">

    <!--spring 和 spring data jpa的配置-->

    <!-- 1.创建entityManagerFactory对象交给spring容器管理-->
    <bean id="entityManagerFactoty" class="org.springframework.orm.jpa.LocalContainerEntityManagerFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <!--配置的扫描的包（实体类所在的包） -->
        <property name="packagesToScan" value="cn.itcast.domain" />
        <!-- jpa的实现厂家 -->
        <property name="persistenceProvider">
            <bean class="org.hibernate.jpa.HibernatePersistenceProvider"/>
        </property>

        <!--jpa的供应商适配器 -->
        <property name="jpaVendorAdapter">
            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaVendorAdapter">
                <!--配置是否自动创建数据库表 -->
                <property name="generateDdl" value="false" />
                <!--指定数据库类型 -->
                <property name="database" value="MYSQL" />
                <!--数据库方言：支持的特有语法 -->
                <property name="databasePlatform" value="org.hibernate.dialect.MySQLDialect" />
                <!--是否显示sql -->
                <property name="showSql" value="true" />
            </bean>
        </property>

        <!--jpa的方言 ：高级的特性 -->
        <property name="jpaDialect" >
            <bean class="org.springframework.orm.jpa.vendor.HibernateJpaDialect" />
        </property>

    </bean>

    <!--2.创建数据库连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="user" value="root"></property>
        <property name="password" value="111111"></property>
        <property name="jdbcUrl" value="jdbc:mysql:///jpa" ></property>
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
    </bean>

    <!--3.整合spring dataJpa-->
    <jpa:repositories base-package="cn.itcast.dao" transaction-manager-ref="transactionManager"
                   entity-manager-factory-ref="entityManagerFactoty" ></jpa:repositories>

    <!--4.配置事务管理器 -->
    <bean id="transactionManager" class="org.springframework.orm.jpa.JpaTransactionManager">
        <property name="entityManagerFactory" ref="entityManagerFactoty"></property>
    </bean>

    <!-- 4.txAdvice-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="insert*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="get*" read-only="true"/>
            <tx:method name="find*" read-only="true"/>
            <tx:method name="*" propagation="REQUIRED"/>
        </tx:attributes>
    </tx:advice>

    <!-- 5.aop-->
    <aop:config>
        <aop:pointcut id="pointcut" expression="execution(* cn.itcast.service.*.*(..))" />
        <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut" />
    </aop:config>


    <!--5.声明式事务 -->

    <!-- 6. 配置包扫描-->
    <context:component-scan base-package="cn.itcast" ></context:component-scan>
</beans>
```

实体类

dao层

只需要编写dao层接口，不需要编写dao层接口的实现类

dao层接口规范

1. 需要继承两个接口（JpaRepository，JpaSpecificationExecutor）
2. 需要提供响应的泛型

执行方法

-  `findOne(id)` ：根据id查询
- `save(customer)`:保存或者更新（依据：传递的实体类对象中，是否包含id属性）
- `delete(id) `：根据id删除
- `findAll() `: 查询全部

```java
/**
 * 符合SpringDataJpa的dao层接口规范
 *      JpaRepository<操作的实体类类型，实体类中主键属性的类型>
 *          * 封装了基本CRUD操作
 *      JpaSpecificationExecutor<操作的实体类类型>
 *          * 封装了复杂查询（分页）
 */
public interface CustomerDao extends JpaRepository<Customer,Long> , JpaSpecificationExecutor<Customer> {
    
}
```



### 执行

```java
@RunWith(SpringJUnit4ClassRunner.class) //声明spring提供的单元测试环境
@ContextConfiguration(locations = "classpath:applicationContext.xml")//指定spring容器的配置信息
public class CustomerDaoTest {
    @Autowired
    private CustomerDao customerDao;

    /**
     * 根据id查询
     */
    @Test
    public void testFindOne() {
        Customer customer = customerDao.findOne(2l);
        System.out.println(customer);
    }

    /**
     * save : 保存或者更新
     *      根据传递的对象是否存在主键id，
     *      如果没有id主键属性：保存
     *      存在id主键属性，根据id查询数据，更新数据
     */
    @Test
    public void testSave() {
        Customer customer  = new Customer();
        customer.setCustName("黑马程序员");
        customer.setCustLevel("vip");
        customer.setCustIndustry("it教育");
        customerDao.save(customer);
    }

    @Test
    public void testUpdate() {
        Customer customer  = new Customer();
        customer.setCustId(4l);
        customer.setCustName("黑马程序员很厉害");
        customerDao.save(customer);
    }

    @Test
    public void testDelete () {
        customerDao.delete(3l);
    }


    /**
     * 查询所有
     */
    @Test
    public void testFindAll() {
        List<Customer> list = customerDao.findAll();
        for(Customer customer : list) {
            System.out.println(customer);
        }
    }

    /**
     * 测试统计查询：查询客户的总数量
     *      count:统计总条数
     */
    @Test
    public void testCount() {
        long count = customerDao.count();//查询全部的客户数量
        System.out.println(count);
    }

    /**
     * 测试：判断id为4的客户是否存在
     *      1. 可以查询以下id为4的客户
     *          如果值为空，代表不存在，如果不为空，代表存在
     *      2. 判断数据库中id为4的客户的数量
     *          如果数量为0，代表不存在，如果大于0，代表存在
     */
    @Test
    public void  testExists() {
        boolean exists = customerDao.exists(4l);
        System.out.println("id为4的客户 是否存在："+exists);
    }


    /**
     * 根据id从数据库查询
     *      @Transactional : 保证getOne正常运行
     *
     *  findOne：
     *      em.find()           :立即加载
     *  getOne：
     *      em.getReference     :延迟加载
     *      * 返回的是一个客户的动态代理对象
     *      * 什么时候用，什么时候查询
     */
    @Test
    @Transactional
    public void  testGetOne() {
        Customer customer = customerDao.getOne(4l);
        System.out.println(customer);
    }

}

```



### dao层接口的代码（JPQL语言）

> 重申一遍！dao层没有实现类

```java
/**
 * 案例：根据客户名称查询客户
 *      使用jpql的形式查询
 *  jpql：from Customer where custName = ?
 *
 *  配置jpql语句，使用的@Query注解
 */
@Query(value="from Customer where custName = ?")
public Customer findJpql(String custName);
/**
 * 案例：根据客户名称和客户id查询客户
 *      jpql： from Customer where custName = ? and custId = ?
 *
 *  对于多个占位符参数
 *      赋值的时候，默认的情况下，占位符的位置需要和方法参数中的位置保持一致
 *
 *  可以指定占位符参数的位置
 *      ? 索引的方式，指定此占位的取值来源
 */
@Query(value = "from Customer where custName = ?2 and custId = ?1")
public Customer findCustNameAndId(Long id,String name);
/**
 * 使用jpql完成更新操作
 *      案例 ： 根据id更新，客户的名称
 *          更新4号客户的名称，将名称改为“黑马程序员”
 *
 *  sql  ：update cst_customer set cust_name = ? where cust_id = ?
 *  jpql : update Customer set custName = ? where custId = ?
 *
 *  @Query : 代表的是进行查询
 *      * 声明此方法是用来进行更新操作
 *  @Modifying
 *      * 当前执行的是一个更新操作
 *
 */
@Query(value = " update Customer set custName = ?2 where custId = ?1 ")
@Modifying
public void updateCustomer(long custId,String custName);
/**
 * 使用sql的形式查询：
 *     查询全部的客户
 *  sql ： select * from cst_customer;
 *  Query : 配置sql查询
 *      value ： sql语句
 *      nativeQuery ： 查询方式
 *          true ： sql查询
 *          false：jpql查询
 *
 */
//@Query(value = " select * from cst_customer" ,nativeQuery = true)
@Query(value="select * from cst_customer where cust_name like ?1",nativeQuery = true)
public List<Object [] > findSql(String name);
/**
 * 方法名的约定：
 *      findBy : 查询
 *            对象中的属性名（首字母大写） ： 查询的条件
 *            CustName
 *            * 默认情况 ： 使用 等于的方式查询
 *                  特殊的查询方式
 *
 *  findByCustName   --   根据客户名称查询
 *
 *  再springdataJpa的运行阶段
 *          会根据方法名称进行解析  findBy    from  xxx(实体类)
 *                                      属性名称      where  custName =
 *
 *      1.findBy  + 属性名称 （根据属性名称进行完成匹配的查询=）
 *      2.findBy  + 属性名称 + “查询方式（Like | isnull）”
 *          findByCustNameLike
 *      3.多条件查询
 *          findBy + 属性名 + “查询方式”   + “多条件的连接符（and|or）”  + 属性名 + “查询方式”
 */
public Customer findByCustName(String custName);
public List<Customer> findByCustNameLike(String custName);
//使用客户名称模糊匹配和客户所属行业精准匹配的查询
public Customer findByCustNameLikeAndCustIndustry(String custName,String custIndustry);
```

### 命名约定

findBy : 查询

-   对象中的属性名（首字母大写） ： 查询的条件

  ​           CustName

   findByCustName   --   根据客户名称查询

- 再springdataJpa的运行阶段会根据方法名称进行解析  findBy    from(属性名称)  xxx(实体类) (where  custName )

  1.findBy  + 属性名称 （根据属性名称进行完成匹配的查询=）
   2.findBy  + 属性名称 + “查询方式（Like | isnull）”
           findByCustNameLike
   3.多条件查询
           findBy + 属性名 + “查询方式”   + “多条件的连接符（and|or）”  + 属性名 + “查询方式”

  

```java
public Customer findByCustName(String custName);
    public List<Customer> findByCustNameLike(String custName);
    //使用客户名称模糊匹配和客户所属行业精准匹配的查询
    public Customer findByCustNameLikeAndCustIndustry(String custName,String custIndustry);
```



<!-- 2020.07.06 -->

## 动态查询

### 自定义查询条件

1. 实现Specification接口（提供泛型：查询的对象类型）
2. 实现toPredicate方法（构造查询条件）
3. 需要借助方法参数中的两个参数
   + root：获取需要查询的对象属性    
   + CriteriaBuilder：构造查询条件的，内部封装了很多的查询条件（模糊匹配，精准匹配）

样本：

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:applicationContext.xml")
public class SpecTest {

    @Autowired
    private CustomerDao customerDao;

    /**
     * 根据条件，查询单个对象
     *
     */
    @Test
    public void testSpec() {
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                //1.获取比较的属性
                Path<Object> custName = root.get("custName");
                //2.构造查询条件  ：    select * from cst_customer where cust_name = '传智播客'
                /**
                 * 第一个参数：需要比较的属性（path对象）
                 * 第二个参数：当前需要比较的取值
                 */
                Predicate predicate = cb.equal(custName, "传智播客");//进行精准的匹配  （比较的属性，比较的属性的取值）
                return predicate;
            }
        };
        Customer customer = customerDao.findOne(spec);
        System.out.println(customer);
    }

    /**
     * 多条件查询
     *      案例：根据客户名（传智播客）和客户所属行业查询（it教育）
     *
     */
    @Test
    public void testSpec1() {
        /**
         *  root:获取属性
         *      客户名
         *      所属行业
         *  cb：构造查询
         *      1.构造客户名的精准匹配查询
         *      2.构造所属行业的精准匹配查询
         *      3.将以上两个查询联系起来
         */
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                Path<Object> custName = root.get("custName");//客户名
                Path<Object> custIndustry = root.get("custIndustry");//所属行业

                //构造查询
                //1.构造客户名的精准匹配查询
                Predicate p1 = cb.equal(custName, "传智播客");//第一个参数，path（属性），第二个参数，属性的取值
                //2..构造所属行业的精准匹配查询
                Predicate p2 = cb.equal(custIndustry, "it教育");
                //3.将多个查询条件组合到一起：组合（满足条件一并且满足条件二：与关系，满足条件一或满足条件二即可：或关系）
                Predicate and = cb.and(p1, p2);//以与的形式拼接多个查询条件
                // cb.or();//以或的形式拼接多个查询条件
                return and;
            }
        };
        Customer customer = customerDao.findOne(spec);
        System.out.println(customer);
    }


    /**
     * 案例：完成根据客户名称的模糊匹配，返回客户列表
     *      客户名称以 ’传智播客‘ 开头
     *
     * equal ：直接的到path对象（属性），然后进行比较即可
     * gt，lt,ge,le,like : 得到path对象，根据path指定比较的参数类型，再去进行比较
     *      指定参数类型：path.as(类型的字节码对象)
     */
    @Test
    public void testSpec3() {
        //构造查询条件
        Specification<Customer> spec = new Specification<Customer>() {
            @Override
            public Predicate toPredicate(Root<Customer> root, CriteriaQuery<?> query, CriteriaBuilder cb) {
                //查询属性：客户名
                Path<Object> custName = root.get("custName");
                //查询方式：模糊匹配
                Predicate like = cb.like(custName.as(String.class), "传智播客%");
                return like;
            }
        };
//        List<Customer> list = customerDao.findAll(spec);
//        for (Customer customer : list) {
//            System.out.println(customer);
//        }
        //添加排序
        //创建排序对象,需要调用构造方法实例化sort对象
        //第一个参数：排序的顺序（倒序，正序）
        //   Sort.Direction.DESC:倒序
        //   Sort.Direction.ASC ： 升序
        //第二个参数：排序的属性名称
        Sort sort = new Sort(Sort.Direction.DESC,"custId");
        List<Customer> list = customerDao.findAll(spec, sort);
        for (Customer customer : list) {
            System.out.println(customer);
        }
    }

    /**
     * 分页查询
     *      Specification: 查询条件
     *      Pageable：分页参数
     *          分页参数：查询的页码，每页查询的条数
     *          findAll(Specification,Pageable)：带有条件的分页
     *          findAll(Pageable)：没有条件的分页
     *  返回：Page（springDataJpa为我们封装好的pageBean对象，数据列表，共条数）
     */
    @Test
    public void testSpec4() {

        Specification spec = null;
        //PageRequest对象是Pageable接口的实现类
        /**
         * 创建PageRequest的过程中，需要调用他的构造方法传入两个参数
         *      第一个参数：当前查询的页数（从0开始）
         *      第二个参数：每页查询的数量
         */
        Pageable pageable = new PageRequest(0,2);
        //分页查询
        Page<Customer> page = customerDao.findAll(null, pageable);
        System.out.println(page.getContent()); //得到数据集合列表
        System.out.println(page.getTotalElements());//得到总条数
        System.out.println(page.getTotalPages());//得到总页数
    }
}

```



## 多表查询

<!--515-->