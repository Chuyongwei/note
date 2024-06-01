## 触发器

格式

```sql
CREATE TRIGGER trigger_name
BEFORE|AFTER <触发事件> ON oversee_table
FOR EACH ROW|STATEMENT
<处理语句>
```

例

建表：

```sql
CREATE TABLE employees_audit (
    id INT AUTO_INCREMENT PRIMARY KEY,
    employeeNumber INT NOT NULL,
    lastname VARCHAR(50) NOT NULL,
    changedat DATETIME DEFAULT NULL,
    action VARCHAR(50) DEFAULT NULL
); 
```

设置触发器

```sql
CREATE TRIGGER before_employee_update 
	BEFORE UPDATE ON employees 
	FOR EACH ROW
	INSERT INTO employees_audit 
	SET action = 'update',
	employeeNumber = OLD.employeeNumber,
	lastname = OLD.lastname,
	changedat = NOW( );
```

注意：

- sql语句中`old`指代修改前的数据，`new`代表修改后的数据，因此：

  `insert`只有`new`，

  `update`有`new`和`old`

  `delete`有`old`