安装的jar包

下载jar
http://maven.springframework.org/release/org/springframework/spring/
spring-framework-4.3.9.RELEASE-dist.zip

> spring-aop-4.3.9.RELEASE
>
> spring-beans-4.3.9.RELEASE
>
> spring-context-4.3.9.RELEASE
>
> spring-core-4.3.9.RELEASE
>
> spring-expression-4.3.9.RELEASE
>
> commons-logging-1.1.1

### 准备

1.安装sts

2.链接对象

```java
package nuc.wcy.entiy;

import nuc.wcy.newinstance.HtmlCourse;
import nuc.wcy.newinstance.ICourse;
import nuc.wcy.newinstance.JavaCourse;

public class Student {
	private int stuno;
	private String stuname;
	private int stuage;
	
	public Student() {
		super();
	}
	public Student(int stuno, String stuname, int stuage) {
		super();
		this.stuno = stuno;
		this.stuname = stuname;
		this.stuage = stuage;
	}
	public int getStuno() {
		return stuno;
	}
	public void setStuno(int stuno) {
		this.stuno = stuno;
	}
	public String getStuname() {
		return stuname;
	}
	public void setStuname(String stuname) {
		this.stuname = stuname;
	}
	public int getStuage() {
		return stuage;
	}
	public void setStuage(int stuage) {
		this.stuage = stuage;
	}
	@Override
	public String toString() {
		return "Student [stuno=" + stuno + ", stuname=" + stuname + ", stuage=" + stuage + "]";
	}
}

```

3.配置xml

File -> new file-> other

<img src="data:image/png;base64,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" style="zoom:80%;" />

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="student" class="nuc.wcy.entiy.Student">
        <!--保证name与class对应-->
		<property name="stuno" value="2"></property>			
		<property name="stuname" value="ls"></property>
		<property name="stuage" value="24"></property>
	</bean>

</beans>

```

3.测试

```java
package nuc.wcy.entiy;

import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import nuc.wcy.newinstance.ICourse;

public class Test {
	public static void main(String[] args) {
        hh();
		learnCourseWithFactory();
	}
	
	public static void hh() {
		ApplicationContext content = new ClassPathXmlApplicationContext("application.xml");
		Student student = (Student)content.getBean("student");
		System.out.println(student);
	}
}

```

## 开始！

<font color="red" size="18">***赋值必须有无参构造!!!***</font>

# IOC(超级工厂)

## XML

1.IOC容器（工厂）

```xml
<bean id="javaCourse" class="nuc.wcy.newinstance.JavaCourse"></bean>
	<bean id="htmlCourse" class="nuc.wcy.newinstance.HtmlCourse"></bean>
```

2.引用工厂内容

```java
public void learnJava() {
//		ICourse course = new HtmlCourse();
//		course根据naem获取相应的课程
		
		
		//直接在ioc容器获取
		//2.直接在ioc中取
		ApplicationContext content = new ClassPathXmlApplicationContext("application.xml");
		ICourse course = (ICourse)content.getBean("javaCourse");
		course.learn();
	}
```

### 引用类：

>teacher：
>
>```java
>private String name;
>private int age;
>```

course：
>```java
>private String courseName;
>	private int courseHour;
>	private Teacher teacher;
>```
>
>

```xml
<bean id="teacher" class="nuc.wcy.entiy.Teacher">
		<property name="name" value = "zs"></property>
		<property name="age" value="18"></property>
	</bean>
	<bean id="course" class="nuc.wcy.entiy.Course">
		<property name="courseName" value="java"></property>
		<property name="courseHour" value="19"></property>
		<property name="teacher" ref="teacher"></property>
	</bean>
```

##### 自动匹配

`autowirte="buName"`使用引用类目之间自动匹配（根据id查找）

```xml
<bean id="course" class="nuc.wcy.entiy.Course" autowirte="buName" >
		<property name="courseName" value="java"></property>
		<property name="courseHour" value="19"></property>
	</bean>
