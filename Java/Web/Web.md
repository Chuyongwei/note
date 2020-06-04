# Web之过滤器与监听器

## MVC

**MVC模式**（Model–view–controller）是[软件工程](https://zh.wikipedia.org/wiki/软件工程)中的一种[软件架构](https://zh.wikipedia.org/wiki/软件架构)模式，把软件系统分为三个基本部分：模型（Model）、视图（View）和控制器（Controller）。

MVC模式最早由[Trygve Reenskaug](https://zh.wikipedia.org/w/index.php?title=Trygve_Reenskaug&action=edit&redlink=1)在1978年提出[[1\]](https://zh.wikipedia.org/wiki/MVC#cite_note-1)，是[施乐帕罗奥多研究中心](https://zh.wikipedia.org/wiki/帕羅奧多研究中心)（Xerox PARC）在20世纪80年代为程序语言[Smalltalk](https://zh.wikipedia.org/wiki/Smalltalk)发明的一种软件架构。**MVC模式**的目的是实现一种动态的程序设计，使后续对程序的修改和扩展简化，并且使程序某一部分的重复利用成为可能。除此之外，此模式透过对复杂度的简化，使程序结构更加直观。软件系统透过对自身基本部分分离的同时也赋予了各个基本部分应有的功能。专业人员可以依据自身的专长分组：

- 控制器（Controller）- 负责转发请求，对请求进行处理。

- 视图（View） - 界面设计人员进行图形界面设计。

- 模型（Model） - 程序员编写程序应有的功能（实现算法等等）、数据库专家进行数据管理和数据库设计(可以实现具体的功能)。

### 组件的互动[[编辑](https://zh.wikipedia.org/w/index.php?title=MVC&action=edit&section=1)]

  [![img](https://upload.wikimedia.org/wikipedia/commons/thumb/a/a0/MVC-Process.svg/200px-MVC-Process.svg.png)](https://zh.wikipedia.org/wiki/File:MVC-Process.svg)

  MVC组件之间的典型合作

  将应用程序划分为三种组件，模型 - 视图 - 控制器（MVC）设计定义它们之间的相互作用。[[2\]](https://zh.wikipedia.org/wiki/MVC#cite_note-posa-2)

  - **模型（Model）** 用于封装与应用程序的业务逻辑相关的数据以及对数据的处理方法。“ Model ”有对数据直接访问的权力，例如对数据库的访问。“Model”不依赖“View”和“Controller”，也就是说， Model 不关心它会被如何显示或是如何被操作。但是 Model 中数据的变化一般会通过一种刷新机制被公布。为了实现这种机制，那些用于监视此 Model 的 View 必须事先在此 Model 上注册，从而，View 可以了解在数据 Model 上发生的改变。（比如：[观察者模式](https://zh.wikipedia.org/wiki/观察者模式)（[软件设计模式](https://zh.wikipedia.org/wiki/软件设计模式)））
  - **视图（View）**能够实现数据有目的的显示（理论上，这不是必需的）。在 View 中一般没有程序上的逻辑。为了实现 View 上的刷新功能，View 需要访问它监视的数据模型（Model），因此应该事先在被它监视的数据那里注册。
  - **控制器（Controller）**起到不同层面间的组织作用，用于控制应用程序的流程。它处理事件并作出响应。“事件”包括用户的行为和数据 Model 上的改变。

  ### 优点[[编辑](https://zh.wikipedia.org/w/index.php?title=MVC&action=edit&section=2)]

  在最初的[JSP](https://zh.wikipedia.org/wiki/JSP)网页中，像[数据库](https://zh.wikipedia.org/wiki/数据库)查询语句(SQL query)这样的数据层代码和像[HTML](https://zh.wikipedia.org/wiki/HTML)这样的表示层代码是混在一起。虽然有着经验比较丰富的开发者会将数据从表示层分离开来，但这样的良好设计通常并不是很容易做到的，实现它需要精心地计划和不断的尝试。MVC可以从根本上强制性地将它们分开。尽管构造MVC应用程序需要一些额外的工作，但是它带给我们的好处是毋庸置疑的。

  首先，多个 View 能共享一个 Model 。如今，同一个Web应用程序会提供多种用户界面，例如用户希望既能够通过浏览器来收发[电子邮件](https://zh.wikipedia.org/wiki/电子邮件)，还希望通过手机来访问[电子邮箱](https://zh.wikipedia.org/wiki/电子邮箱)，这就要求Web网站同时能提供[Internet](https://zh.wikipedia.org/wiki/Internet)界面和[WAP](https://zh.wikipedia.org/wiki/WAP)界面。在MVC设计模式中， Model 响应用户请求并返回响应数据，View 负责格式化数据并把它们呈现给用户，业务逻辑和表示层分离，同一个 Model 可以被不同的 View 重用，所以大大提高了代码的可重用性。

  其次，Controller 是自包含（self-contained,指高独立内聚）的对象，与 Model 和 View 保持相对独立，所以可以方便的改变应用程序的数据层和业务规则。例如，把数据库从[MySQL](https://zh.wikipedia.org/wiki/MySQL)移植到[Oracle](https://zh.wikipedia.org/wiki/Oracle)，或者把[RDBMS](https://zh.wikipedia.org/wiki/RDBMS)数据源改变成[LDAP](https://zh.wikipedia.org/wiki/LDAP)数据源，只需改变 Model 即可。一旦正确地实现了控制器，不管数据来自数据库还是[LDAP](https://zh.wikipedia.org/wiki/LDAP)服务器，View 都会正确地显示它们。由于MVC模式的三个模块相互独立，改变其中一个不会影响其他两个，所以依据这种设计思想能构造良好的少互扰性的构件。

  此外，Controller 提高了应用程序的灵活性和可配置性。Controller 可以用来连接不同的 Model 和 View 去完成用户的需求，也可以构造应用程序提供强有力的手段。给定一些可重用的 Model 、 View 和Controller 可以根据用户的需求选择适当的 Model 进行处理，然后选择适当的的 View 将处理结果显示给用户。

## 三层架构

## 一、什么是三层架构

三层架构就是把整个软件系统分为三个层次

- 表现层（Presentation layer）
- 业务逻辑层（Business Logic Layer）
- 数据访问层（Data access layer）

如图所示：

![img](https://pic2.zhimg.com/80/v2-96f5b1a70fc8d53caa69fffac21612f1_hd.jpg)

至于为什么要分层？我通过查阅书籍，网上浏览，询问老师得出来大概以下的优点：

- 方便团队分工，一个程序员单独完成一个软件产品不是不可以，但遇到大型软件需要团队配合的时候问题就来了，由于每个程序员风格不一样，而开发软件大量的代码风格不统一就会造成后期调试和维护出现问题，然而软件分层后，每个层合理分工这样的问题便迎刃而解。
- 规范代码，在开发软件时对每个层的代码进行规范，固定开发语言的风格。
- 忽略数据库差异，当软件系统要换数据库时，只要将数据访问层的代码修改就好了。
- 实现"高内聚、低耦合"。把问题划分开来各个解决，易于控制，易于延展，易于分配资源。

总之优点很多的样子，不过分层给我最直观的感受是，代码逻辑清晰了很多。

## 二，各个层次的任务

- **表现层**（Presentation layer）：表现层可以说是距离用户最近的层，主要是用于接收用户输入的数据和显示处理后用户需要的数据。一般表现为界面，用户通过界面输入查询数据和得到需要的数据。
- **业务逻辑层**（Business Logic Layer）：业务逻辑层是处于表现层和数据访问层之间，主要是从数据库中得到数据然后对数据进行逻辑处理。
- **数据访问层**（Data access layer）：数据访问层是直接和数据库打交道的，对数据进行“增、删、改、查”等基本的操作。

不知道大家和我有没有同样的困惑：为什么中间要有业务逻辑层？为什么数据访问层不能对数据进行逻辑处理呢?少了中间一层不是减少了代码量吗？

我后来想了很久，查了很多资料，突然有所感悟，试着用自己语言描述下其中缘由。

- 用户对应为：食客；（食客通过服务员点单）
- 表现层对应为：服务员；（服务员负责食客的点单和上菜）
- 业务逻辑层对应为：主厨；（主厨从服务员那获得通知，向助手要原材料，并将原材料绘制成成品交给服务员）
- 数据访问层对应为：助手；（助手从主厨那获得通知，提交给主厨原材料）

![img](https://pic3.zhimg.com/80/v2-bcdecc5de48d924be0259b1244163a6a_hd.jpg)

这样的话，当大型个软件系统中出现问题时就可以很方便的解决问题

- 服务员服务态度不好>>>>>>换服务员
- 菜品味道不好，调味失衡>>>>>>换主厨
- 菜品的原材料不够新鲜>>>>>>>>换助手

## 三、三层架构之间如何联系起来？

一般来说都是通过实体层（entity layer）贯穿三个层次之间。实体层不属于三层架构中的任意一层。

## 四、通过实例讲解三层架构

就拿最简单的学生注册登录系统来说吧。

**1、实体层**：首先我们需要实体层来对应数据库中的表。

在本例中 用Student类对应数据库中的Student表,Student类对象作为连接三层架构的桥梁。

```java
public class Student{
	String id;		//学生的学号
	String name;	        //学生的姓名
	String password;       //学生的登录密码
	public String getId() {
		return id;
	}
	public void setId(String id) {
		this.id = id;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
	public String getPassword() {
		return password;
	}
	public void setPassword(String password) {
		this.password = password;
	}
}
```

**2、数据访问层**：这一层主要是利用sql语言直接对数据库进行最基础的操作如：“增，删，改，查”等等。不涉及更深一步的逻辑上的处理。

```java
public class DAOsupport {
	static Connection connection;							//声明Connection对象
	 static String driver ="com.mysql.jdbc.Driver";	//驱动程序名
	 static String url = "jdbc:mysql://119.23.79.90:3306/survey"; //URL指向要访问的数据库名mydata
	 static String username = "root";						//MySQL配置时的用户名
	 static String password = "MYSQL";						//MySQL配置时的密码  

	public DAOsupport(){
		try {
			Class.forName(driver);
		} catch (ClassNotFoundException e) {
			System.out.println("加载数据库驱动出错");
			e.printStackTrace();
		} 
        try {
			connection= (Connection) DriverManager.getConnection(url, username, password);
		} catch (SQLException e) {
			System.out.println("连接数据库出错");
			e.printStackTrace();
		}
	}
}
```

编写一个DAOsuport类，为其他的DAO类提供最基础的代码（比如连接数据库），减少代码的重复。

接着针对于学生类编写，StudentDAO类。该类继承DAOsuport类。

```java
public class StudentDAO extends DAOsupport{
	public Student student;   //对用户类进行数据库的操作前，先要获取用户对象
	public StudentDAO(Student student){
		super();
		this.student=student;
	}
        //返回实参
        public Student get_Stu(){
                 retrun student;
        }
	//增加用户student 
	public void save(){
		String sql="insert into student(id,name,password,)"+"values(?,?,?)";
		PreparedStatement pStatement;
		try {
			pStatement = connection.prepareStatement(sql);
			//为SQl参数赋值
			pStatement.setString(1, student.getId());
			pStatement.setString(2, student.getName());
			pStatement.setString(3, student.getPassword());
			//向user表插入记录
			pStatement.executeUpdate();
			//关闭
			pStatement.close();
		} catch (SQLException e) {
			System.out.println("保存用户时出错");
			e.printStackTrace();
		}
	}
	//删除用户student 
	public void delete() throws SQLException{
		String sql="delete from user where id=?";
			PreparedStatement pStatement=connection.prepareStatement(sql);
			pStatement.setString(1, student.getId());
			pStatement.executeUpdate();
			pStatement.close();
	}
	//更新用户student 
	public void update(){
			// TODO Auto-generated method stub
			String sql="update user set password=? where id="+student .getId();
			try {
				PreparedStatement preparedStatement=(PreparedStatement)connection.prepareStatement(sql);
				preparedStatement.setString(1,student.getPassword());
				preparedStatement.executeUpdate();
			} catch (SQLException e) {
				System.out.println("更新用户时出错");
				e.printStackTrace();
			}
	}
	//通过学号查找用户
	public Student getStuById() {
		Student student =new Student ();
		String sql="select * from student where id="+id;
		try {
			PreparedStatement preparedStatement=connection.prepareStatement(sql);
			ResultSet rs=preparedStatement.executeQuery();
			rs.next();
			student.setId(id);
			student.setName(rs.getString(2));
			student.setPassword(rs.getString(3));
		} catch (SQLException e) {
			System.out.println("通过ID查询用户时出错");
			e.printStackTrace();
		}
		return student ;
	}
}
```

**3、业务逻辑层**

```java
public class StudentService {
	public StudentDAO studentDAO;
	public StudentService(StudentDAO studentDAO){
		this.studentDAO=studentDAO;
	}
        //注册就是将student对象加入数据库
	public void  rgister(){
		studentDAO.save();
	}
        //登录就是查看登录时填写的账号和密码是否和数据库中的账号和密码一致。
	public boolean login() throws Exception{
		boolean flag=false;
                //获取数据库中这个学生的密码
                Student stu=studentDAO.getStuById();
                String password=stu.getPassword();
		/*
                实际上可以直接写成，上述代码只是为方便理解。
                String password=studentDAO.getStuById().getPassword();
                */
		if(password.equals(null)){
			throw new Exception("<学号为"+studentDAO.getStu().getId()+">不存在");
		}
                /*studentDAO.getStu()得到的是由登录信息实例化的学生对象，不是存在数据库的记录*/
		else if(!password.equals(studentDAO.getStu().getPassword())){
			throw new Exception("密码不正确");
		}
		else if(password.equals(studentDAO.getStu().getPassword())){
			flag=true;
		}
		return flag;
	}
       //注销该学生
	public void delete(){
		studentDAO.delete();
	}
        //更新该学生
	public void updte(){
		studentDAO.update();
	}
}
```

每个servlet只有一个功能，

### 三层优化

1.加入接口

​		建议面向接口编程

