

## 准备

1. 创建jar

   ```xml
   <dependency>
       <groupId>org.springframework</groupId>
       <artifactId>spring-context</artifactId>
       <version>5.2.7.RELEASE</version>
   </dependency>
   ```

2. 制作三层架构

   Dao:

   ```java
   public class AccountDaoImpl implements IAccountDao {
       public  void saveAccount(){
           System.out.println("保存了账户");
       }
   }
   ```

   service:

   ```java
   public class AccountServiceImpl implements IAccountService {
   
       private IAccountDao accountDao ;
   
       public AccountServiceImpl(){
           System.out.println("对象创建了");
       }
   
       public void  saveAccount(){
           accountDao.saveAccount();
       }
   }
   ```

   配置bean

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd">
   
       <!--把对象的创建交给spring来管理-->
       <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
   
       <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl"></bean>
   </beans>
   ```

   

   实现

   ```java
   public class Client {
   
       /**
        * 获取spring的Ioc核心容器，并根据id获取对象
        *
        * ApplicationContext的三个常用实现类：
        *      ClassPathXmlApplicationContext：它可以加载类路径下的配置文件，要求配置文件必须在类路径下。不在的话，加载不了。(更常用)
        *      FileSystemXmlApplicationContext：它可以加载磁盘任意路径下的配置文件(必须有访问权限）
        *
        *      AnnotationConfigApplicationContext：它是用于读取注解创建容器的，是明天的内容。
        *
        * 核心容器的两个接口引发出的问题：
        *  ApplicationContext:     单例对象适用              采用此接口
        *      它在构建核心容器时，创建对象采取的策略是采用立即加载的方式。也就是说，只要一读取完配置文件马上就创建配置文件中配置的对象。
        *
        *  BeanFactory:            多例对象使用
        *      它在构建核心容器时，创建对象采取的策略是采用延迟加载的方式。也就是说，什么时候根据id获取对象了，什么时候才真正的创建对象。
        * @param args
        */
       public static void main(String[] args) {
           //1.获取核心容器对象
           ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
   //        ApplicationContext ac = new FileSystemXmlApplicationContext("C:\\Users\\zhy\\Desktop\\bean.xml");
           //2.根据id获取Bean对象
           IAccountService as  = (IAccountService)ac.getBean("accountService");
           IAccountDao adao = ac.getBean("accountDao",IAccountDao.class);
   
           System.out.println(as);
           System.out.println(adao);
           as.saveAccount();
   
   
           //--------BeanFactory----------
   //        Resource resource = new ClassPathResource("bean.xml");
   //        BeanFactory factory = new XmlBeanFactory(resource);
   //        IAccountService as  = (IAccountService)factory.getBean("accountService");
   //        System.out.println(as);
       }
   }
   ```

sql

```sql
create table account(
	id int primary key auto_increment,
	name varchar(40),
	money float
)character set utf8 collate utf8_general_ci;

