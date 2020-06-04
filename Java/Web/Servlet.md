#  JSP

## Servlet 

*网络路径*@WebServlet("/ReadCookieServlet")
[TOC]
<!--（2019.11.17）-->

Servlet拦截数据（自动配置）



```xml
<!--制作节点-->
<servlet>
    <description></description>
    <display-name>StudentServlet</display-name>
    <servlet-name>StudentServlet</servlet-name>
    <servlet-class>nuc.wcy.servlet.StudentServlet</servlet-class>
  </servlet>
<!-- 拦截节点 -->
  <servlet-mapping>
    <servlet-name>StudentServlet</servlet-name>
    <url-pattern>/StudentServlet</url-pattern>
  </servlet-mapping>
```



### 1.编码问题

```java
//1.修改请求对象编码
request.setCharacterEncoding("utf-8");
//2.1使用
//response.setCharacterEncoding("utf-8");//发的是utf-8，但浏览器会误解为GBK
//2.2
response.setContentType("text/html;charset=utf-8");
```



### 3.下载文件

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		
		//1.读取文件
		//类加载器
		//File file = new File("imgage/003.gif");
		//类加载器
		//System.out.println(file.getAbsolutePath());
		//InputStream is = DownLoadServlet.class.getClassLoader().getResourceAsStream("imgage/003.gif");
		//File file = new File("G:\\soft\\doc\\JavaEE\\day12web1\\WebContent\\imgage\\003.gif");
		ServletContext application=getServletContext();
		String realpath = application.getRealPath("imgage\\003.gif");
		System.out.println(realpath);
		FileInputStream fis = new FileInputStream(realpath);
		
		//String filename =new String(file.getName().getBytes("utf-8"),"iso-8859-1");
		String filename = URLEncoder.encode(file.getName(),"utf-8");
		//4设置头（告诉浏览器我是一个文件
		response.setHeader("content-disposition", "attachment;filename="+filename );
		ServletOutputStream sos= response.getOutputStream();
		
		byte[] buf = new byte[1024*4];
		int len = 0;
		while((len=fis.read(buf))!=-1) {
			sos.write(buf,0,len);
		}
	}

```

### 4.接收和发送信息

- ##### 在表单中

```html
<form action="/day12web1/LoginServlet" method="host">
    用户名：<input type="text" name="username"/><br/>
    密码: <input type="password" name="pwd"/>
    <input type="submit" value="登录"/>
</form>
```

- ##### 在服务器中

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//1.编码问题
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		//2.接收数据
		String username = request.getParameter("username");
		String pwd = request.getParameter("pwd");
		System.out.println("用户名："+username+"密码："+pwd);
		//3.判断
		if(username.equals("jojn")&&pwd.equals("8888")) {
			//重定向
			//response.sendRedirect("/day12web1/index.html");
			RequestDispatcher dispatcher=request.getRequestDispatcher("/ListServlet");
			dispatcher.forward(request, response);
			return;
		} else {
			response.sendRedirect("/day12web1/login.html");
		}
	}
```

##### 	转发信息

```java
//ListServlet
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		//2.接收数据
			String username = request.getParameter("username");
			String pwd = request.getParameter("pwd");
			System.out.println("用户名：..."+username+"密码："+pwd);
    
			PrintWriter out=response.getWriter();
			out.print("这是listServlet返回的数据");
	}
```



### 5.路径问题

```java
//写路径
response.sendRedirect(request.getServletContext().getContextPath()+"/index.html");
response.sendRedirect(request.getContextPath()+"/index.html");
//获取路径
String path = application.getRealPath("imgage\\003.jpg");
		System.out.println(path);
		//3.获取路径信息
		String contextpath = application.getContextPath();
		String contextpath1 = request.getContextPath();
		System.out.println(contextpath1);
```

### 一.获取Servlet的相应

#### 1.获取servlet相应

