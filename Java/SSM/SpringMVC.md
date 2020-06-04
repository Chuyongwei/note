# SpringMVC

jar包

> spring-aop.jar
> spring-bean.jar
> spring-context.jar
> spring-core.jar
> spring-web.jar
>
> spring-webmvc.jar
> commons-logging.jar



### 使用springmvc代理

#### JSP没什么问题

#### web.xml

<font color="bule">用sts可以使用alt+/ 添加dispathservlet一键生成</font>

```xml
  <servlet>
  	<servlet-name>springDispatcherServlet</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<!-- 设置映射文件的路径默认路径为WEB-INF/springDispatcherServlet-servlet.xml -->
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>classpath:Springmvc.xml</param-value>
  	</init-param>
  </servlet>
  
  <servlet-mapping>
  	<servlet-name>springDispatcherServlet</servlet-name>
  	<url-pattern>/</url-pattern>
  </servlet-mapping>
```

#### Servlet

`@Controller`配置

`@RequestMapping("SpringMVCHandler")`映射网址

> @RequestMapping(value = "welcome/abc",method = RequestMethod.POST,params = "name=zs")
>
> ​		<font color = "red">                         映射地址                                       请求格式                            必须有name = zs</font>

##### 获取值

> @RequestMapping(value = "welcome2/*/abc")//任意文件夹的abc **代表任意目录

> @RequestMapping(value = "welcome3/a?c/abc")//?代表一个字符 如：welcome3/afc/abc

> @RequestMapping(value = "welcome5/{name}")//--->welcome5/zb 返回name为zb 

```
设置name="xxxx"的情况：
params= {"name2=zs","age!=23"}

name2:必须有name="name2"参数

age!=23 :    a.如果有name="age"，则age值不能是23
	     b.没有age
!name2  ：不能name="name2"的属性
```



### 过滤的条件

请求

### 过滤器()

> GET
>
> POST
>
> PUT
>
> DELETE

web.xml

```xml
			<!-- 增加HiddenHttpMethodFilte过滤器：目的是给普通浏览器 增加 put|delete请求方式 -->
	<filter>
			<filter-name>HiddenHttpMethodFilte</filter-name>
			<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
	
	</filter>
	
	<filter-mapping>
			<filter-name>HiddenHttpMethodFilte</filter-name>
			<url-pattern>/*</url-pattern>
	</filter-mapping>
```

Servlet

```java
	@RequestMapping(value="testRest/{id}",method=RequestMethod.PUT)
	public String  testPut(@PathVariable("id") Integer id) {
		System.out.println("put：改 " +id);
		//Service层实现 真正的增
		return "success" ;//  views/success.jsp，默认使用了 请求转发的 跳转方式
	}
```

JSP

```jsp
		<form action="SpringMVCHandler/testRest/1234" method="post">
			<input type="hidden"  name="_method" value="PUT"/>
		<input type="submit" value="改">
	</form>
```



Param方法

```java
	@RequestMapping(value = "testParam")
	public String testParam(@RequestParam("uname") String name,@RequestParam(value = "uage",required = false,defaultValue = "23") Integer age) {
		System.out.println("put:改"+name);
		return "success";
	}
```



```jsp
	<form action="SpringMVCHandler/testParam" method="get">
		<input name="uname" type="text"/>
		<input type="submit" value="查">
	</form>
```



获取Header请求

```
public String sa(@RequestHeader("信息头") String a) 
```

获取Cookie请求

```
public String  testCookieValue(@CookieValue("JSESSIONID") String jsessionId) 
```



### 带有数据的模块

JSP：

```jsp
${requestScope.student.id}--${requestScope.student.name }
```



```java
//ModelandView
@RequestMapping(value = "testModelAndView")
	public ModelAndView testModeAndView () {
		
		ModelAndView mv = new ModelAndView("success");//view:view/success
		Student student = new Student();
		student.setId(2);
		student.setName("zs");
		mv.addObject("student",student);//相当于request。setAttribute("student",student)
		return mv;
	}
//ModelMap
	@RequestMapping(value = "testModeMap")
	public String testModeMap (ModelMap mm) {
		
		Student student = new Student();
		student.setId(2);
		student.setName("zs");
		mm.put("student2",student);//request域
		
		return "success";
	}
	
	@RequestMapping(value = "testMap")
	public String testMap (Map<String,Object> mm) {
		
		Student student = new Student();
		student.setId(2);
		student.setName("zs");
		mm.put("student3",student);//request域
		
		return "success";
	}
	//模块方法
	@RequestMapping(value = "testModel")
	public String testModel (Model model) {
		
		Student student = new Student();
		student.setId(2);
		student.setName("zs");
		model.addAttribute("student4", student);
		
		return "success";
	}
```



`@SessionAttributes([value=]"student4")`//用request获取的同时也用session获取
`@SessionAttributes(types = Student.class)`//获取所有Student对象

`@SessionAttributes()`

`ModelAttrbute`在任何请求都会运行

约定：map中的键值对应变量



### 视图解析器

顶级接口View

视图解析器：View

常见的视图和解析器：
InternalResourceView、InternalResourceViewResolver

public class JstlView extends InternalResourceView：

springMVC解析jsp时 会默认使用InternalResourceView， 
如果发现Jsp中包含了jstl语言相关的内容，则自动转为JstlView。



JstlView 可以解析jstl\实现国际化操作

国际化： 针对不同地区、不同国家 ，进行不同的显示 

中国:（大陆、香港）     欢迎
美国：			welcome  

> i18n_zh_CH.properties		
> resource.welcome=你好
> resource.exist=退出
> i18n.properties	





xml:

```xml
	<bean id="" class="org.springframework.context.support.ResourceBundleMessageSource">\
		<property name="basename" value="i18n"></property>
	</bean>
```