insert into account(name,money) values('aaa',1000);
insert into account(name,money) values('bbb',1000);
insert into account(name,money) values('ccc',1000);
```




## bean工厂

创建bean的三种方式

1. 使用默认构造函数创建。

   - 在spring的配置文件中使用bean标签，配以id和class属性之后，且没有其他属性和标签时。
   -  采用的就是默认构造函数创建bean对象，此时如果类中没有默认构造函数，则对象无法创建。

   ```xml
   <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
   ```

   

2. 使用普通工厂中的方法创建对象（使用某个类中的方法创建对象，并存入spring容器）

   ```xml
   <bean id="instanceFactory" class="com.itheima.factory.InstanceFactory"></bean>
   <bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>
   ```

3. 使用工厂中的静态方法创建对象（使用某个类中的静态方法创建对象，并存入spring容器)

   ```xml
   <bean id="accountService" class="com.itheima.factory.StaticFactory" factory-method="getAccountService"></bean>
   ```



bean的作用范围调整

bean标签的scope属性：

- 作用：用于指定bean的作用范围

-  取值： 常用的就是单例的和多例的

  - singleton：单例的（默认值）
  -  prototype：多例的
  - request：作用于web应用的请求范围
  - session：作用于web应用的会话范围
  - global-session：作用于集群环境的会话范围（全局会话范围），当不是集群环境时，它就是session

  ```xml
  <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl" scope="prototype"></bean>
  ```

bean对象的生命周期

- 单例对象
  - 出生：当容器创建时对象出生
  - 活着：只要容器还在，对象一直活着
  - 死亡：容器销毁，对象消亡
  -  总结：单例对象的生命周期和容器相同
  
- 多例对象

    - 出生：当我们使用对象时spring框架为我们创建
    -   活着：对象只要是在使用过程中就一直活着。
    -  死亡：当对象长时间不用，且没有别的对象引用时，由Java的垃圾回收器回收

    ```xml
    <!-- 配置生命周期的相应方法 -->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"
              scope="prototype" init-method="init" destroy-method="destroy"></bean>
    ```



###  创建id对象

#### spring中的依赖注入

- 依赖注入：

  - Dependency Injection

- IOC的作用：
  - 降低程序间的耦合（依赖关系）
- 依赖关系的管理：
  - 以后都交给spring来维护
在当前类需要用到其他类的对象，由spring为我们提供，我们只需要在配置文件中说明
依赖关系的维护： 就称之为依赖注入。
依赖注入：
- 能注入的数据：有三类
  - 基本类型和String
  - 其他bean类型（在配置文件中或者注解配置过的bean）
  - 复杂类型/集合类型
- 注入的方式：有三种
  - 第一种：使用构造函数提供
  - 第二种：使用set方法提供
  - 第三种：使用注解提供（明天的内容）

#### 1.构造函数注入：

- 使用的标签:constructor-arg
- 标签出现的位置：bean标签的内部
标签中的属性
- type：用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
- index：用于指定要注入的数据给构造函数中指定索引位置的参数赋值。索引的位置是从0开始
- name：用于指定给构造函数中指定名称的参数赋值                                        常用的

以上三个用于指定给构造函数中哪个参数赋值

- value：用于提供基本类型和String类型的数据
- ref：用于指定其他的bean类型数据。它指的就是在spring的Ioc核心容器中出现过的bean对象
优势：
    在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功。
弊端：
    改变了bean对象的实例化方式，使我们在创建对象时，如果用不到这些数据，也必须提供。
```xml
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <constructor-arg name="name" value="泰斯特"></constructor-arg>
        <constructor-arg name="age" value="18"></constructor-arg>
        <constructor-arg name="birthday" ref="now"></constructor-arg>
    </bean>
```

#### 2.set方法注入                更常用的方式
- 涉及的标签：property
- 出现的位置：bean标签的内部
- 标签的属性
  - name：用于指定注入时所调用的set方法名称
  - value：用于提供基本类型和String类型的数据
  - ref：用于指定其他的bean类型数据。它指的就是在spring的I
优势：
    创建对象时没有明确的限制，可以直接使用默认构造函数
弊端：
    如果有某个成员必须有值，则获取对象是有可能set方法没有执行。

```xml
<bean id="accountService2" class="com.itheima.service.impl.AccountServiceImpl2">
    <property name="name" value="TEST" ></property>
    <property name="age" value="21"></property>
    <property name="birthday" ref="now"></property>
</bean>
```

#### 3.复杂方式注入

```xml
<bean id="accountService3" class="com.itheima.service.impl.AccountServiceImpl3">
    <property name="myStrs">
        <set>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </set>
    </property>
    <property name="myList">
        <array>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </array>
    </property>
    <property name="mySet">
        <list>
            <value>AAA</value>
            <value>BBB</value>
            <value>CCC</value>
        </list>
    </property>
    <property name="myMap">
        <props>
            <prop key="testC">ccc</prop>
            <prop key="testD">ddd</prop>
        </props>
    </property>
    <property name="myProps">
        <map>
            <entry key="testA" value="aaa"></entry>
            <entry key="testB">
                <value>BBB</value>
            </entry>
        </map>
    </property>
