<!--2020.01.20-->

## 1.搭建

1.装包

2.搭服务器

3.做代理截取

1. web.xml

   ```xml
   <!DOCTYPE web-app PUBLIC
           "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
           "http://java.sun.com/dtd/web-app_2_3.dtd" >
   
   <web-app>
     <display-name>Archetype Created Web Application</display-name>
   
     <servlet>
       <servlet-name>dispatcherServlet</servlet-name>
       <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
       <init-param>
         <param-name>contextConfigLocation</param-name>
         <param-value>classpath:springmvc.xml</param-value>
       </init-param>
       <load-on-startup>1</load-on-startup>
     </servlet>
     <servlet-mapping>
       <servlet-name>dispatcherServlet</servlet-name>
       <url-pattern>/</url-pattern>
     </servlet-mapping>
   
   
     <!-- 配置乱码过滤器-->
     <filter>
       <filter-name>characterEncodingFilter</filter-name>
       <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
       <init-param>
         <param-name>encoding</param-name>
         <param-value>UTF-8</param-value>
       </init-param>
     </filter>
     <filter-mapping>
       <filter-name>characterEncodingFilter</filter-name>
       <url-pattern>/*</url-pattern>
     </filter-mapping>
   </web-app>
   
   ```

2. springmvc.xml

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:mvc="http://www.springframework.org/schema/mvc"
          xmlns:context="http://www.springframework.org/schema/context"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="
           http://www.springframework.org/schema/beans
           http://www.springframework.org/schema/beans/spring-beans.xsd
           http://www.springframework.org/schema/mvc
           http://www.springframework.org/schema/mvc/spring-mvc.xsd
           http://www.springframework.org/schema/context
           http://www.springframework.org/schema/context/spring-context.xsd">
       <!-- 开启扫描 -->
       <context:component-scan base-package="com.wcy"></context:component-scan>
   
       <!-- 视图解析器 -->
       <bean id="internalResourceViewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
           <property name="prefix" value="/WEB-INF/pages/"/>
           <property name="suffix" value=".jsp"/>
       </bean>
   
       <!--配置文件解析器-->
       <bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
           <property name="maxUploadSize" value="10485760"/>
       </bean>
       <!-- 开启springmvc注解：开启默认配置 -->
       <mvc:annotation-driven/>
   </beans>
   ```

3. 控制台

   ```java
   @Controller
   @RequestMapping("/user")
   public class My {
   
       @RequestMapping("/a")
       public String a(){
           System.out.println("sadf");
           return "success";
       }
   }
   ```

   



 

<img src="../image/ssm/springmvc执行流程原理.jpg" style="zoom:80%;" />

## 注释

1. `RequestMapping('url')`

   - 作用：用于建立URL和冲击力方法的地址

   - 位置：类和方法之间

   - 属性：
     - value：用于指定请求
     - method：用于请求指定请求方式
     - params：限制条件 如 params={”account=asd"
     - headers:用于指定限制请求消息的条件
2. `RequestParam`
   - 作用：