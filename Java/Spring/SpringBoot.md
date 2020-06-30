<!--2020.01.03-->

<font color = "red">看日志管理 </font>

<font color = "red">看web </font>





usingthyeleaf

>server
>
>controller
>
>dao

## 各类注释

- `@Mapper`注解，表明该接口是一个Mapper：表明该接口是一个mybatis中的mapper，这种方法需要给每个MApper都加上注解，也可以在配置类上添加@MapperScan("org.sang.mapper")表示扫描org.sang.mapper包下的所有接口作为Mapper

- `@Entity`注解表示该类是一个实体类，在启动项目时会根据该类自动生成一张表，表的名称即`@Entity`注解中anme的值，如果不配置name，默认表名为类名
- `@Service` 表示为服务层,`@Controller`表示控制层

### 关于xml文件问题

在maven工程中，xml配置文件建议写到resources目录下，若写到其他包下，maven在运行时会忽略剥下的XML文件，因此需要在pom.xml文件中重新指明资源文件位置

pom.xml

```

```

<!-- 2020.06.27 -->

## 学习

### 第一个微服务程序

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
<dependency>
```

### 配置

#### yaml

```yaml
#对象
student:
  name:qinjiang
  age: 3
  
student:{name:qinjiang,age:3}

#数组
pets:
  - cat
  - dag
  - pig
    
pets: [cat,dog,pig]
```



yaml文件：

```yaml
person:
  name: qingjiang
  age: 3
  happy: false
  birth: 2019/11/02
  maps: {k1: v1,k2: v2}
  lists:
    - code
    - music
    - girl
  dog:
    name: 旺财
    age: 3

```

实体类：

```java
@Component
@ConfigurationProperties(prefix = "person")

public class Person {
    //SPEL表达式
    @Value("${name}")
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;a
    private Dog dog;
}
```

测试：

```java
@SpringBootTest
class Springboot02ConfigApplicationTests {

    @Autowired
    private Person person;

    @Autowired
    @Test
    void contextLoads() {
        System.out.println(person);
    }

}
```



peroperties:

```properties
name=狂神
```

```java
@Component
@PropertySource(value = "classpath:application.properties")
public class Person {
    //SPEL表达式
    @Value("${name}")
    private String name;
    private Integer age;
    private Boolean happy;
    private Date birth;
    private Map<String,Object> maps;
    private List<Object> lists;
    private Dog dog;
}
```



配置级别

1. ./config/
2. ./
3. /config/
4. /

### 多环境配置

propesties方法

application-dev.properties

application-test.properties

```properties
#多环境配置
spring.profiles.active=dev
```

yaml方法

```yaml
server:
  port: 8081
spring:
  profiles:
    active: dev
---
server:
  port: 8082
spring:
  profiles: dev
  
---
server:
  port: 8083
spring:
  profiles: test
```



## springboot web开发

[webjars](https://www.webjars.org/)

1. 存放静态资源的地方
   - [webjars](https://www.webjars.org/)
   - public,static,/**,resource
   - 优先级 resource>static>public

模板引擎

[thymeleaf](https://github.com/thymeleaf/thymeleaf)

```xml
 <dependency>
     <groupId>org.thymeleaf</groupId>
     <artifactId>thymeleaf-spring5</artifactId>
 </dependency>
 <dependency>
     <groupId>org.thymeleaf.extras</groupId>
     <artifactId>thymeleaf-extras-java8time</artifactId>
 </dependency>
```

只需要使用

```java
//在templates目录下的所有页面，只能通过controller来跳转
//需要模板引擎的支持
@Controller
public class IndexController {

    @RequestMapping("/test")
    public String index(){
        return "text";
    }
}
```

