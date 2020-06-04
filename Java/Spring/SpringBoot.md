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

