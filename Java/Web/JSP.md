

# JSP

[TOC]

<!--2019.11.20-->

### 1.什么是jsp

```
JSP全称Java Server Pages，是一种动态网页开发技术。它使用JSP标签在HTML网页中插入Java代码。标签通常以<%开头以%>结束。

JSP是一种Java servlet，主要用于实现Java web应用程序的用户界面部分。网页开发者们通过结合HTML代码、XHTML代码、XML元素以及嵌入JSP操作和命令来编写JSP。
```

### 2.JSP的优势

以下列出了使用JSP带来的其他好处：

- 与ASP相比：JSP有两大优势。首先，动态部分用Java编写，而不是VB或其他MS专用语言，所以更加强大与易用。第二点就是JSP易于移植到非MS平台上。
- 与纯 Servlet 相比：JSP可以很方便的编写或者修改HTML网页而不用去面对大量的println语句。
- 与SSI相比：SSI无法使用表单数据、无法进行数据库链接。
- 与JavaScript相比：虽然JavaScript可以在客户端动态生成HTML，但是很难与服务器交互，因此不能提供复杂的服务，比如访问数据库和图像处理等等。
- 与静态HTML相比：静态HTML不包含动态信息。

#### 格式

中文编码

```jsp
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8"%>
```

##### JSP表达式

一个JSP表达式中包含的脚本语言表达式，先被转化成String，然后插入到表达式出现的地方。

由于表达式的值会被转化成String，所以您可以在一个文本行中使用表达式而不用去管它是否是HTML标签。

表达式元素中可以包含任何符合Java语言规范的表达式，但是不能使用分号来结束表达式。

JSP表达式的语法格式：

```jsp
<%! 全局变量 %>
<% 代码片段 %>
<%= 输出表达式 %>
<%-- 注释 -->
```



### 3.内置对象

#### 标准脚本变量[[编辑](https://zh.wikipedia.org/w/index.php?title=JSP&action=edit&section=6)]

以下是永远可用的脚本变量：

- out：JSPWriter，用来写入响应流的数据
- page：servlet自身
- pageContext：一个PageContext实例包括和整个页面相联系的数据，一个给定的HTML页面可以在多个JSP之间传递。
- request：HTTP request（请求）对象
- response：HTTP response（响应）对象
- session：HTTP session（服务端会话）对象

### 4.SP动作[[编辑](https://zh.wikipedia.org/w/index.php?title=JSP&action=edit&section=8)]

JSP动作是一系列可以调用内建于网络服务器中的功能的XML标签。JSP提供了以下动作：

