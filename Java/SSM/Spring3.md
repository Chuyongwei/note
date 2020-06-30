

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

   

## 开始

### bean工厂

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