```



### 注入（赋值）

set方式的依赖注入：

- 赋值默认使用set方法；依赖注入底层的方式实现

- ```
<property name="name" value = "zs"></property>
  		<property name="age" value="18"></property>
  ```
  
  2.构造方法
  
  ```xml
  <constructor-arg value="24" index="1"></constructor-arg>
  		<constructor-arg value="ls" index="0"></constructor-arg>
  ```
  
  ```xml
  <constructor-arg value="24" type="int" name="age"></constructor-arg>
  		<constructor-arg value="ls" type="String" name="name"></constructor-arg>
  
  ```
  
  3.p方法
  
  ```xml
  xmlns:p="http://www.springframework.org/schema/p"
  ```
  
  ```xml
  <bean id="teacher" class="nuc.wcy.entiy.Teacher" p:name="ls" p:age="12"></bean>
  ```

### 数组的注入

```xml
<bean id="all" class="nuc.wcy.entiy.AllColectionType">
		<property name="list">
		<list>
			<value>足球</value>
			<value>蓝球</value>
			<value>乒乓球</value>
		</list>
		</property>
		<property name="array">
		<array>
			<value>足球</value>
			<value>蓝球</value>
			<value>乒乓球</value>
		</array>
		</property>
		<property name="set">
		<set>
			<value>足球1</value>
			<value>蓝球1</value>
			<value>乒乓球1</value>
		</set>
		</property>
		<property name="map">
		<map>
			<entry>
				<key>
					<value>foot</value>
				</key>
				<value>足球2</value>
			</entry>
			<entry>
				<key>
					<value>bask</value>
				</key>
				<value>蓝球2</value>
			</entry>
			<entry>
				<key>
					<value>ping</value>
				</key>
				<value>乒乓球2</value>
			</entry>
		</map>
		</property>
	</bean>
```

> 同理平时的属性配置也可以这么做

```xml
	<bean id="teacher2" class="nuc.wcy.entiy.Teacher">
		<property name="name">
			<value type="java.lang.String">sdlk<![CDATA[<>$]]>afj</value>
		</property>
		<property name="age">
			<value type="int">18</value>
		</property>
		<!--当空时用null，“”等方法-->
	</bean>
```

> 转意用<![CDATA[<>$]]>或转移码



### 自动装配

<bean ... class="org.lanqiao.entity.Course"  autowire="byName|byType|constructor|no" >  byName本质是byId
byName:  自动寻找：其他bean的id值=该Course类的属性名
byType:  其他bean的类型(class)  是否与 该Course类的ref属性类型一致  （注意，此种方式 必须满足：当前Ioc容器中 只能有一个Bean满足条件  ）
constructor： 其他bean的类型(class)  是否与 该Course类的构造方法参数 的类型一致；此种方式的本质就是byType

可以在头文件中 一次性将该ioc容器的所有bean  统一设置成自动装配：
<beans xmlns="http://www.springframework.org/schema/beans"
...
default-autowire="byName">

自动装配虽然可以减少代码量，但是会降低程序的可读性，使用时需要谨慎。

### 注解（在java中自动导入）

使用注解定义bean：通过注解的形式 将bean以及相应的属性值 放入ioc容器

<context:component-scan base-package="org.lanqiao.dao">
\</context:component-scan\>Spring在启动的时候，会根据base-package在 该包中扫描所有类，查找这些类是否有注解`@Component("studentDao")`,如果有，则将该类 加入spring Ioc容器。

@Component细化：

dao层注解：`@Repository`
service层注解：`@Service`
控制器层注解：`@Controller`

`@Autowired`自动匹配数值

<!--2020.01.15-->

## properties

```properties
#bean.properties
accountService = com.wcy.service.impl.AccountServiceImpl
accountDao = com.wcy.dao.impl.AccountDaoImpl
```

```java
public class BeanFactory {
    //定义一个properties的对象
    static Properties props;

    static {
        try {
            //实例化对象
            props =  new Properties();
            //获取文件的流对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
        } catch (IOException e) {
            throw new ExceptionInInitializerError("初始化properties对象失败");
        }
    }

    /**
     * 根据Bean名称定义Bean对象
     * @param beanName
     * @return
     */
    public static Object getBean(String beanName){
        Object bean=null;
        //获取真正的类
        String beamPath = props.getProperty(beanName);
//        System.out.println(beamPath);
        try {
            bean = Class.forName(beamPath).newInstance();
        } catch (InstantiationException e) {
            e.printStackTrace();
        } catch (IllegalAccessException e) {
            e.printStackTrace();
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        return bean;
    }
}
```

多例

<img src="data:image/png;base64,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" style="zoom:125%;" />

单例：

```java
public class BeanFactory {
    //定义一个properties的对象
    static Properties props;