</bean>
```



## IOC容器

### 注解导入方式



 账户的业务层实现类
 曾经XML的配置：

```xml
  <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"
        scope=""  init-method="" destroy-method="">
      <property name=""  value="" | ref=""></property>
  </bean>
```

-  用于创建对象的

  > 他们的作用就和在XML配置文件中编写一个<bean>标签实现的功能是一样的

  - Component:
              作用：用于把当前类对象存入spring容器中
              属性：
                  value：用于指定bean的id。当我们不写时，它的默认值是当前类名，且首字母改小写。

  - Controller：一般用在表现层
  - Service：一般用在业务层
  - Repository：一般用在持久层

  ​      以上三个注解他们的作用和属性与Component是一模一样。
  ​      他们三个是spring框架为我们提供明确的三层使用的注解，使我们的三层对象更加清晰

-  用于注入数据的

  > 他们的作用就和在xml配置文件中的bean标签中写一个<property>标签的作用是一样的

  - Autowired:
              作用：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功
                    如果ioc容器中没有任何bean的类型和要注入的变量类型匹配，则报错。
                    如果Ioc容器中有多个类型匹配时：
              出现位置：
                  可以是变量上，也可以是方法上
              细节：
                  在使用注解注入时，set方法就不是必须的了。
  -  Qualifier:
              作用：在按照类中注入的基础之上再按照名称注入。它在给类成员注入时不能单独使用。但是在给方法参数注入时可以（稍后我们讲）
              属性：
                  value：用于指定注入bean的id。
  -  Resource
              作用：直接按照bean的id注入。它可以独立使用
              属性：
                  name：用于指定bean的id。
          以上三个注入都只能注入其他bean类型的数据，而基本类型和String类型无法使用上述注解实现。
          另外，集合类型的注入只能通过XML来实现。
  -  Value
              作用：用于注入基本类型和String类型的数据
              属性：
                  value：用于指定数据的值。它可以使用spring中SpEL(也就是spring的el表达式）
                          SpEL的写法：${表达式}

-  用于改变作用范围的

  >  他们的作用就和在bean标签中使用scope属性实现的功能是一样的

  - Scope

     作用：用于指定bean的作用范围

     属性：

    -   value：指定范围的取值。常用取值：singleton prototype

-  和生命周期相关 了解

  > 他们的作用就和在bean标签中使用init-method和destroy-methode的作用是一样的

  - PreDestroy
              作用：用于指定销毁方法      
  - PostConstruct
              作用：用于指定初始化方法

#### 代码

bean.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--告知spring在创建容器时要扫描的包，配置所需要的标签不是在beans的约束中，而是一个名称为
    context名称空间和约束中-->
    <context:component-scan base-package="com.itheima"></context:component-scan>
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl">
        <!-- 注入dao -->
        <property name="accountDao" ref="accountDao"></property>
    </bean>
    <!--配置Dao对象-->
    <bean id="accountDao" class="com.itheima.dao.impl.AccountDaoImpl">
        <!-- 注入QueryRunner -->
        <property name="runner" ref="runner"></property>
    </bean>
</beans>
```

dao:

```java
@Repository("accountDao1")
public class AccountDaoImpl implements IAccountDao {
    public void saveAccount() {
        System.out.println("保存了账户1111111111111");
    }
}

@Repository("accountDao2")
public class AccountDaoImpl2  implements IAccountDao {
    public  void saveAccount(){
        System.out.println("保存了账户2222222222222");
    }
}
```

service

```java
@Service("accountService")
//@Scope("prototype")
public class AccountServiceImpl implements IAccountService {

//    @Autowired
//    @Qualifier("accountDao1")
    @Resource(name = "accountDao2")
    private IAccountDao accountDao = null;

    @PostConstruct
    public void  init(){
        System.out.println("初始化方法执行了");
    }

    @PreDestroy
    public void  destroy(){
        System.out.println("销毁方法执行了");
    }

    public void  saveAccount(){
        accountDao.saveAccount();
    }
}

```

client

