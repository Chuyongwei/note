## 1.配置

1. ### maven

   ```xml
       <dependencies>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
           </dependency>
           <dependency>
               <groupId>org.mybatis.spring.boot</groupId>
               <artifactId>mybatis-spring-boot-starter</artifactId>
               <version>2.1.4</version>
           </dependency>
       </dependencies>
   ```

2. 设置mysql语句

   ```sql
   DROP TABLE IF EXISTS user;
   
   CREATE TABLE user
   (
   	id BIGINT(20) NOT NULL COMMENT '主键ID',
   	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
   	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
   	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
   	PRIMARY KEY (id)
   );
   DELETE FROM user;
   
   INSERT INTO user (id, name, age, email) VALUES
   (1, 'Jone', 18, 'test1@baomidou.com'),
   (2, 'Jack', 20, 'test2@baomidou.com'),
   (3, 'Tom', 28, 'test3@baomidou.com'),
   (4, 'Sandy', 21, 'test4@baomidou.com'),
   (5, 'Billie', 24, 'test5@baomidou.com');
   ```

3. 配置仓库

   ```yml
   spring:
     datasource:
       driver-class-name: com.mysql.cj.jdbc.Driver
       url: jdbc:mysql://localhost:3306/nucpermission?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8
       username: root
       password: 654321
   ```

4. 设置mapper

   ```java
   @Repository
   public interface UserMapper extends BaseMapper<User> {
       // 所有的crud操作都完成了
   }
   ```

5. 设置实体

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
   
       private Long id;
       private String name;
       private Integer age;
       private String email;
   }
   ```

6. 运行mybatis

   ```java
   @RunWith(SpringRunner.class)
   @SpringBootTest
   public class mybatisTest {
   
       @Autowired
       private UserMapper userMapper;
   
       @Test
       public void demo(){
           //阐述是一个Wrapper。条件构造器，我们用null
           List<User> users = userMapper.selectList(null);
   //        System.out.println(users);
           users.forEach(System.out::println);
       }
   }
   ```

## 2.CRUD操作

### 增加

```java
    @Test
    public void testInsert(){
        User user =new User();
        user.setName("狂神说");
        user.setAge(3);
        user.setEmail("213424@qq.com");

        final int result = userMapper.insert(user); // 添加
        System.out.println(result); // 搜影响的函数
        System.out.println(user); // 插入内容
    }
```

### 自动填充

#### 主键

- [分布式系统唯一ID生成方案汇总](https://www.cnblogs.com/haoxinyue/p/5208136.html)

雪花算法：

snowflake是Twitter开源的分布式ID生成算法，结果是一个long型的ID。其核心思想是：使用41bit作为毫秒数，10bit作为机器的ID（5个bit是数据中心，5个bit的机器ID），12bit作为毫秒内的流水号（意味着每个节点在每毫秒可以产生 4096 个 ID），最后还有一个符号位，永远是0。

```java
public enum IdType {
    AUTO(0), // 自增
    NONE(1), // 不做任何操作
    INPUT(2), // 自行输入
    ASSIGN_ID(3), // 默认全据唯一id
    ASSIGN_UUID(4), // uuid
    /** @deprecated */
    @Deprecated
    ID_WORKER(3),
    /** @deprecated */
    @Deprecated
    ID_WORKER_STR(3), // 字符串表示算法
    /** @deprecated */
    @Deprecated
    UUID(4);
}
```

#### 时间

1. 通过使用sql语句

   ```
   
   ```

   

2. 通过代码填充

   实体

   ```java
   @Data
   @AllArgsConstructor
   @NoArgsConstructor
   public class User {
   
       @TableId(type = IdType.AUTO)
       private Long id;
       private String name;
       private Integer age;
       private String email;
   
       @TableField(fill = FieldFill.INSERT)
       private Date createTime;
   
       @TableField(fill = FieldFill.INSERT_UPDATE)
       private Date updateTime;
   }
   ```

   添加配置

   ```java
   @Slf4j
   @Component // 将该类放进ioc容器中
   public class MyMetaObjectHandler implements MetaObjectHandler {
       // 插入时的填充策略
       @Override
       public void insertFill(MetaObject metaObject) {
           log.info("start insert fill....");
           // setFieldValByName(String fieldName, Object fieldVal, MetaObject metaObject)
           this.setFieldValByName("createTime",new Date(),metaObject);
           this.setFieldValByName("updateTime",new Date(),metaObject);
       }
   
       @Override
       public void updateFill(MetaObject metaObject) {
           log.info("start update fill....");
           this.setFieldValByName("updateTime",new Date(),metaObject);
       }
   }
   
   ```

   

### 日志



```yaml
#配置日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```



### 乐观锁

> 乐观锁：顾名思义十分乐观，他总是认为不会出现问题，无论干什么不去上锁！如果出现问题，再次更新值测试
>
> 悲观锁：顾名思义十分悲观，他总是认为总是会出现问题，无论干什么都会上锁！再操作

乐观锁机制

乐观锁实现方式：

> - 取出记录时，获取当前version
> - 更新时，带上这个version
> - 执行更新时， set version = newVersion where version = oldVersion
> - 如果version不对，就更新失败