    //定义一个MAp,用于存放我们要创建的对象，我们把它称之为容器
    private  static Map<String,Object> beans;
    static {
        try {
            //实例化对象
            props =  new Properties();
            //获取文件的六对象
            InputStream in = BeanFactory.class.getClassLoader().getResourceAsStream("bean.properties");
            props.load(in);
            //实例化容器
            beans = new HashMap<String, Object>();
            //去获取配置文件中所有的类
            Enumeration keys =  props.keys();
            //遍历枚举
            while (keys.hasMoreElements()){
                String key = keys.nextElement().toString();
                //根据key获取value
                String beanPath = props.getProperty(key);
                //反射创键对象
                Object value = Class.forName(beanPath).newInstance();
                //把key和value存入容器中
                beans.put(key,value);
            }
        }  catch (Exception e) {
            throw new ExceptionInInitializerError("初始化properties对象失败");
        }
    }

    public static Object getBean(String beanName){
        return beans.get(beanName);
    }

```

## Bean

### Bean的无参构造三种创建方式

```java
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        IAccountService as1 = (IAccountService)ac.getBean("accountService");
        System.out.println(as1);
```

<font color="red"> 要有对应的无参构造</font>

1.默认方式创建

```xml
<!--    把对象的创建交给spring来管理-->
    <!-- 第一种方式：使用默认方式配置 -->
<!--    <bean id="accountService" class="com.wcy.service.impl.AccountServiceImpl"></bean>-->
```

2.使用普通工厂创建（获取get类方法）

通过将所有的类的get方法都放到一个工厂中，然后使用该类中方法创建类

```java
public class InstanceFactory {
    public IAccountService getAccountService(){
        return new AccountServiceImpl();
    }
}
```



```xml
<!--    2.使用普通工厂创建-->
<!--    <bean id="instanceFactory" class="com.wcy.factory.InstanceFactory"></bean>
    <bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>-->
```

3.使用静态工厂创建

可以直接使用的方法不用定义工厂类

```java
public class StaticFactory {
    public static IAccountService getAccounService(){
        return new AccountServiceImpl();
    }
}
```



```xml
<!--    3.使用静态工厂创建-->
    <bean id="accountService" class="com.wcy.factory.StaticFactory" factory-method="getAccounService"></bean>
```

### 构造函数的注入

<font color="red"> 要有对应的有参构造</font>

使用的标签：`constructor-arg`
标签出现的位置：bean内部
标签的属性
	   ` type`:根据类型赋值
	    `index`:根据索引复制
	    `name`:根据名称复制
 以上用于指定给构造函数中哪个参数数值
    `value`：用于进本类型的赋值
  `  ref`：用于指定其他的ban对象




```xml
bean id="accountService" class="com.wcy.service.impl.AccountServiceImpl">
        <constructor-arg name="name" value="test"></constructor-arg>
        <constructor-arg name="age" value="18"></constructor-arg>
        <constructor-arg name="brithday" ref="now"></constructor-arg>
    </bean>
    <bean id="now" class="java.util.Date"></bean>
```

### SET方法注入



​		涉及的的标签：property

​		出现位置：bean标签内部

​		标签的属性
​	   	` type`:根据类型赋值
​	    	`index`:根据索引复制
​	    	`name`:根据名称复制
 以上用于指定给构造函数中哪个参数数值
​    `value`：用于进本类型的赋值
​    `ref`：用于指定其他的ban对象

```
    <bean id="accountService2" class="com.wcy.service.impl.AccountServiceImpl">
        <property name="name" value="TEST"></property>
        <property name="age" value="18"></property>
        <property name="brithday" ref="now"></property>
    </bean>
```

### 数组的注入

```java
private String[] myStrs;
    private List<String> myList;
    private Set<String> mySet;
    private Map<String,String> myMap;
    private Properties myProps;
```

复杂类型的注入

- ​	set,array,list

- ​    map,property

  可以混用

```xml
    <bean id="accountService3" class="com.wcy.service.impl.AccountServiceImpl3">
        <property name="myStrs">
            <array>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </array>
        </property>
        <property name="myList">
            <list>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </list>
        </property>
        <property name="mySet">
            <set>
                <value>AAA</value>
                <value>BBB</value>
                <value>CCC</value>
            </set>
        </property>
        <property name="myMap">
            <map>
                <entry key="testA" value="AAA"></entry>
                <entry key="testB">
                    <value>BB</value>
                </entry>
            </map>
        </property>
        <property name="myProps">
        <props>
            <prop key="testC">ccc</prop>
            <prop key="testD">ddd</prop>
        </props>
        </property>
    </bean>
```

### xml和注解（在java中自动导入）

使用Bean.xml

```xml
  xmlns:context="http://www.springframework.org/schema/context"
```

使用注解定义bean：通过注解的形式 将bean以及相应的属性值 放入ioc容器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">
    <!--  告知spring在创建容器时要扫描的包-->
    <context:component-scan base-package="com.wcy"></context:component-scan>
</beans>
```

\</context:component-scan\>Spring在启动的时候，会根据base-package在 该包中扫描所有类，查找这些类是否有注解`@Component("studentDao")`,如果有，则将该类 加入spring Ioc容器。

@Component细化：

dao层注解：`@Repository`
service层注解：`@Service`
控制器层注解：`@Controller`

`@Autowired`自动匹配数值

```xml
<!--    自动注释的包：用于将注释的对象自动创建类-->
<context:component-scan base-package="com.wcy"></context:component-scan>
```

>
> 账户的业务层实现类
> 
> 曾经XML的配置：
>  <bean id="accountService" class="com.itheima.service.impl.AccountServiceImpl"
>        scope=""  init-method="" destroy-method="">
>      <property name=""  value="" | ref=""></property>
>  </bean>
> 
> 用于创建对象的
>      他们的作用就和在XML配置文件中编写一个<bean>标签实现的功能是一样的
>      Component:
>          作用：用于把当前类对象存入spring容器中
>          属性：
>              value：用于指定bean的id。当我们不写时，它的默认值是当前类名，且首字母改小写。
>      Controller：一般用在表现层
>      Service：一般用在业务层
>      Repository：一般用在持久层
>      以上三个注解他们的作用和属性与Component是一模一样。
>      他们三个是spring框架为我们提供明确的三层使用的注解，使我们的三层对象更加清晰
> 
> 用于注入数据的
>  他们的作用就和在xml配置文件中的bean标签中写一个<property>标签的作用是一样的
>      Autowired:
>          作用：自动按照类型注入。只要容器中有唯一的一个bean对象类型和要注入的变量类型匹配，就可以注入成功
>                如果ioc容器中没有任何bean的类型和要注入的变量类型匹配，则报错。
>                如果Ioc容器中有多个类型匹配时：
>          出现位置：
>              可以是变量上，也可以是方法上
>          细节：
>              在使用注解注入时，set方法就不是必须的了。
>      Qualifier:
>          作用：在按照类中注入的基础之上再按照名称注入。它在给类成员注入时不能单独使用。但是在给方法参数注入时可以（稍后我们讲）
>          属性：
>              value：用于指定注入bean的id。
>      Resource
>          作用：直接按照bean的id注入。它可以独立使用
>          属性：
>              name：用于指定bean的id。
>      以上三个注入都只能注入其他bean类型的数据，而基本类型和String类型无法使用上述注解实现。
>      另外，集合类型的注入只能通过XML来实现。
>     
>  Value
>          作用：用于注入基本类型和String类型的数据
>          属性：
>              value：用于指定数据的值。它可以使用spring中SpEL(也就是spring的el表达式）
>                      SpEL的写法：${表达式}
>     
> 用于改变作用范围的
>  他们的作用就和在bean标签中使用scope属性实现的功能是一样的
>      Scope
>          作用：用于指定bean的作用范围（单例|多例）
>          属性：
>              value：指定范围的取值。常用取值：singleton prototype
>     
> 和生命周期相关 了解
>  他们的作用就和在bean标签中使用init-method和destroy-methode的作用是一样的
>      PreDestroy
>          作用：用于指定销毁方法
>      PostConstruct
>          作用：用于指定初始化方法

## 注释创建

### 引入Context注释扫包

```xml
    <!--  告知spring在创建容器时要扫描的包-->
    <context:component-scan base-package="com.wcy"></context:component-scan>
```



```java
@Configuration
@ComponentScan(basePackages = "com.wcy")
public class SptingConfiguration {

    @Bean(name = "runner")
    @Scope("prototype")
    public QueryRunner createQueryRunner(DataSource dataSource){
        return  new QueryRunner(dataSource);
    }

    @Bean(name = "dataSource")
    public DataSource createdataSource(){
        ComboPooledDataSource ds = new ComboPooledDataSource();
        try {
            ds.setDriverClass("com.mysql.jdbc.Driver");
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/textjdbc");
            ds.setUser("root");
            ds.setPassword("654321");
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

### 导入多文件配置的方法

#### 	1.使用Configuration配置

在配置文件只有一个的情况下可以没有Configuration（表示扫描这个类）

```java
@Configuration
@ComponentScan({"com.wcy","config"})
public class SptingConfiguration {

}
```

​	

```java
@Configuration
public class JdbcConfig {

    @Bean(name = "runner")
    @Scope("prototype")
    public QueryRunner createQueryRunner(DataSource dataSource){
        return  new QueryRunner(dataSource);
    }

    @Bean(name = "dataSource")
    public DataSource createdataSource(){
        ComboPooledDataSource ds = new ComboPooledDataSource();
        try {
            ds.setDriverClass("com.mysql.jdbc.Driver");
            ds.setJdbcUrl("jdbc:mysql://localhost:3306/textjdbc");
            ds.setUser("root");
            ds.setPassword("654321");
            return ds;
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

#### 2.在引入文件时引入多个文件

```
new AnnotationConfigApplicationContext(new Class[]{SptingConfiguration.class, JdbcConfig.class});
```

#### 3.使用import

```java
@ComponentScan({"com.wcy","config"})
@Import(JdbcConfig.class)
@PropertySource("jdbcConfig.properties")
public class SptingConfiguration {
}
```



## 生命周期

```xml
<!-- bean的作用范围
        bean标签的scope属性：
            作用：用于指定bean的作用范围
            取值：
            singleton：单例模式
            prototype：多例
            request：作用于web应用的请求范围
            session：作用于web应用的会化范围
            global-session：作用于集群环境的绘画范围（全局绘画范围），当不是集群环境时
        bean标签的生命周期
            初始化
            销毁
     -->
    <bean id="accountService" class="com.wcy.service.impl.AccountServiceImpl" scope="singleton" init-method="init" destroy-method="destroy" ></bean>

```

## 注解

#### 在配置类中：

```
该类是一个配置类，他的作用和bean.xml是一样的
@Configuration
     作用：指定当前类为配置类
@ComponentScan(basePackages = "com.wcy")
     作用：用于通过注解指定spring在创建容器时要扫描的包，方便自动配入
     属性：
         value：他和basePackages的作用是一样的
@Bean
     作用：用于把当前方法的返回值作为bean对象存入spring对象存入spring的
     属性：
         name:用于指定Bean的id
     如果方法有参数spring会自动查找有么有可用的对象
     查找方式和 @Autowired一样
@Import
     作用：用于导入其他配置文件
     属性：
         value：用于指定其他配置类的字节码
             当我们使用Impory时，有import的类就是父配置类
@PropertySource
     作用用于指定properties文件的位置
     属性：
         value：指定文件的名称和路径
             关键字：classpath
```

#### 使用junit

<font color="red">在spring5.x中，要使用junit4.12以上版本</font>

```
使用Junit测试
要加入spring-test类
spring整合junit
     1.
     2.使用junit提供一个注解把原有的main方法替换了
         @RunWith(SpringJUnit4ClassRunner.class)
     3.告知spring的运行器。spring和ioc运行时基于xml还是注解的，并且说明位置
         @ContextConfiguration(classes = SptingConfiguration.class)
             location：指定xml文件的位置
             classes：指定注解类所在的位置
当我们是哦也能够spring 5.x版本时，junit应使用4.12及以上版本
```

## 使用数据库

1.配置tx

jar：

> commons-dbcp.jar
>
> ojdbc.jar	
>
> commons-pool.jar
>
> mybatis-spring.jar	spring-tx.jar	spring-jdbc.jar		spring-expression.jar
> spring-context-support.jar	spring-core.jar		spring-context.jar
> spring-beans.jar	spring-aop.jar	spring-web.jar	commons-logging.jar
> commons-dbcp.jar	ojdbc.jar	mybatis.jar	log4j.jar	commons-pool.jar

```xml
	<!-- 配置数据库相关-事务 -->
	
	<!-- 配置数据库相关 -->
	<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
		<property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
		<property name="url" value="127.0.0.1:3306/test"></property>
		<property name="username" value="root"></property>
		<property name="password" value="654321"></property>
	</bean>
	<!-- 配置事务管理器txManager -->	
	<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"></property>
	</bean>
	<!-- 增加对事物的支持 -->
	<tx:annotation-driven transaction-manager="txManager"/>
```

### 数据的制作（完整版）

### dao

```java
public class ConnectionUtils {
    private ThreadLocal<Connection> tl = new ThreadLocal<Connection>();

    private DataSource dataSource;

    public void setDataSource(DataSource dataSource) {
        this.dataSource = dataSource;
    }
    
    public Connection getThreadConnection() {
        //1.先从ThreadLocal获取
        Connection conn = tl.get();
        try {
            //判断当前线程上是否有链接
            if (conn == null) {
                //从数据源中获取一个链接
                conn = dataSource.getConnection();
                tl.set(conn);
            }
            //返回当前线程上的链接
            return conn;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
    
    /**
     * 把链接和线程解绑
     */
    public void removeConnection(){
        tl.remove();
    }

}
```



```java
public class AccountDaoImpl implements IAccountDao {
	private QueryRunner runner;
    private ConnectionUtils connectionUtils;

    public void setConnectionUtils(ConnectionUtils connectionUtils) {
        this.connectionUtils = connectionUtils;
    }
  public void setRunner(QueryRunner runner) {
        this.runner = runner;
    }

    public List<Account> findAllAccount() {
        try {
      	return runner.query(connectionUtils.getThreadConnection(),"SELECT * FROM `account`", new BeanListHandler<Account>(Account.class));
        } catch (SQLException e) {
            e.printStackTrace();
        }
        return null;
    }
}
```

xml

```xml
    <!-- 配置QueryRunner对象 -->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
<!--        <constructor-arg name="ds" ref="dataSource"></constructor-arg>-->
    </bean>

    <!-- 配置数据源 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--配置数据源的基本信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/textjdbc"></property>
        <property name="user" value="root"></property>
        <property name="password" value="654321"></property>
    </bean>

    <!--配置connection的工具类-->
    <bean id="connectionUtils" class="com.wcy.utils.ConnectionUtils">
        <!-- 注入数据源 -->
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <!-- 配置事务管理器 -->
    <bean id="txManage" class="com.wcy.utils.TranactionManager">
        <!--注入connectionUtil-->
        <property name="connectionUtils" ref="connectionUtils"></property>
    </bean>
```



### service

```java
public class TranactionManager {
    private ConnectionUtils connectionUtils;

    public void setConnectionUtils(ConnectionUtils connectionUtils) {
        this.connectionUtils = connectionUtils;
    }

    public void beginTransacttion(){
        try {
            connectionUtils.getThreadConnection().setAutoCommit(false);
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    public void commit(){
        try {
            connectionUtils.getThreadConnection().commit();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    public void rollback(){
        try {
            connectionUtils.getThreadConnection().rollback();
        }catch (Exception e){
            e.printStackTrace();
        }
    }

    public void release(){
        try {
            connectionUtils.getThreadConnection().close();
            connectionUtils.removeConnection();
        }catch (Exception e){
            e.printStackTrace();
        }
    }
    
}
```



```java
public class AccountService implements IAccountService {
    private IAccountDao accountDao;
    private TranactionManager txManager;

    public void setTxManager(TranactionManager txManager) {
        this.txManager = txManager;
    }

    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }
    public List<Account> findAllAccount() {
        try{
            //开启事务
            txManager.beginTransacttion();
            //实行操作
            List<Account> accounts = accountDao.findAllAccount();
            //提交事务
            txManager.commit();
            //返回结果
            return accounts;
        }catch (Exception e){
            //回滚操作
            txManager.rollback();
        }finally {
            //释放连接
            txManager.release();
        }
        return null;
    }
    public List<Account> findAllAccount() {
        try{
            //开启事务
            txManager.beginTransacttion();
            //实行操作
            List<Account> accounts = accountDao.findAllAccount();
            //提交事务
            txManager.commit();
            //返回结果
            return accounts;
        }catch (Exception e){
            //回滚操作
            txManager.rollback();
        }finally {
            //释放连接
            txManager.release();
        }
        return null;
    }
    }
```



### controller

```java
 @Autowired
    private IAccountService as;

    @Test
    public void testTransfer(){
        as.transfer("bbb","aaa",200f);
    }

```



# AOP

通过[预编译](https://baike.baidu.com/item/预编译/3191547)方式和运行期动态代理实现程序功能的统一维护的一种技术

## 动态代理

### 面向接口

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



### 面向子类

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

## 使用Aop

### 名词

> **1.通知（Advice）**  
>    就是你想要的功能，也就是上面说的 安全，事物，日志等。你给先定义好把，然后在想用的地方用一下。
>  **2.连接点（JoinPoint）**
>   这个更好解释了，就是spring允许你使用通知的地方，那可真就多了，基本每个方法的前，后（两者都有也行），或抛出异常时都可以是连接点，spring只支持方法连接点.其他如aspectJ还可以让你在构造器或属性注入时都行，不过那不是咱关注的，只要记住，和方法有关的前前后后（抛出异常），都是连接点。
>  **3.切入点（Pointcut）**
>   上面说的连接点的基础上，来定义切入点，你的一个类里，有15个方法，那就有几十个连接点了对把，但是你并不想在所有方法附近都使用通知（使用叫织入，以后再说），你只想让其中的几个，在调用这几个方法之前，之后或者抛出异常时干点什么，那么就用切点来定义这几个方法，让切点来筛选连接点，选中那几个你想要的方法。
>  **4.切面（Aspect）**
>   切面是通知和切入点的结合。现在发现了吧，没连接点什么事情，连接点就是为了让你好理解切点，搞出来的，明白这个概念就行了。通知说明了干什么和什么时候干（什么时候通过方法名中的before,after，around等就能知道），而切入点说明了在哪干（指定到底是哪个方法），这就是一个完整的切面定义。
>  **5.引入（introduction）**
>   允许我们向现有的类添加新方法属性。这不就是把切面（也就是新方法属性：通知定义的）用到目标类中吗
>  **6.目标（target）**
>   引入中所提到的目标类，也就是要被通知的对象，也就是真正的业务逻辑，他可以在毫不知情的情况下，被咱们织入切面。而自己专注于业务本身的逻辑。
>  **7.代理(proxy)**
>   怎么实现整套aop机制的，都是通过代理，这个一会给细说。
>   **8.织入(weaving)**
>   把切面应用到目标对象来创建新的代理对象的过程。有3种方式，spring采用的是运行时，为什么是运行时，后面解释。
> 关键就是：切点定义了哪些连接点会得到通知


>  spring中基于Xml的Aop配置步骤 -
> 1.把通知Bean也交给spring来管理
> 2.使用aop:config标签来表明开始AOp配置
> 3.使用aop:aspect标签表明配置切面
>     id属性：是给切面提供一个唯一标识
>     ref属性：是指通知类bean的id
> 4.在aop:aspect标签的内部使用对应标签来配置通知的类型
>     我们现在实例是让printLog方法在切入点方法执行之前
>     aop:before配置前置通知
>         menthod:指定Logger类的方法通知
>         ppointcut:用于指定切入点表达式，该表达式的含义指的是对业务层中的哪些方法
>   切入点表达式的写法：
>     关键字：execution
>     表达式：
>         访问修饰符 返回值 包名.类名.方法名
>     标准的表达式写法：
>         public void com.wcy.service.impl.AccountS
>     访问修饰符可以省略
>         void com.wcy.service.impl.AccountServiceI
>     返回值可以用*表示
>         * com.wcy.service.impl.AccountServiceImpl
>        包名可以使用*表示，有几个包就写几个*.
>                * *.*.*.*.AccountServiceImpl.saveAccount(
>           报名可以使用..表示当前包和子包
>                       * *..AccountServiceImpl.saveAccount()
>              使用*来表示实现统配
>                              * *..*.saveAccount()
>                 使用*实现方法统配
>                                     参数列表
>             空    无参数
>             直接写类型 int
>             有参数 *
>             有无均可 ..
>                 全通配写法：
>                                     * *..*.*(..)
>                    一般写法
>                                            包名
>                                            * 包名.*.*(..)


```xml
    <!-- 配置Logger类 -->
    <bean id="logger" class="com.wcy.utils.Logger"></bean>
 <!-- 配置AOp -->
    <aop:config>
        <!-- 配置切入点表达式 id用于指定表达式唯一标识 expression属性用于指定表达式的内容
              此标签写在aop:aspect内部只能当前页面使用
              它还可以卸载aop:aspect外面-->
        <aop:pointcut id="ptl" expression="execution(* com.wcy.service.impl.*.*(..))"/>
        <!-- 配置切面-->
        <aop:aspect id="logAdvice" ref="logger">
            <!--配置通知类型
            <aop:before method="brforeprintLog" pointcut-ref="ptl"></aop:before>-->
            <!--配置后置通知类型
            <aop:after-returning method="afterReturnprintLog" pointcut-ref="ptl"></aop:after-returning>-->
            <!--配置异常通知类型
            <aop:after-throwing method="afterthrowingprintLog" pointcut-ref="ptl"></aop:after-throwing>-->
            <!--配置最终通知类型
            <aop:after method="afterprintLog" pointcut-ref="ptl"></aop:after>-->
            <aop:around method="aroundPringLog" pointcut-ref="ptl"></aop:around>
<!--这边也可以写
<aop:pointcut id="ptl" expression="execution(* com.wcy.service.impl.*.*(..))"/>-->
        </aop:aspect>
    </aop:config>
```



环绕通知

```java
    /**
     * 环绕通知
     * 配置之后切入点方法没有执行
     * 分析：
     *      通过对比动态代理中的环绕通知代码：发现我们没有明确的切入点调用
     * 解决：
     *      提供ProceedingJoinPoint
     *
     */
    public Object aroundPringLog(ProceedingJoinPoint pjp){
        Object rtValue =null;
        try {
            Object[] args = pjp.getArgs();
            System.out.println(("环绕通知Logger类中的aroundPringLog方法开始记录日志 了。。前置"));
            rtValue = pjp.proceed(args);
            System.out.println(("环绕通知Logger类中的aroundPringLog方法开始记录日志 了。。后置"));
            return rtValue;
        } catch (Throwable throwable) {
            throwable.printStackTrace();
            System.out.println(("环绕通知Logger类中的aroundPringLog方法开始记录日志 了。。"));
            throw new RuntimeException(throwable);
        }finally {
            System.out.println(("环绕通知Logger类中的aroundPringLog方法开始记录日志 了。。"));
        }

    }
```









<font color = "red" size ="19">6-10集重看</font>

# WEB合体

<font color="red">配置文件jar和capplicationContext.xml要放在***/WebContent/WEB-INF/lib***中</font>



web项目启动时 ，会自动加载web.xml，因此需要在web.xml中加载 监听器（ioc容器初始化）。



Web项目启动时，启动实例化Ioc容器：

```xml


<!-- 指定 Ioc容器（applicationContext.xml）的位置-->
  <context-param>
  		<!--  监听器的父类ContextLoader中有一个属性contextConfigLocation，该属性值 保存着 容器配置文件applicationContext.xml的位置 -->
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:applicationContext.xml</param-value>
  </context-param>  
  <listener>
  	<!-- 配置spring-web.jar提供的监听器，此监听器 可以在服务器启动时 初始化Ioc容器。
  		初始化Ioc容器（applicationContext.xml） ，
  			1.告诉监听器 此容器的位置：context-param
  			2.默认约定的位置	:WEB-INF/applicationContext.xml
  	 -->
  	<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  </listener>

```



### 在web中的对象与SpringIoc不相同

<img src="..\image\001.png"  />

#### 解决方法：

#### 在"init()"中给Servlet的值赋值

