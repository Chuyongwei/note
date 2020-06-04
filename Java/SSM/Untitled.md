## XML

#### 组

- filter
  - fillter-name：项目名
  - filter-class：导入文件
- filter-mapping
- listener

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://java.sun.com/xml/ns/javaee" xmlns:web="http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd" id="WebApp_ID" version="2.5">
  
  	<!-- 中文乱码处理 -->
  	<filter>
  		<filter-name>CharacterEncodingFilter</filter-name>
  		<filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
  	
  		<init-param>
  			<param-name>encoding</param-name>
  			<param-value>UTF-8</param-value>
  		</init-param>
  		<init-param>
  			<param-name>forceEncoding</param-name>
  			<param-value>true</param-value>
  		</init-param>
  	</filter>
  	
  	<filter-mapping>
  		<filter-name>CharacterEncodingFilter</filter-name>
  		<url-pattern>/*</url-pattern>
  	</filter-mapping>
  	
  	<!-- 隐藏http数据 -->
  	<filter>
  		<filter-name>HiddenHttpMethodFilter</filter-name>
  		<filter-class>org.springframework.web.filter.HiddenHttpMethodFilter</filter-class>
  	</filter>
  	
  	<filter-mapping>
  		<filter-name>HiddenHttpMethodFilter</filter-name>
  		<url-pattern>/*</url-pattern>
  	</filter-mapping>
  
  	<!-- Spring配置文件信息 -->
  	<context-param>
        <!-- 起名 -->
  		<param-name>contextConfigLocation</param-name>
        <!-- 介绍位置classpath:--src下的文件 -->
  		<param-value>classpath:config/spring/applicationContext.xml</param-value>
  	</context-param>
  	<!-- ContextLoaderListener监听器 -->
  	<listener>
  		<listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
  	</listener>
  	<!-- 日志配置 -->
	  <context-param>
	   <param-name>log4jConfigLocation</param-name>
	   <param-value>classpath:config/log4j.properties</param-value>
	</context-param>
	<listener>
	   <listener-class>org.springframework.web.util.Log4jConfigListener</listener-class>
	</listener>
  
  	<!-- 配置前端控制器 -->
	<servlet>
		<servlet-name>DispatcherServlet</servlet-name>
		<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
		<!-- 初始化控制器 -->
		<init-param>
			<param-name>contextConfigLocation</param-name>
			<param-value>classpath:config/springmvc/springmvc.xml</param-value>
		</init-param>
		
		<load-on-startup>1</load-on-startup>
	</servlet>
	
    <!-- 分发器 -->
	<servlet-mapping>
		<servlet-name>DispatcherServlet</servlet-name>
		<url-pattern>/</url-pattern>
	</servlet-mapping>
	
    <!-- 各种报错处理结果 -->
	<error-page>
		<error-code>404</error-code>
		<location>/WEB-INF/errors/404.jsp</location>
	</error-page>

	<error-page>
		<error-code>500</error-code>
		<location>/WEB-INF/errors/500.jsp</location>
	</error-page>
  
	  <welcome-file-list>
	    <welcome-file>index.jsp</welcome-file>
	  </welcome-file-list>
  
</web-app>
```