```java
public class Client {
    /**
     * @param args
     */
    public static void main(String[] args) {
        //1.获取核心容器对象
//        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        ClassPathXmlApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.根据id获取Bean对象
        IAccountService as = (IAccountService) ac.getBean("accountService");
//        IAccountService as2  = (IAccountService)ac.getBean("accountService");
//        System.out.println(as);
//        IAccountDao adao = ac.getBean("accountDao",IAccountDao.class);
//        System.out.println(adao);
//        System.out.println(as == as2);
        as.saveAccount();
        ac.close();
    }
}

```

### 实例

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>5.0.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>5.0.2.RELEASE</version>
    </dependency>
    <dependency>
        <groupId>commons-dbutils</groupId>
        <artifactId>commons-dbutils</artifactId>
        <version>1.4</version>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.6</version>
    </dependency>
    <dependency>
        <groupId>c3p0</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.1.2</version>
    </dependency>
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.12</version>
    </dependency>
</dependencies>
```





test

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:bean.xml")
public class AccountServiceTest {
    @Autowired
    private  IAccountService as;
    
        @Test
    public void testFindAll() {
        //3.执行方法
        List<Account> accounts = as.findAllAccount();
        for(Account account : accounts){
            System.out.println(account);
        }
    }
}
```



### 注解方法

扫包

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 告知spring在创建容器时要扫描的包 -->
    <context:component-scan base-package="com.itheima"></context:component-scan>
    <!--没法扫的写出来就自己写-->
    <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--注入数据源-->
        <constructor-arg name="ds" ref="dataSource"></constructor-arg>
    </bean>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库的必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy"></property>
        <property name="user" value="root"></property>
        <property name="password" value="1234"></property>
    </bean>
</beans>
```

dao

```java
@Repository("accountDao")
public class AccountDaoImpl implements IAccountDao {

    @Autowired
    private QueryRunner runner;

