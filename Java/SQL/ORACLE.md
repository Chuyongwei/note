

[TOC]

# 开机

1. `sqlplus 用户名/密码`

2. `sqlplus`  ->用户名->密码

3. 修改密码

   ```sql
   alter user 用户名 identifiyed by 密码
   ```

4. 维护

   ```powershell
   第 1 行出现错误:
   ORA-01109: 数据库未打开
   SQL>shutdown immediate;
   
   已经卸载数据库。
   ORACLE 例程已经关闭。
   SQL> startup;
   ORACLE 例程已经启动。
   
   Total System Global Area 3674501120 bytes
   Fixed Size                  2181144 bytes
   Variable Size            1979713512 bytes
   Database Buffers         1677721600 bytes
   Redo Buffers               14884864 bytes
   数据库装载完毕。
   ORA-01113: 文件 5 需要介质恢复
   ORA-01110: 数据文件 5: 'C:\APP\CHUYONG_WEI\ORADATA\ORCL\EXAMPLE01.DBF'
   
   SQL> recover datafile 'C:\APP\CHUYONG_WEI\ORADATA\ORCL\EXAMPLE01.DBF';
   完成介质恢复。
   SQL> alter database open ;
   ```

   

设置行和列

```sql
set linesize 150;
set pagesize 50;
```

### 结构

数据库（database）-> 表（table）->列()

### 类型

char 用于存储固定长度的字符串，范围是1-200字节

varchar2 存储变长而非固定长度

number(7[,2]) 长度为7小数占2位的整数或浮点数

date 日期时间 sysdate函数可查找当前时间 to_date函数将数值或字符串转换成date类型

rowid 伪列记录列数

<font color = "red">**注意:在操作表时一定要提交事务，否则原表是不会发生改变的**</font>

SQL语言的特点：集合性，统一性，可移植性；

# 用户模式

### 	用户与模式

- **用户（user）**:Oracle用户是用连接数据库和访问数据库对象的。（用户是用来连接数据库访问数据库）。

- **模式(schema)**：模式是数据库对象的集合。模式对象是数据库数据的逻辑结构。（把数据库对象用模式分开成不同的逻辑结构）。

- **用户（user）与模式(schema)的区别**：用户是用来连接数据库对象。而模式用是用创建管理对象的。(模式跟用户在oracle 是一对一的关系。)

### 实例模式SCOTT

  ​       为了给用户提供一些实例表和数据来展示Oracle数据库的一些特性，Oracle提供的典型的实例模式——SCOTT模式。SCOTT模式拥有的模式对象（都是数据表）如下：


