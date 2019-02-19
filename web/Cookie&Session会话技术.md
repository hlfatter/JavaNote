# 1.会话技术-Cookie(客户端会话技术)

* 一次会话从打开浏览器开始访问服务器项目开始到浏览器关闭会话就结束了；其中的多次请求都属于一次会话范围内
* **Cookie的实现原理**

```java
1.原理：
	Cookie属于客户端技术，当客户端第一次访问服务器时，在服务器端创建Cookie对象，使用response的响应头将Cookie发送给浏览器保存起来。当浏览器再次访问服务器该项目时，浏览器会自动将Cookie发送过来，那么在服务端就可以继续从Cookie中取值或者存储值
2.常用api
	创建Cookie对象：Cookie cookie = new Cookie(String nae, String value);
	将cookie添加到响应中：response.addCookie(cookie);
	获取所有Cookie:Cookie[] cookies = request.getCookies();
	获取Cookie的name：String name = cookie.getValue();
	获取cookie的value：String value = cookie.getValue();
	在响应之前设置有效时长：cookie.setMaxAge(int time);
```

* Cookie 的使用细节

```java
1.在一个Servlet中，可以向浏览器响应多个Cookie
2.Cookie的有效时长，默认有效时长是一次会话
	cookie.setMaxAge(int time);//秒为单位，整数表示最大生存时间，-1表示默认时长也就是一次会话，0表示删除Cookie
3.Cookie 中保存中文问题
	tomcat 8之前不允许直接保存中文
	tomcat 8之后允许直接保存中文；但是要注意保存空格字符的问题
4.Cookie的有效路径：浏览器会根据path路径发送Cookie
	如果没有设置path路径，那么默认路径是：创建Cookie的Servlet所在目录
	cookie.setPath("/day16");// 访问day16项目下的所有资源都会携带Cookie
	cookie.setPath("/day16/demo");// 访问day16项目中demo里面的所有资源都会携带Cookie

```

* Cookie的注意事项

```java
1.浏览器保存Cookie的数量和大小有限制
2.Cookie只能保存字符串类型的数据，并且是不太重要的数据
使用场景：
	记录上一次访问该网站的事件，记住用户名，网站的设置信息(无登录状态下)
```

# 2.jsp本质以及脚本元素

* jsp概念：JSP全名为Java Servlet Pages,中文名叫java服务器页面
* 作用：Servlet想在浏览器展示数据很不方便，所以sun公司推出了新的动态web资源就是jsp
* **原理：jsp的本质是Servlet**
* 脚本元素，可以和html代码嵌套使用

```java
1.成员声明脚本：<%! int i=100; %>  声明的变量在类的成员位置，也可以声明成员方法
2.程序代码脚本：<% int i=100; %>  声明的变量是局部变量，在_jspService()方法中
3.输出脚本：<%=表达式%> 将表达式结果使用out对象输出到页面上
```

* 案例：九九乘法表

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
	<title>九九乘法表</title>
</head>
<body>
    <%
    	for (int i = 1; i < 10; i++) {
    		for (int j = 1; j <=i; j++) {
    %>
    		<%=i%>*<%=j%>=<%=i*j%>&nbsp;&nbsp;&nbsp;
    <%
   	 		}
    %>
    	<br>
    <%
    	}
    %>
</body>
</html>
```

# 3.会话技术-Session(服务端技术)

* **session实现原理**
* **session作为域对象存值、取值、移除值**

```java
1.作用范围：一次会话范围，可以包含多次请求，范围比request域对象大，比ServletContext域对象范围小
2.创建和销毁时间：
	创建：第一次调用HttpSession session = request.getSession();
	销毁：1.服务器关闭(正常关闭可以获取数据，非正常关闭就彻底获取不到数据了) 2.手动调用invalidate(); 3.默认30分钟到期了
3.api：
	存值：request.setAttribute(String name,Object value)
	取值：Object value = request.getAttribute(String name)
	移除值：request.removeAttribute(String name)
```

* 设置session的存活时间

```html
在项目的web.xml中配置
<session-config>
	<session-timeout>分钟</session-timeout>
</session-config>
```