```java
//1.获取Servlet
		//1.1使用getServletContext（）；
		ServletContext application = this.getServletContext();
		System.out.println(application.hashCode());
		//1.2使用ServletConfig的getServletContext
		ServletContext application2 = getServletConfig().getServletContext();
		System.out.println(application2.hashCode());
		//1.3使用session获取
		ServletContext application3 = request.getSession().getServletContext();
		System.out.println(application3.hashCode());
		//1.4使用request获取
		ServletContext application4 = request.getServletContext();
		System.out.println(application4.hashCode());
```



- ##### 发送文件

```java
		//4.当容器使用
		application.setAttribute("welcome", "huanxing");
```

- ##### 访问


```java
ServletContext application = request.getServletContext();
		String welcome = (String)application.getAttribute("welcome");
		System.out.println(welcome);
```

- #### 例：实现servlet访问次数：

```java
//Servlret3.java
package com.qf.servlet;

import java.io.IOException;
import java.io.PrintWriter;

import javax.servlet.ServletContext;
import javax.servlet.ServletException;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;

/**
 * Servlet implementation class Servlret3
 */
@WebServlet("/Servlret3")
public class Servlret3 extends HttpServlet {
	private static final long serialVersionUID = 1L;
       
    /**
     * @see HttpServlet#HttpServlet()
     */
    public Servlret3() {
        super();
        // TODO Auto-generated constructor stub
    }

	/**
	 * @see HttpServlet#doGet(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stu
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		
		
		ServletContext application = request.getServletContext();
		Integer count = (Integer) application.getAttribute("count");
		
		if(count==null) {
			count = 1;
			application.setAttribute("count", count);
		}else {
			count++;
			application.setAttribute("count", count);
		}
		PrintWriter out = response.getWriter();
		out.write("你是第"+count+"次访问");
	}

	/**
	 * @see HttpServlet#doPost(HttpServletRequest request, HttpServletResponse response)
	 */
	protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		doGet(request, response);
	}

}

```

### 二.Cookie（状态管理）

   **Cookie**（复数形态Cookies），又称为“小甜饼”。类型为“**小型文本文件**”[[1\]](https://zh.wikipedia.org/wiki/Cookie#cite_note-1)，指某些[网站](https://zh.wikipedia.org/wiki/网站)为了辨别用户身份而储存在用户本地终端（Client Side）上的数据（通常经过[加密](https://zh.wikipedia.org/wiki/加密)） 

#### 用途

```
		因为[HTTP协议](https://zh.wikipedia.org/wiki/HTTP)是无状态的，即[服务器](https://zh.wikipedia.org/wiki/服务器)不知道用户上一次做了什么，这严重阻碍了[交互式Web应用程序](https://zh.wikipedia.org/wiki/交互式Web应用程序)的实现。在典型的网上购物场景中，用户浏览了几个页面，买了一盒饼干和两瓶饮料。最后结帐时，由于HTTP的无状态性，不通过额外的手段，服务器并不知道用户到底买了什么，所以Cookie就是用来绕开HTTP的无状态性的“额外手段”之一。服务器可以设置或读取Cookies中包含信息，借此维护用户跟服务器[会话](https://zh.wikipedia.org/wiki/会话_(计算机科学))中的状态。

​		在刚才的购物场景中，当用户选购了第一项商品，服务器在向用户发送网页的同时，还发送了一段Cookie，记录着那项商品的信息。当用户访问另一个页面，浏览器会把Cookie发送给服务器，于是服务器知道他之前选购了什么。用户继续选购饮料，服务器就在原来那段Cookie里追加新的商品信息。结帐时，服务器读取发送来的Cookie就行了

​		Cookie另一个典型的应用是当登录一个网站时，网站往往会请求用户输入用户名和密码，并且用户可以勾选“下次自动登录”。如果勾选了，那么下次访问同一网站时，用户会发现没输入用户名和密码就已经登录了。这正是因为前一次登录时，服务器发送了包含登录凭据（用户名加密码的某种[加密](https://zh.wikipedia.org/wiki/加密)形式）的Cookie到用户的硬盘上。第二次登录时，如果该Cookie尚未到期，浏览器会发送该Cookie，服务器验证凭据，于是不必输入用户名和密码就让用户登录了。
```



#### 创建

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		PrintWriter out = response.getWriter();
		out.println("欢迎访问我的小店");
		//3.添加cookie
		//Cookie myCookie = new Cookie("username", "xiaocang");
		//3.0如果有中文，编码url编码
		Cookie myCookie = new Cookie("username2",URLEncoder.encode("小白","utf-8"));
		//3.1设置cookie的有效期
		System.out.println(myCookie.getMaxAge()) ;
		myCookie.setMaxAge(60*10);//10分钟的有效期
		//3.2设置cookie的有效路径
		//myCookie.setPath("/");
		System.out.println(myCookie.getPath());
		//4.把cookie加入到response
		response.addCookie(myCookie);

		//5.读取cookie的信息
		System.out.println("..............读取cookie的内容...........");
		Cookie[] cookies = request.getCookies();
		if(cookies != null) {
			for (Cookie cookie :cookies) {
				System.out.println(cookie.getName()+".........."+URLDecoder.decode(cookie.getValue(),"utf-8"));
			}
		}
	}