    @Override
    public List<Account> findAllAccount() {
        try{
            return runner.query("select * from account",new BeanListHandler<Account>(Account.class));
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public Account findAccountById(Integer accountId) {
        try{
            return runner.query("select * from account where id = ? ",new BeanHandler<Account>(Account.class),accountId);
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void saveAccount(Account account) {
        try{
            runner.update("insert into account(name,money)values(?,?)",account.getName(),account.getMoney());
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void updateAccount(Account account) {
        try{
            runner.update("update account set name=?,money=? where id=?",account.getName(),account.getMoney(),account.getId());
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void deleteAccount(Integer accountId) {
        try{
            runner.update("delete from account where id=?",accountId);
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}

```

service

```java
@Service("accountService")
public class AccountServiceImpl implements IAccountService{

    @Autowired
    private IAccountDao accountDao;

    @Override
    public List<Account> findAllAccount() {
        return accountDao.findAllAccount();
    }

    @Override
    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);
    }

    @Override
    public void saveAccount(Account account) {
        accountDao.saveAccount(account);
    }

    @Override
    public void updateAccount(Account account) {
        accountDao.updateAccount(account);
    }

    @Override
    public void deleteAccount(Integer acccountId) {
        accountDao.deleteAccount(acccountId);
    }
}

```

test

```java
public class AccountServiceTest {

    @Test
    public void testFindAll() {
        //1.获取容易
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        //2.得到业务层对象
        IAccountService as = ac.getBean("accountService",IAccountService.class);
        //3.执行方法
        List<Account> accounts = as.findAllAccount();
        for(Account account : accounts){
            System.out.println(account);
        }
    }
}
```





### 不用注解方法

jdbcConfig.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/eesy
jdbc.username=root
jdbc.password=1234
```

以上的东西不重要

#### 设计配置类

> 该类是一个配置类，它的作用和bean.xml是一样的

spring中的新注解
Configuration
    作用：指定当前类是一个配置类
    细节：当配置类作为AnnotationConfigApplicationContext对象创建的参数时，该注解可以不写。

- ComponentScan
       作用：用于通过注解指定spring在创建容器时要扫描的包
       属性：
           value：它和basePackages的作用是一样的，都是用于指定创建容器时要扫描的包。
                  我们使用此注解就等同于在xml中配置了:
                       <context:component-scan base-package="com.itheima"></co
- Bean
       作用：用于把当前方法的返回值作为bean对象存入spring的ioc容器中
       属性:
           name:用于指定bean的id。当不写时，默认值是当前方法的名称
       细节：
           当我们使用注解配置方法时，如果方法有参数，spring框架会去容器中查找有没有可用的bean对象。
           查找的方式和Autowired注解的作用是一样的
- Import
       作用：用于导入其他的配置类
       属性：
           value：用于指定其他配置类的字节码。
                   当我们使用Import的注解之后，有Import注解的类就父配置类，而导入的都是子配置类
- PropertySource
       作用：用于指定properties文件的位置
       属性：
           value：指定文件的名称和路径。
                   关键字：classpath，表示类路径下



```java
@ComponentScan("com.itheima")
@Import(JdbcConfig.class)
@PropertySource("classpath:jdbcConfig.properties")
public class SpringConfiguration {
}
```

设计数据类（外部代码设计）

```java
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("${jdbc.password}")
    private String password;

    /**
     * 用于创建一个QueryRunner对象
     * @param dataSource
     * @return
     */
    @Bean(name="runner")
    @Scope("prototype")
    public QueryRunner createQueryRunner(@Qualifier("ds2") DataSource dataSource){
        return new QueryRunner(dataSource);
    }

    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name="ds2")
    public DataSource createDataSource(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Bean(name="ds1")
    public DataSource createDataSource1(){
        try {
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/eesy02");
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

#### 正文

dao

```java
@Repository("accountDao")
public class AccountDaoImpl implements IAccountDao {
@Autowired
    private QueryRunner runner;

    @Override
    public List<Account> findAllAccount() {
        try{
            return runner.query("select * from account",new BeanListHandler<Account>(Account.class));
        }catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

service

```java
@Service("accountService")
public class AccountServiceImpl implements IAccountService{

    @Autowired
    private IAccountDao accountDao;

    @Override
    public List<Account> findAllAccount() {
        return accountDao.findAllAccount();
    }
}
```

test

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {

    @Autowired
    private IAccountService as = null;


    @Test
    public void testFindAll() {
        //3.执行方法
        List<Account> accounts = as.findAllAccount();
        for(Account account : accounts){
            System.out.println(account);
        }
    }
}
```

注意：

```
使用Junit单元测试：测试我们的配置
Spring整合junit的配置
     1、导入spring整合junit的jar(坐标)
     2、使用Junit提供的一个注解把原有的main方法替换了，替换成spring提供的
            @Runwith
     3、告知spring的运行器，spring和ioc创建是基于xml还是注解的，并且说明位置
         @ContextConfiguration
                 locations：指定xml文件的位置，加上classpath关键字，表示在类路径下
                 classes：指定注解类所在地位置
  当我们使用spring 5.x版本的时候，要求junit的jar必须是4.12及以上
```



### 内部原理

待写入

day03_eesy_01account

## AOP切面

### 动态代理

```
动态代理：
 特点：字节码随用随创建，随用随加载
 作用：不修改源码的基础上对方法增强
 分类：
     基于接口的动态代理
     基于子类的动态代理
 基于子类的动态代理：
     涉及的类：Enhancer
     提供者：第三方cglib库
 如何创建代理对象：
     使用Enhancer类中的create方法
 创建代理对象的要求：
     被代理类不能是最终类
 create方法的参数：
     Class：字节码
         它是用于指定被代理对象的字节码。
     Callback：用于提供增强的代码
         它是让我们写如何代理。我们一般都是些一个该接口的实现类，通常情况下都是匿名内部类，但不是必须的。
         此接口的实现类都是谁用谁写。
         我们一般写的都是该接口的子接口实现类：MethodInterceptor
```

```
动态代理：
 特点：字节码随用随创建，随用随加载
 作用：不修改源码的基础上对方法增强
 分类：
     基于接口的动态代理
     基于子类的动态代理
 基于接口的动态代理：
     涉及的类：Proxy
     提供者：JDK官方
 如何创建代理对象：
     使用Proxy类中的newProxyInstance方法
 创建代理对象的要求：
     被代理类最少实现一个接口，如果没有则不能使用
 newProxyInstance方法的参数：
     ClassLoader：类加载器
         它是用于加载代理对象字节码的。和被代理对象使用相同的类加载器。固定写法。
     Class[]：字节码数组
         它是用于让代理对象和被代理对象有相同方法。固定写法。
     InvocationHandler：用于提供增强的代码
         它是让我们写如何代理。我们一般都是些一个该接口的实现类，通常情况下都是匿名内部类，但不是必须的。
         此接口的实现类都是谁用谁写。
```

### spring中基于XML的AOP配置步骤

1、把通知Bean也交给spring来管理
2、使用aop:config标签表明开始AOP的配置
3、使用aop:aspect标签表明配置切面
        id属性：是给切面提供一个唯一标识
        ref属性：是指定通知类bean的Id。
4、在aop:aspect标签的内部使用对应标签来配置通知的类型
       ==我们现在示例是让printLog方法在切入点方法执行之前之前：所以是前置通知==
       aop:before：表示配置前置通知
            method属性：用于指定Logger类中哪个方法是前置通知
            pointcut属性：用于指定切入点表达式，该表达式的含义指的是对业务层中哪些方法增强
    切入点表达式的写法：
        关键字：execution(表达式)
        表达式：
            访问修饰符  返回值  包名.包名.包名...类名.方法名(参数列表)
        标准的表达式写法：
            `public void com.itheima.service.impl.AccountServiceImpl.saveAccount`

```
访问修饰符可以省略
    void com.itheima.service.impl.AccountServiceImpl.saveAccount()
返回值可以使用通配符，表示任意返回值
    * com.itheima.service.impl.AccountServiceImpl.saveAccount()
包名可以使用通配符，表示任意包。但是有几级包，就需要写几个*.
    * *.*.*.*.AccountServiceImpl.saveAccount())
包名可以使用..表示当前包及其子包
    * *..AccountServiceImpl.saveAccount()
类名和方法名都可以使用*来实现通配
    * *..*.*()
```

参数列表：

- 可以直接写数据类型：

  参数列表：

  - 基本类型直接写名称           int

  - 引用类型写包名.类名的方式   java.lang.String
  
- 可以使用通配符表示任意类型，但是必须有参数
  
- 可以使用..表示有无参数均可，有参数可以是任意类型
全通配写法:`* *..*.*(..)`

实际开发中切入点表达式的通常写法：
      切到业务层实现类下的所有方法`* com.itheima.service.impl.*.*(..)`

#### 实例

```java
public class Logger {

    /**
     * 用于打印日志：计划让其在切入点方法执行之前执行（切入点方法就是业务层方法）
     */
    public  void printLog(){
        System.out.println("Logger类中的pringLog方法开始记录日志了。。。");
    }
}
```

xml:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 配置srping的Ioc,把service对象配置进来-->
    <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"></bean>
    
    <!-- 配置Logger类 -->
    <bean id="logger" class="com.itheima.utils.Logger"></bean>

    <!--配置AOP-->
    <aop:config>
        <!--配置切面 -->
        <aop:aspect id="logAdvice" ref="logger">
            <!-- 配置通知的类型，并且建立通知方法和切入点方法的关联-->
            <aop:before method="printLog" pointcut="execution(* com.itheima.service.impl.*.*(..))"></aop:before>
        </aop:aspect>
    </aop:config>

</beans>
```



执行结果

> 每次执行时都有`Logger类中的pringLog方法开始记录日志了。。。`的消息

```
Logger类中的pringLog方法开始记录日志了。。。
执行了保存
Logger类中的pringLog方法开始记录日志了。。。
执行了更新1
Logger类中的pringLog方法开始记录日志了。。。
执行了删除
```



各种切面类型

我们先将切入点表达式给通用了

```xml
<aop:pointcut id="pt1" expression="execution(* com.itheima.service.impl.*.*(..))"></aop:pointcut>
```

前置

```xml
<aop:before method="beforePrintLog" pointcut-ref="pt1" ></aop:before>
```

后置

```xml
<aop:after-returning method="afterReturningPrintLog" pointcut-ref="pt1"></aop:after-returning>
```

异常

> 配置异常通知：在切入点方法执行产生异常之后执行。它和后置通知永远只能执行一个

```xml
<aop:after-throwing method="afterThrowingPrintLog" pointcut-ref="pt1"></aop:after-throwing>
```

最终

> 配置最终通知：无论切入点方法是否正常执行它都会在其后面执行

```xml
<aop:after method="afterPrintLog" pointcut-ref="pt1"></aop:after>
```

环绕通知

> 类似于装饰器模式的方法

```xml
<aop:around method="aroundPringLog" pointcut-ref="pt1"></aop:around>
```

问题：
     当我们配置了环绕通知之后，切入点方法没有执行，而通知方法执行了。
分析：
     通过对比动态代理中的环绕通知代码，发现动态代理的环绕通知有明确的切入点方法调用，而我们的代码中没有。
解决：
     Spring框架为我们提供了一个接口：ProceedingJoinPoint。该接口有一个方法proceed()，此方法就相当于明确调用切入点方法。
     该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用。
spring中的环绕通知：
     它是spring框架为我们提供的一种可以在代码中手动控制增强方法何时执行的方式。

```java
public Object aroundPringLog(ProceedingJoinPoint pjp){
    Object rtValue = null;
    try{
        Object[] args = pjp.getArgs();//得到方法执行所需的参数

        System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。前置");

        rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）

        System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。后置");

        return rtValue;
    }catch (Throwable t){
        System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。异常");
        throw new RuntimeException(t);
    }finally {
        System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。最终");
    }
}
```



结果

```
Logger类中的aroundPringLog方法开始记录日志了。。。前置
执行了保存
Logger类中的aroundPringLog方法开始记录日志了。。。异常
Logger类中的aroundPringLog方法开始记录日志了。。。最终
```





### 注解方法

xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!-- 配置spring创建容器时要扫描的包-->
    <context:component-scan base-package="com.itheima"></context:component-scan>

    <!-- 配置spring开启注解AOP的支持 -->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```



```java
@Component("logger")
@Aspect//表示当前类是一个切面类
public class Logger {

    @Pointcut("execution(* com.itheima.service.impl.*.*(..))")
    private void pt1(){}

    /**
     * 前置通知
     */
//    @Before("pt1()")
    public  void beforePrintLog(){
        System.out.println("前置通知Logger类中的beforePrintLog方法开始记录日志了。。。");
    }

    /**
     * 后置通知
     */
//    @AfterReturning("pt1()")
    public  void afterReturningPrintLog(){
        System.out.println("后置通知Logger类中的afterReturningPrintLog方法开始记录日志了。。。");
    }
    /**
     * 异常通知
     */
//    @AfterThrowing("pt1()")
    public  void afterThrowingPrintLog(){
        System.out.println("异常通知Logger类中的afterThrowingPrintLog方法开始记录日志了。。。");
    }

    /**
     * 最终通知
     */
//    @After("pt1()")
    public  void afterPrintLog(){
        System.out.println("最终通知Logger类中的afterPrintLog方法开始记录日志了。。。");
    }

    @Around("pt1()")
    public Object aroundPringLog(ProceedingJoinPoint pjp){
        Object rtValue = null;
        try{
            Object[] args = pjp.getArgs();//得到方法执行所需的参数

            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。前置");

            rtValue = pjp.proceed(args);//明确调用业务层方法（切入点方法）

            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。后置");

            return rtValue;
        }catch (Throwable t){
            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。异常");
            throw new RuntimeException(t);
        }finally {
            System.out.println("Logger类中的aroundPringLog方法开始记录日志了。。。最终");
        }
    }
}

```