| **jsp:include**     | 和子过程类似，JAVA SERVLET暂时接管对其它指定的JSP页的请求和响应。当处理完该JSP页后就马上把控制权交还当前JSP页。这样JSP代码就可以在多个JSP页中共享而不用复制。 |
| ------------------- | ------------------------------------------------------------ |
| **jsp:param**       | 可以在jsp:include, jsp:forward或jsp:params块之间使用。指定一个将加入请求的当前参数组中的参数。 |
| **jsp:forward**     | 用于处理对另一个JSP或SERVLET的请求和响应。控制权永远不会交还给当前JSP页。 |
| **jsp:plugin**      | [Netscape Navigator](https://zh.wikipedia.org/wiki/Netscape_Navigator)的老版本和[Internet Explorer](https://zh.wikipedia.org/wiki/Internet_Explorer)使用不同的标签以嵌入一个[applet](https://zh.wikipedia.org/wiki/Java_applet)。这个动作产生为嵌入一个APPLET所需要的指定浏览器标签。 |
| **jsp:fallback**    | 如果浏览器不支持APPLETS则会显示的内容。                      |
| **jsp:getProperty** | 从指定的[JavaBean](https://zh.wikipedia.org/wiki/JavaBeans)中获取一个属性值。 |
| **jsp:setProperty** | 在指定的JavaBean中设置一个属性值。                           |
| **jsp:useBean**     | 创建或者复用一个JavaBean变量到JSP页。                        |

#### 标签样例[[编辑](https://zh.wikipedia.org/w/index.php?title=JSP&action=edit&section=9)]

##### jsp:include[[编辑](https://zh.wikipedia.org/w/index.php?title=JSP&action=edit&section=10)]（嵌入）

```jsp
<html>
<head></head>
<body>
<jsp:include page="mycommon.jsp" >
    <jsp:param name="extraparam" value="myvalue" />
</jsp:include>
name:<%=request.getParameter("extraparam")%>
</body>
</html>
```

将页面放在当前页面同时出现

##### jsp:forward[[编辑](https://zh.wikipedia.org/w/index.php?title=JSP&action=edit&section=11)]（跳转）

```jsp
<jsp:forward page="subpage.jsp" >
     <jsp:param name="forwardedFrom" value="this.jsp" />
</jsp:forward>
```

在本例中，请求被传递到"subpage.jsp"，而且请求的处理权不会再返回前者。

##### jsp:plugin[[编辑](https://zh.wikipedia.org/w/index.php?title=JSP&action=edit&section=12)]（标签）

```jsp
<jsp:plugin type=applet height="100%" width="100%"
            archive="myjarfile.jar,myotherjar.jar"
            codebase="/applets"
            code="com.foo.MyApplet" >
    <jsp:params>
        <jsp:param name="enableDebug" value="true" />
    </jsp:params>
    <jsp:fallback>
        Your browser does not support applets.
    </jsp:fallback>
</jsp:plugin>
```

上述[plugin](https://zh.wikipedia.org/w/index.php?title=Plugin&action=edit&redlink=1)例子说明了一种在网页中嵌入[applet](https://zh.wikipedia.org/wiki/Applet)的统一方法。在<*OBJECT*>标签出现之前，并没有一种嵌入applets的通用方法。这个标签设计得并不好，但有希望在以后加入动态属性（height="${param.height}", code="${chart}"等）和动态参数的新功能。当前jsp:plugin标签不允许动态调用applets。例如，你如果有一个图表applet需要数据点以参数形式被传入，除非数据点的数量是一个常量，否则你就不能使用ResultSet循环来创建jsp:param标签，你不得不手写每个jsp:param标签的代码。而每个上述jsp:param标签可以有一个动态命名和动态值。

##### jsp:useBean[[编辑](https://zh.wikipedia.org/w/index.php?title=JSP&action=edit&section=13)]（方法）

```jsp
<jsp:useBean id="myBean" class="com.foo.MyBean" scope="request" />类的全名称
<jsp:getProperty name="myBean" property="lastChanged" />
<jsp:setProperty name="myBean"（名） property="lastChanged"（） value="<%= new Date()%>" 值/>赋值
<jsp:setProperty name="myBean" propertyt="user" value = "asd"/>
```

scope属性可以是request, page, session or application，它有以下用意：

- request— 该属性在请求的生命周期内有效，一旦请求被所有的JSP页处理完后，那么该属性就不可引用。
- page— 该属性只是当前页中有效。
- session— 该属性在用户会话的生命周期内有效。
- application— 该属性在各种情况下都有效，并且永远不会被变为不可引用，和全局变量[global variable](https://zh.wikipedia.org/w/index.php?title=Global_variable&action=edit&redlink=1)相同。

上述例子将会用一个创建一个类的实例，并且把该实例存储在属性中，该属性将在该请求的生命周期内有效。它可以在所有被包含或者从主页面（最先接收请求的页面）转向到的JSP页之间共享。

### 5.指令

#### 	1.page指令

​	

```jsp
引用包：
<%@page import="java.util.ArrayList"%>
<%@page import="java.util.List"%>
<%@ page language="java" contentType="text/html; charset=UTF-8"
    pageEncoding="UTF-8" errorPage="/error.jsp"%>
    如果报错会跳转到error.jsp
isELIgnored = "true" 是否支持计算:支持就是false，不支持就是true
	如：${1+1}
	false: 2 true:${1+1}
```

#### 	2.include指令

```jsp
静态包含：
<%include file = "/地址">
    动态包含:<jsp:include page="header.jsp"></jsp:include>
        静态包含：在翻译时就把两个文件进行合并
        动态包含：不会合并文件，当代码执行到include时，才包含另一个文件的内容
```

#### 3.taglib指令

```
作用：在JSP页面中导入JSTL标签库。替换jsp中的java代码片段
<%@ taglib url="http://"%>
```

### 6.内置对象

## EL表达式

**表达式语言**（Expression Language），或称**EL表达式**，简称**EL**，是[Java](https://zh.wikipedia.org/wiki/Java)中的一种特殊的通用编程语言，借鉴于[JavaScript](https://zh.wikipedia.org/wiki/JavaScript)和[XPath](https://zh.wikipedia.org/wiki/XPath)。主要作用是在Java Web应用程序嵌入到网页（如[JSP](https://zh.wikipedia.org/wiki/JSP)）中，用以访问页面的上下文以及不同作用域中的[对象 ](https://zh.wikipedia.org/wiki/对象_(计算机科学))，取得对象属性的值，或执行简单的运算或判断操作。EL在得到某个[数据](https://zh.wikipedia.org/wiki/数据)时，会自动进行[数据类型](https://zh.wikipedia.org/wiki/數據類型)的转换。

#### 语法[[编辑](https://zh.wikipedia.org/w/index.php?title=表达式语言&action=edit&section=1)]

以“**${**”开始，以“**}**”作为结束：

```
${EL表达式}
```

获取某对象的值可以直接写入对象的名称，如获取对象名为“user”的对象的值：

```
${user}
```

获取某对象的属性的值使用点操作符（“.”操作符），如获取对象user的name属性和age属性的值的语法如下：

```jsp
<%
	user = ....;
%>

 ${requestScope.user.name} 
点操作
${user.name}
中括号操作
${requestScope.user["name"][""]}

${name}
${getName()}
<!--
		List list = new ArrayList<String>();
		list.add("小米");
		list.add("苹果");
		application.setAttribute("list", list);
-->
${list[0]}
<!--map:
	Map map = new HashMap<String,String>();
		map.put("zh", "中国");
		map.put("us", "美国");
		application.setAttribute("map", map);
-->
${ map.zh }
${ map["us"] }
<!-- 数字为键值时只能用第二个 -->

```

不用el的方式

```jsp
	<%
	User u = (User)session.getAttribute("user");
	out.println(u);
	%>
```



## JSTL

> jstl.jar
>
> standard.jar

#### 所谓的导包:

在文件头

```jsp
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c" %>
```

#### 使用方法

1.在某个作用域中，给某个变量赋值

```jsp
<c:set var="name" value="zhangsan" scope="request"/>
	${requestScope.name }
```

2.给某个对象的属性赋值（此方法无需scope变量）

set 添加

```
<c:set var="name" value="zhangsan" scope="request"/>
```

c:out 

```
true:<c:out value='<a href="https://www.baidu.com">百度</a>' escapeXml="true"/><br/>
	false:<c:out value='<a href="https://www.baidu.com">百度</a>' escapeXml="false"/><br/>
```

c:remove 删除

```
<c:remove var="name"/>
```

条件：

```jsp
	<c:if test="${10>2 }" var="result" scope="request">
		真221
		${result}
	</c:if>
```

选择：

```jsp
	<c:set var="role" value="学生" scope="request"></c:set>
	<c:choose>
		<c:when test="${requestScope.role=='老师' }">
			老师
		</c:when>
		<c:when test="${requestScope.role eq'学生' }">
			学生
		</c:when>
		<c:otherwise>
		管理员
		</c:otherwise>
	</c:choose>
```

循环：

```jsp
<!-- 数字循环-->
<c:forEach begin="0" end="5" step="1" varStatus="status">
	${status.index }
	test...
	</c:forEach>
	<br/>
<!-- 数组循环 -->
	<c:forEach var="name" items="${requestScope.names }">
	${name }
	</c:forEach>
	<br/>
<!-- 对象循环 -->
	<c:forEach var="student" items="${requestScope.students}"> 
		${student.sname}
	</c:forEach>
```

<font color="red">注意：不要使用itmes的值不要有多余的空格</font>