```

#### 读取

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		if(request.getCookies()!=null) {
			for (Cookie cookie :request.getCookies()) {
				System.out.println(cookie.getName()+".........."+URLDecoder.decode(cookie.getValue(),"utf-8"));
			}
		}
	}

```



#### cookie的路径问题

```
cookie一般都是由于用户访问页面而创建，可并不是只有在这个cookie创建页面时才可以访问。
```

##### 	1.如何设置Cookie设置路径

```
通过Cookie的setPath方法设置路径
```

##### 	2.缺陷

1. Cookie会被附加在每个HTTP请求中，所以无形中增加了流量。
2. 由于在HTTP请求中的Cookie是明文传递的，所以安全性成问题，除非用[HTTPS](https://zh.wikipedia.org/wiki/HTTPS)。
3. Cookie的大小限制在4KB左右，对于复杂的存储需求来说是不够用的。

   ###### 	防治方法

```
Cookie的setHttpOnly(true)方法
```

### 三.状态管理-Session

#### 	1.什么是Session

```
在计算机科学领域来说，尤其是在网络领域，会话（session，Microsoft Windows 中文版译作工作阶段）是一种持久网络协议，在用户（或用户代理）端和服务器端之间创建关联，从而起到交换数据包的作用机制，session在网络协议（例如telnet或FTP）中是非常重要的部分。

在不包含会话层（例如UDP）或者是无法长时间驻留会话层（例如HTTP）的传输协议中，会话的维持需要依靠在传输数据中的高级别程序。例如，在浏览器和远程主机之间的HTTP传输中，HTTP cookie就会被用来包含一些相关的信息，例如session ID，参数和权限信息等。
```

#### 	2.如何获得Session

```java
		HttpSession session = request.getSession();//系统会自动创建cookie
		System.out.println("sessionId："+session.getId());
		System.out.println("session创建时间:"+new Date(session.getCreationTime()).toLocaleString());
		System.out.println("session最后一次访问时间："+new Date(session.getLastAccessedTime()).toLocaleString());
		System.out.println("session过时时间："+session.getMaxInactiveInterval());
		
		//3.向session存放数据
		session.setAttribute("username" , "zhangsan");
```

#### 	3.接收Session

```java
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		HttpSession session = request.getSession();
		String name = (String)session.getAttribute("username");
		System.out.println(name);
	}
```

#### 	4.自定义Session

```java
Cookie cookie = new Cookie("JSESSIONID",session.getId());
			cookie.setMaxAge(60*5);
			cookie.setPath("/day13web1");
			cookie.setHttpOnly(true);
			response.addCookie(cookie);
```



#### 	5.退出登录

```java
	protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		//1.设置session失效
		HttpSession session = request.getSession();
		session.removeAttribute("username");//从session中移除
		session.invalidate();
		PrintWriter out = response.getWriter();
		out.write("推出成功");
	}
```

#### 6.用户失效

##### 		1.设置时间

```xml
 1 使用HttpSession的session.setMaxInactiveInterval(20*60);设置，单位秒
 2 在WEB—IF的web.xml中配置，单位分
   <session-config>
  	<session-timeout>20</session-timeout>
  </session-config>

```

##### 		2.失效的几种方法：

```
1.超过设置的时间
2.主动调用invalidate方法
3.服务器主动或异常关闭
注意：浏览器关闭不会让Session失效
```





##### 	应用：

- ##### 例如：

```java
//UserServlet
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub
		//1.读取cookie
		Cookie[] cookies = request.getCookies();
		if(cookies != null) {
			for(Cookie cookie :cookies) {
				if(cookie.getName().equals("userinfo")) {
					String val = cookie.getValue();
					String[] vals = val.split("#");
					if(vals[0].equals("admin")&&vals[1].equals("8888")) {
						HttpSession session=request.getSession();
						session.setAttribute("username", vals[0]);
						request.getRequestDispatcher("/day13web2/index.html").forward(request, response);
						return;
					}
				}
			}
		}
		response.sendRedirect("/day13web2/login.html");
	}
```

```java
//LoginServlet
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
		// TODO Auto-generated method stub、
		//1.乱码
		request.setCharacterEncoding("utf-8");
		response.setContentType("text/html;charset=utf-8");
		//2，获取数据
		String username = request.getParameter("username");
		String password = request.getParameter("password");
		if(username.equals("admin")&&password.equals("8888")) {
			//3.获取session
			HttpSession session = request.getSession();
			session.setAttribute("username", username);
			//4.创建
			Cookie cookie = new Cookie("userinfo","admin#888");
			cookie.setMaxAge(5*60);
			cookie.setPath("/day13web2");
			cookie.setHttpOnly(true);
			response.addCookie(cookie);
			//5重定向
			response.sendRedirect("/day13web2/index.html");
			return;
		}else {
			response.sendRedirect("/day13web2/login.html");
		}
	}
```

## Web

### 四.过滤器

#### 		1.什么是过滤器

```
Filter在英文中是过滤器的意思，当然在此处的使用也是完美的切合了它的意思，我们使用filter的主要目的就是完成一个过滤的作用。可以在一个请求到达servlet之前，将其截取进行逻辑判断，然后决定是否放行到请求的servlet。也可以在一个response到达客户端之前，截取结果进行逻辑判断，然后决定是否允许返回给客户端。
```

#### 		2.如何编写

```
1.编写Java类实现Filter接口
2.重写doFile方法
3.设置拦截的url
```

#### 		3.过滤器的配置

​				注解式配置

​					在自定义的Filter类中使用注解 @WebFilter（“/*”）确定为全局规则

​				xml配置				

```xml
  //起名（导入文件）
  <filter>
  	<filter-name>myfilter1</filter-name>
  	<filter-class>com.qf.filter.MyFilter1</filter-class>
  </filter>
    <filter>
  	<filter-name>myfilter2</filter-name>
  	<filter-class>com.qf.filter.MyFilter2</filter-class>
  </filter>


  //应用 （编写顺序）
  <filter-mapping>
  	<filter-name>myfilter2</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>

  <filter-mapping>
  	<filter-name>myfilter1</filter-name>
  	<url-pattern>/*</url-pattern>
  </filter-mapping>
```

​	filter的使用默认按照类名的字符串顺序来排

​			第一次访问时，把index.html读取发送欸浏览器，并在服务其中缓存一份

​				第二次访问时，由于服务器中已有一份，比对时间，如果时间一样直接返回304

<img src="data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAABIAAAAKICAIAAACHSRZaAAAAAXNSR0IArs4c6QAAAARnQU1BAACxjwv8YQUAAAAJcEhZcwAAEnQAABJ0Ad5mH3gAADVqSURBVHhe7d3NjezWuTVgjwwDBgwPNLgGBNyJUviS0EgZ3Bw0UAY3BwGK4MZwoAycgpSBBoIGggAB/qrI9VazWT+sOoe9u/Y+zwPCUK8iWfzzZi+xuvSX/wAAANCEAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgYAANCIAgbX/fTzn/mn2Y+/ffnN7z/lh1c+fPfLl9/9kR9e/PHtV798+2N++OmHX7/86rcP+QkAgM+QAgbXHOrWV6tadSxUX//wupVNFgVsWbqW//zn999cLGkx1bNfXk3TzBfyKyUQAICnp4DBDT///vXrwvPTD799++OpgL30q+0CNte5C1OeiR2L1uKNTitc5asfAQDoigIG24496qw4HZrV/QXs+A8XH52dKGAAAJ8BBQwedvww4dSm7i1gx1en1nT6h3PHZrXqeKcCtsoVMACAXilgcNmpZcXiCzlePde6o4Ad/vfX738+/vhhLmPHHjUnLzwBAwD4DChgcNmfP/24+AOw419wVWU6/mFYCtV9BWz+55XjS7XINafmlj8hu/0hRgAAnp4CBre8rlhTGZs+Ezh/c8brVzfa1C0XPme4Ma0foAEA0AMFDO40fSPioW5dfNi1KmDHH9eVaT1tfyfHoWVNT+E8+AIAGIUCBnc7PaeqTxXeLGC3Hoit/8Dstem/GHbqXT58CAAwDgUMbnmpWJPjj6f/ctcDBWz5J2RHVwrY/J8de/UWs/l91TAAgN4pYHDDy/dtTFKQTsleBSz96jjdem42rbbmXNRCAAB6oYDBdYvv2zg69p9vfvv2m1NNureAVXE6rerCE7AL38MxreFC7mvoAQB6pYDBdcselW/F+Pn8cdbBXI2uFLDpj7i+/e7wv+cF7KXCHde/aFanNazy1Y8AAHRFAYOrju0oz6lefRPG9CUZLy1o8Wzqq0Qn8/Or44LH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AACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgEQUMAACgif/85/8D85fW1Krd7A8AAAAASUVORK5CYII=" alt="服务器缓存303" style="zoom: 67%;" />

控制从服务器缓存

```java
HttpServletRequest req = (HttpServletRequest) request;
		HttpServletResponse res = (HttpServletResponse) response;
		
		//控制器浏览器缓存（控制第一次不走浏览器缓存）
		res.setDateHeader("Expires", -1);
		res.setHeader("Cache-Control", "no-cache");//过期时间
		res.setHeader("Pragma", "no-cache");//批注
		System.out.println("jj");
		//放行
		chain.doFilter(req, res);
```



```java
//控制器浏览器缓存（控制第一次不走浏览器缓存）
		res.setDateHeader("Expires", System.currentTimeMillis()+20000);
		res.setHeader("Cache-Control", "no-cache");//过期时间
		res.setHeader("Pragma", "no-cache");//批注
		System.out.println("yyy");
		chain.doFilter(request, response);
```

### 压缩文件

### 五.监听器

```java
package com.qf.listener;

import javax.servlet.ServletRequestEvent;
import javax.servlet.ServletRequestListener;
import javax.servlet.annotation.WebListener;

@WebListener
public class MyServletRequestListener implements ServletRequestListener{
	@Override
	public void requestDestroyed(ServletRequestEvent sre) {
		// TODO Auto-generated method stub
		System.out.println("请求对象销毁了"+sre.getServletRequest().hashCode());
	}
	
	@Override
	public void requestInitialized(ServletRequestEvent sre) {
		// TODO Auto-generated method stub
		System.out.println("请求对象初始化了了"+sre.getServletRequest().hashCode());
	}
}
```

```java
package com.qf.listener;

import javax.servlet.ServletContext;
import javax.servlet.ServletContextAttributeEvent;
import javax.servlet.ServletContextAttributeListener;
import javax.servlet.ServletRequestAttributeEvent;
import javax.servlet.ServletRequestAttributeListener;

public class MyServletRequestAttributeListener implements ServletRequestAttributeListener{
	@Override
	public void attributeAdded(ServletRequestAttributeEvent srae) {
		// TODO Auto-generated method stub
		System.out.println("request对象中添加数据"+srae.getName()+"..."+srae.getValue());
	}
	@Override
	public void attributeRemoved(ServletRequestAttributeEvent srae) {
		// TODO Auto-generated method stub
		System.out.println("request对象中移除数据"+srae.getName()+"..."+srae.getValue());
	}
}

```




# （（（（（（（（（（（（（（（（（（（（（（