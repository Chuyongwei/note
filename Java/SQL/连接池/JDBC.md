2、使用最新jdbc8.0的连接驱动问题：
2.1、首先是在连接池信息配置过程中的数据库驱动类名：
以往的jdbc驱动类名为“com.mysql.jdbc.Driver”或“org.git.mm.mysql.Driver”但是现在来看这样都是废弃的，而是为：“com.mysql.cj.jdbc.Driver”



```java
<property name="url" value="jdbc:mysql://localhost:3306/mybatis_test?serverTimezone=UTC&***amp;***characterEncoding=utf-8" /> 
```

> url = "jdbc:mysql://localhost:3306/test_demo?useSSL=false&serverTimezone=UTC"

```xml
<property name="url" value="jdbc:mysql://localhost:3306/db_mybatis?useSSL=false&amp;serverTimezone=UTC"/>
```