![img](http://dl2.iteye.com/upload/attachment/0115/1806/cfc505ca-3ecc-31ee-b6a2-c06dd27b3a18.png)

### 		检索数据

```sql
select {[distinct] 列名..}
[into 表名]
from 表名
[where 选择条件]
[group by 列名]
[having 选择条件]
[order by 列名]
```



# DDL操作

>    (Data Definition Language 数据定义语言）用于操作对象及对象本身，这种对象包括数据库,表对象，及视图对象

## 方法

### 创建create

```sql
create  table 表名 (列名 类型[约束] ...  constraint 约束名 约束类型); 
```

复制

```sql
create table 表名 as select...
```

将select的内容粘到表中

### 修改alter

​	`drop` 和`add`

- 修改(添加)约束

```sql
alter table 表名 add constraint 约束名 约束类型(列名)
alter table 表名 drop constraint 约束名
```

- 添加字段

  ```sql
  alter table 表名 add (字段名 类型)
  ```

- 删除字段

  ```sql
  alter table 表名 drop (字段名,..)
  ```

- 修改字段

  ```sql
  alter table 表名 modify 列名 修改的属性
  ```

- 重命名

  表名

  ```sql
  alter table 表名 remane to 新表名
  ```

  列名

  ```sql
  alter table 表名 rename 列名 name to 新列名
  ```

### 删除drop

- 删除表

  ```sql
drop table 表名[cascade constraints];
  ```
  
  如果该表存在约束，关联的视图等，则必须使用`cascade constraints`才能删除
  
  当用户需要的时候可以使用flashback table来还原
  
  ``` sql
  flashback table 表名 to befre drop
  ```
  
  

### 查看约束

按表查

```sql
select *  from user_constraints where table_name = '表名';
```

按列查

```sql
select constraint_name from user_cons_columns where table_name = '表名' and column_name='列名';
```

## 约束

在 SQL 中，我们有如下约束：

- **NOT NULL** - 指示某列不能存储 NULL 值。
- **UNIQUE** - 保证某列的每行必须有唯一的值。
- **PRIMARY KEY** - NOT NULL 和 UNIQUE 的结合。确保某列（或两个列多个列的结合）有唯一标识，有助于更容易更快速地找到表中的一个特定的记录。
- **FOREIGN KEY** - 保证一个表中的数据匹配另一个表中的值的参照完整性，被引用的表必须有唯一约束或者主键约束。
- **CHECK** - 保证列中的值符合指定的条件。
- **DEFAULT** - 规定没有给列赋值时的默认值。

1. ### 非空约束（not null）

   > 指示某列不能存储 NULL 值。

   ```sql
   表名 not null
   ```

   

2. ### 主键(primary key)

   ```sql
   alter table 表名 add constraint primary key(列名)
   ```

   

3. ### 唯一约束(UNQUIE)

   > 保证某列的每行必须有唯一的值,可以为空。

   创建

   ```sql
   create 表名 ... <列名> constraint 约束名 unquie
   ```

   添加

   ```sql
   alter table 表名 add constraint 约束名 unquie(列名)
   ```

   删除

   ```sql
   alter table 表名 drop constraint 约束名;
   ```

4. ### 外键约束(FOREFIN KEY)

   创建

   ```SQL
   create table 表名 ... <列名> constraint 约束名 foreign key(列名) references 被引用表(列名);
   ```

   添加

   ```sql
   alter table 表名 add constraint 约束名 foreign key(列名) references 被引用表(列名);
   ```

   

5. ### 检查约束(CHECK)

   ```sql
   create table ... constraint 约束名 check(检查条件 and 检查条件)
   ```

6. ### 默认约束(DEFAULT)

   ```sql
   列名 default 默认值
   ```



# DML操作

> （Data Manipulation Language 数据操控语言）： 用于操作数据库对象对象中包含的数据

<font color="red">**insert，update和select一样都可以使用函数处理**</font>

## insert 

建表

1. 有列名的

   ```sql
   insert into dept(DEPTNO, DNAME, LOC)values(); 
   ```

2. 没列名的

   ```sql
    insert into dept
    values(23,'qianfeng','beijing');
   ```

3. 多列

## update

更新数据

格式

```sql
update 表名 set 列 = 值 where 条件
```

```sql
update emp  set sal = (select avg(sal) from emp where JOB = 'MANAGER') where sal < 2000;
```

## delete 

```sql
delete from 表名 [where 条件]
```



## 常用的函数

dual是一个万能表，可以用来测试函数

### 字符类函数

1. #### ASCII(c) 函数和CHR(i)函数

   ASCII函数用于返回一个字符的ASCII字符码，其中c表示一个字符，CHR函数用于返回给出ASCCII码值所对应的字符，i表示一个ASCII码值.

   - 查询码值

   ```sql
   select ascii('Z') Z from dual;
   ```

2. #### CONCAT(s1,s2)函数

   将字符串s2连接到s1后面

   ```sql
   select concat('hello','world') from dual;
   ```

3. #### initcap()将单词第一个字母大写

   ```sql
    select initcap('hello') from dual;
    select concat(initcap(ename),job) from emp;
   ```

4. #### instr(s1,s2,i,j)

   该函数用于返回字符s2在字符串s1中第j次出现的位置，搜索从字符串s1的第i个字符开始找到的字符串的位置。

5. #### length(s)函数

   返回字符串长度

6. #### LOWER（s）和UPPER（s）

   小写转大写，大写转小写

7. #### replace(s1,s2,s3)函数

   替换，将s1 中的s2换成s3

8. #### substr(s,i,[j])

   从字符串s的第i个位置开始截取长度为j的子字符串，若没有第三个参数会截取剩下所有字符

### 数字类函数

1. #### ABS(n):返回绝对值

   

2. #### ROUND(n1,n2)函数

   返回四舍五入的小数点为n2位的你数

3. #### POWER(n1,n2)函数

   n1的n2次方

### 日期函数

1. #### sysdate()表示当前日期

   

2. #### add_months(d,i)

   在制定日期d上加上指定月数i,求出之后的日期

### 聚合类函数

1. avg平均数
2. sum求和
3. max最大
4. min最小
5. count查询结果集中的记录数



# DQL操作

> ​	(Data Query Language 数据查询语言 )用于查询数据

### 查询select

​	

```
select {[distinct] 列名..}
[into 表名]
from 表名
[where 选择条件]
[group by 列名]
[having 选择条件]
[order by 列名]
```

`{}`的秘密

- 内容是`*`或者是指定元素
- `||`*查询连接符*
- `rowid`（隐藏列）标记列数

## WHERE

1. ### 	不等于

   ```sql
   select * from emp where sal!= all(1500,3000,850);
   ```

   <font color="red">在进行比较筛选时字符串和日期值需要单引号`''`标记</font>

2. ### and和or

   and：同时满足 

   or：有一条件为真

3. ### not取反

   ```sql
   not(取反内容)
   ```

4. ### between...and...

   ```sql
   列名 between 最小值 and 最大值
   ```

5. ### in

   dasfasdfas

6. ### like 模糊查找

   >`%`匹配任意长度的内容
   >
   >`_`可以匹配一个长度的内容
   >
   >`[]`：表示括号内所列字符中的一个（类似正则表达式）**。指定一个字符、字符串或范围，要求所匹配对象为它们中的任一个。
   >
   >`[^]` ：表示不在括号所列之内的单个字符



## ORDER BY

`desc`降序

`asc`升序

```sql
oeder 列名 by asc|desc
```

## 分组

```sql
select 列名 from 表名 group by 列名
```

## 多表查询

- ### 别名

  ```sql
  列名 [as] 别名
  ```

  ```sql
   select e.empno,job,e.ename,d.dname,loc from emp e,dept d where e.deptno = d.deptno;
  ```

  

- ### 内连接

  <font color = "red">只有匹配到的信息才能查询出来</font>

  例：

  ```sql
  select e.empno,job,e.ename,d.dname,loc from emp e inner join dept d on e.deptno = d.deptno;
  ```

- ### 外连接

  #### <font color="purple">谁是主表谁的数据就要都显示</font>

  - #### 左连接 `left outer join`

    ```sql
    select e.empno,job,e.ename,d.dname,loc from emp e left outer join dept d on e.deptno = d.deptno;
    ```

    

  - #### 右连接  `left outer join`

    ```sql
    select e.empno,job,e.ename,d.dname,loc from dept d right outer join emp e on e.deptno = d.deptno;
    ```

    

  - #### 完全连接 ` full outer join`

    ```sql
    select e.empno,job,e.ename,d.dname,loc from emp e full outer join dept d on e.deptno = d.deptno;
    ```

    

- ### 自然连接

  `natural join`

  在检索多个表的时候，ORACLE会将第一个表中的列于第二个表中具有相同名称的列进行自动链接，子啊自然连接中，用户不需要明确指定进行连接的列

  ```sql
  select empno,ename,job,dname from emp natural join dept where sal>2000
  ```

- ### 交叉连接

  结果是笛卡尔积

  ```sql
   select * from emp cross join dept;
  ```




## 子查询的用法

子查询是在SQL语句中包含另外一个select语句中的结果

根据另一个表查找相关的信息

```sql
select * from emp where  DEPTNO = (select deptno from dept where DNAME ='RESEARCH');
```

在使用子查询时：

1. 子查询必须用()括起来
2. 子查询中不能包括order by子句
3. 子查询允许嵌套多个，不能超过255个

### 单行子查询

> 返回一行数据的子查询语句

```sql
select * from emp 
where 
sal<(select max(sal) from emp) 
and 
sal>(select min(sal) from emp);
```

### 多行子查询

> 返回多行数据的子查询语句，当在where子句中使用多行子查询，必须使用多行运算符（in,all,any）

- ####  in() 括号内的元素有的都可以

- #### all 

- #### any 必须和单行比较运算符结合使用，并返回行只要匹配子查询的任何一个结果即可

  查询工资大于部门编号为10的任意一个员工的工资的其他部门的员工信息

  ```sql
  select * from emp where sal>any(select sal from emp where deptno = 10) and deptno <>10;
  ```

## 关联子查询

内查询需要外查询，而外查询的执行又离不开内查询的执行，这时，内查询和外查询是相互关联的

检索工资大于同职位的平均工资的员工信息

```sql
select * from emp e where sal > (select avg(sal) from emp where job = e.job);
```




# DCL操作

> （Data Control Language 数据控制语句） 用于操作数据库对象的权限

# 事务

> ​		这种把多条语句作为一个整体进行操作的功能，被称为数据库*事务*。数据库事务可以确保该事务范围内的所有操作都可以全部成功或者全部失败。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

可见，数据库事务具有ACID这4个特性：

- A：Atomic，原子性，将所有SQL作为原子工作单元执行，要么全部执行，要么全部不执行；
- C：Consistent，一致性，事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
- I：Isolation，隔离性，如果有多个事务并发执行，每个事务作出的修改必须与其他事务隔离；
- D：Duration，持久性，即事务完成后，对数据库数据的修改被持久化存储。

判断事务结束的情况：

- commit提交
- 执行rollback
- 执行语句
- 正常断开数据库的连接

# 视图

> 一个视图实际上就是封装了一条复杂的查询语句



1. 授权视图：

   ```sql
   connect system/manager
   grant create any view tcott;
   conn cott/tiger
   ```

2. 创建视图

   ```sql
    create view VIEW_EMP
     as
     select empno,ename,job,deptno
     from emp
     where deptno = 20
   ```

3. 使用视图

   ```sql
   select * from VIEW_EMP
   ```

4. 添加视图信息

   ```sql
   insert into VIEW_EMP values(...)
   ```

5. 更新视图

   ```sql
   update VIEW_EMP set ename = '西方' where
   ```

6. 删除视图内容

   ```sql
   delete from emp where ...
   ```
   
7. 删除视图

   ```sql
   drop view VIEW_EMP;
   ```
   
8. 查看所有视图

   ```sql
   show table status where comment='view';
   ```

   

- 只读视图

  1. 创建

     ```sql
     create or replace view VIEW_EMP
       as
       select empno,ename,job,deptno
       from emp
       where deptno = 20 read only;
     ```

- 复杂视图

  > ​		复杂视图是指包含函数，表达式，分组数据的视图，主要目的是为了简化操作，需要注意的，当视图的查询包含函数或者表达式的

  创建
  
  ```sql
  select deptno 部门编号,
  ```
  
- 连接视图

  > 基于多个表所做的视图，目的是为了简化链接插叙

  <font color="red">注意：建立连接视图时，必须使用where子句中指定有效的连接条件</font>

  ```sql
  create or replace view VIEW_DE as
  select d.deptno,dname,e.ename,sal from dept d left join emp e where d.deptno = e.deptno and d.deptno=20
  ```

# 索引



1. 授权

   `create index`

   ```sql
   grant create any index to scott;
   ```

2. 创建索引

   ```sql
   create index 索引名 on emp(deptno[,job]);
   ```

3. 删除索引

   权限：drop index

   ```sql
   drop index 索引名字
   ```

4. 优点：

   查询快
   
5. 不足

   - 系统要用1.2倍的硬盘和内存
   - 更新慢

# 用户管理

## 密码

### 密码过期

#### 设置密码不过期

1.查看用户的profile设置：



```sql
SQL>  SELECT username,profile FROM dba_users;
```



一般用户的profile设置都为DEFAULT。

2.查看系统profiles中PASSWORD_LIFE_TIME设置。



```sql
SQL> SELECT * FROM dba_profiles s WHERE s.profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME';
```

```powershell
PROFILE            RESOURCE_NAME          RESOURCE           LIMIT

------------------------------ -------------------------------- ------------------------------------------------

DEFAULT            PASSWORD_LIFE_TIME        PASSWORD        180dys
==============================================================
```

3.修改DBA_PROFILES中PASSWORD_LIFE_TIM的设置，改为ULIMITED。

```sql
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;
```

修改后设置立即生效，不需要重启数据库，此时密码永远不会过期。

#### 如果过期

**以system用户为例**

```sql
sqlplus / as sysdba

SQL> alter user system identified by root;
```

再连接数据再也不会出现密码过期的事情了。

**如果是其他用户的话，那么就使用其他用户名。**

```sql
 alter user scott identified by tiger;
```



# 写在后面

## 忘记密码

管理员模式cmd

```sql
 sqlplus /nolog
 connect  /  as  sysdba
 alter user sys identified by cj; 
 alter user system identified by cj; 
 alter user scott identified by cj; 
```

> ORA-01034: ORACLE not available
>
> 执行sartup

## linux安装

[参考](https://blog.csdn.net/Holmofy/article/details/77622284)

[解决方案](https://blog.csdn.net/Adnerly/article/details/50945906)



依赖包

```
yum install binutils compat-libstdc++ compat-libcap elfutils-libelf elfutils-libelf-devel elfutils-libelf-devel-static gcc gcc-c++ glibc glibc-common glibc-devel glibc-headers kernel-headers ksh libaio libaio-devel libgcc libgcc libgomp libstdc++ libstdc++-devel make sysstat unixODBC unixODBC-devel glibc-static
```

检查

```
# rpm -q binutils compat-libstdc++ elfutils-libelf elfutils-libelf-devel elfutils-libelf-devel-static gcc gcc-c++ glibc glibc-common glibc-devel glibc-headers kernel-headers ksh libaio libaio-devel libgcc libgomp libstdc++ libstdc++-devel make sysstat unixODBC unixODBC-devel
```

pdksh compat-libstdc

```
rpm -e ksh
rpm -ivh --force --nodeps pdksh-5.2.14-30.x86_64.rpm 
```



```
LANG=en_US ./runInstaller
```

https://blog.51cto.com/lorysun/1606952