```sql
乐观锁：1、先查询，获得版本号 version = 1
-- A
update user set name = "kuangshen", version = version + 1
where id = 2 and version = 1
-- B 线程抢先完成，这个时候 version = 2，会导致 A 修改失败！
update user set name = "kuangshen", version = version + 1
where id = 2 and version = 1
```

1. 数据库添加`version`字段

2. 实体类添加version字段

   ```java
       @Version
       private Integer version;
   ```

   

3. 配置

   ```java
   @MapperScan("nuc.edu.wcy.mapper")
   @EnableTransactionManagement
   @Configuration
   public class MybatisPlusConfig {
   
       @Bean
       public OptimisticLockerInterceptor optimisticLockerInnerInterceptor(){
           return new OptimisticLockerInterceptor();
       }
   
   }
   ```

   

### 分页

- 先在condig中注册

  ```java
  @MapperScan("nuc.edu.wcy.mapper")
  @EnableTransactionManagement
  @Configuration
  public class MybatisPlusConfig {
  
      @Bean
      public PaginationInterceptor paginationInterceptor() {
          PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
          // 设置请求的页面大于最大页后操作， true调回到首页，false 继续请求  默认false
          // paginationInterceptor.setOverflow(false);
          // 设置最大单页限制数量，默认 500 条，-1 不受限制
          // paginationInterceptor.setLimit(500);
          // 开启 count 的 join 优化,只针对部分 left join
          paginationInterceptor.setCountSqlParser(new JsqlParserCountOptimize(true));
          return paginationInterceptor;
      }
  }
  ```

- 查询

  ```java
      @Test
      public void testPage(){
  
          // 当前页，页面大小
          Page<User> page = new Page<>(1,5);
          userMapper.selectPage(page,null);
          page.getRecords().forEach(System.out::println);
      }
  ```

### 删除

#### 方法

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class mybatisTest {

    @Autowired
    private UserMapper userMapper;

    // 按id删除
    @Test
    public void testDeleteById(){
        userMapper.deleteById(1355385908456628233L);
    }


    // 多个id删除
    @Test
    public void testDeleteBatchId(){
        userMapper.deleteBatchIds(Arrays.asList(1355385908456628231L,1355385908456628232L));
    }

    // 条件删除
    @Test
    public void testDeleteMap(){
        HashMap<String,Object> map = new HashMap<>();
        map.put("name","关注公众号狂神1说");
        map.put("age",4);
        userMapper.deleteByMap(map);
    }
}

```

#### 逻辑删除

1. 数据库添加deleted字段

2. 实体类添加

   ```java
       @TableLogic
       private Integer deleted;
   ```

3. properties配置

   ```properties
   mybatis-plus.global-config.db-config.logic-delete-value=1
   mybatis-plus.global-config.db-config.logic-not-delete-value=0
   ```

   



### 性能分析插件与性能分析打印

我们在平时的开发中，会遇到一些慢sql。测试！ druid,,,,, 

作用：性能分析拦截器，用于输出每条 SQL 语句及其执行时间 

MP提供了两种方式，用于输出每条SQL语句及其执行时间，针对执行较长时间的SQL可以停止运行，有助于发现问题。

特别是在sql优化方面，可以把执行时间较长的sql“揪出来”方便进行sql优化

#### 性能分析插件

该功能在3.2.0版本被废除，3.2.0以上版本可以使用性能分析打印插件，3.2.0以下版本可以接着使用

在配置类中配置插件

```javascript
 @Bean
    @Profile({"dev","test"}) // 指定环境
    public PerformanceInterceptor performanceInterceptor() {
        PerformanceInterceptor interceptor = new PerformanceInterceptor();
        // sql美化打印
        interceptor.setFormat(true);
        // 设置SQL超时时间
        interceptor.setMaxTime(500);
        //格式化语句
        performanceInterceptor.setFormat(true);
        return interceptor;
    }
```

从以上代码中可以看出需要设置执行的环境 在application.yml中配置

```javascript
profiles:
    active: dev #设置当前环境为dev
```

#### 性能分析打印

引入依赖

```xml
<dependency>
    <groupId>p6spy</groupId>
    <artifactId>p6spy</artifactId>
    <version>3.8.7</version>
</dependency>
```

增加spy.properties配置文件

```properties
#3.2.1以上使用
modulelist=com.baomidou.mybatisplus.extension.p6spy.MybatisPlusLogFactory,com.p6spy.engine.outage.P6OutageFactory
#3.2.1以下使用或者不配置
#modulelist=com.p6spy.engine.logging.P6LogFactory,com.p6spy.engine.outage.P6OutageFactory
# 自定义日志打印
logMessageFormat=com.baomidou.mybatisplus.extension.p6spy.P6SpyLogger
#日志输出到控制台
appender=com.baomidou.mybatisplus.extension.p6spy.StdoutLogger
# 使用日志系统记录 sql
#appender=com.p6spy.engine.spy.appender.Slf4JLogger
# 设置 p6spy driver 代理
deregisterdrivers=true
# 取消JDBC URL前缀
useprefix=true
# 配置记录 Log 例外,可去掉的结果集有error,info,batch,debug,statement,commit,rollback,result,resultset.
excludecategories=info,debug,result,commit,resultset
# 日期格式
dateformat=yyyy-MM-dd HH:mm:ss
# 实际驱动可多个
#driverlist=org.h2.Driver
# 是否开启慢SQL记录
outagedetection=true
# 慢SQL记录标准 2 秒
outagedetectioninterval=2
```

application.yml 配置：

```yaml
spring:
  datasource:
    driver-class-name: com.p6spy.engine.spy.P6SpyDriver
    type: com.alibaba.druid.pool.DruidDataSource
    url: jdbc:p6spy:mysql://localhost:3306/ssm?
    username: username
    password: password
```

>  注意！ 

- driver-class-name 为 p6spy 提供的驱动类
- url 前缀为 jdbc:p6spy 跟着冒号为对应数据库连接地址
- 打印出sql为null,在excludecategories增加commit
- 批量操作不打印sql,去除excludecategories中的batch
- 批量操作打印重复的问题请使用MybatisPlusLogFactory (3.2.1新增）
- 该插件有性能损耗，不建议生产环境使用

### Wrapper



> eq,ne,gt,ge,lt,le:=,<>,>,>=,<,<=
> between:区间
> notBetween:非区间
> in: in("age",1,2,3,4) in("age",{1,2,3,4})-->age in(1,2,3,4)
> insql: `in("age","1,2,3,4,5")` ` in("age","select age from user23")`
> OrderBy:排序 `orderBy(true, true, "id", "name")`--->`order by id ASC,name ASC`
> having: `having("sum(age) > 10")`--->`having sum(age) > 10`
>
> `having("sum(age) > {0}", 11)`--->`having sum(age) > 11`

```java
    @Test
    public void test(){
        //查询name不为空，邮箱不为空，年龄大于12
        QueryWrapper<User> wrapper = new QueryWrapper<>();
        // eq,ne,gt,ge,lt,le
        wrapper.isNotNull("name")
                .isNotNull("email")
                .ge("age",12);
        userMapper.selectList(wrapper).forEach(System.out::println);

    }
```

### 代码生成器！！！

maven

```xml
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-generator</artifactId>
            <version>3.4.1</version>
        </dependency>
        <!-- 默认模板引擎 -->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.2</version>
        </dependency>
```

设置代码生成器

```java
public class GentorCode {

    public static void main(String[] args) {
        AutoGenerator mpg = new AutoGenerator();

        GlobalConfig gc = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        // TODO 修改路径和作者
        gc.setOutputDir(projectPath + "\\shiyan\\Mybatisplus-AutoGenerator\\src\\main\\java");
        gc.setAuthor("ChuyongWei");
        //是否打开资源管理器
        gc.setOpen(true);
        gc.setFileOverride(true); // 是否覆盖
//        gc.setServiceImplName("%sService");

        gc.setIdType(IdType.AUTO);
        gc.setDateType(DateType.ONLY_DATE);
        gc.setSwagger2(true);

        // 全据配置装入代码生成器
        mpg.setGlobalConfig(gc);

        // TODO 配置数据源
        DataSourceConfig dsc = new DataSourceConfig();
        dsc.setDriverName("com.mysql.cj.jdbc.Driver");
        dsc.setUrl("jdbc:mysql://localhost:3306/nucpermission?serverTimezone=UTC&useUnicode=true&characterEncoding=utf-8");
        dsc.setUsername("root");
        dsc.setPassword("654321");
        dsc.setDbType(DbType.MYSQL);
        mpg.setDataSource(dsc);

        //TODO 包的配置
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("mybatisplusautoGenerator");
        pc.setParent("nuc.edu.wcy");
        pc.setEntity("entity");
        pc.setMapper("mapper");
        pc.setService("service");
        pc.setServiceImpl("service.impl");
        pc.setController("controller");
        mpg.setPackageInfo(pc);

        //策略配置
        StrategyConfig strategy = new StrategyConfig();
//        strategy.setInclude("user");//设置要映射的表名
        strategy.setNaming(NamingStrategy.underline_to_camel);
        strategy.setColumnNaming(NamingStrategy.underline_to_camel);
//        strategy.setSuperEntityClass("你自己的父类实体,没有就不用设置!");
        strategy.setEntityLombokModel(true);
        //设置逻辑删除
        strategy.setLogicDeleteFieldName("deleted");

        // 填充策略
        TableFill gmtCreate = new TableFill("create_time", FieldFill.INSERT);
        TableFill gmtUpate = new TableFill("update_time", FieldFill.INSERT_UPDATE);
        ArrayList<TableFill> tableFills = new ArrayList<>();
        tableFills.add(gmtCreate);
        tableFills.add(gmtUpate);
        strategy.setTableFillList(tableFills);


        //乐观锁
        strategy.setVersionFieldName("version");
        strategy.setRestControllerStyle(true);
        strategy.setControllerMappingHyphenStyle(true);
        mpg.setStrategy(strategy);
        mpg.execute();
    }
}
```

