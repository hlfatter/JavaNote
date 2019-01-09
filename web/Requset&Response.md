# Request

## 1.  \<url-pattern>访问路径问题

```java
1.完全路径匹配：用的和写的要一模一样：以/开头,不包含*通配符 例如：/user 
2.目录匹配：以/开头，以/*结尾，在用的时候*表示任意字符;例如：/user/*
3.扩展匹配：以*开头，固定的后缀名结尾，例如：*.do  *.abc
```

**注意：<url-pattern>可以配置多个，但是/.do等是不允许的，/abc.do运行，属于第一种，在一个项目中不允许有两个Servlet的访问路径相同**

## 2.HTTP协议

* 特点：

```java
默认端口号是80，并且http协议是基于请求和响应的，必须先有请求后有响应，一次请求一次响应
```

* http协议的组成部分

```java
1.请求行
	请求方式  请求路径url  协议版本
	GET  /day14/demo3?username=zhangsan  HTTP/1.1
    get请求和post请求的区别：
    	1.get请求参数在url后面，post请求参数在请求体中
    	2.get请求参数数据大小有限制，post请求无限制
    	3.get请求相对不安全，post请求相对安全
2.请求头：一般都是一个key对应一个value的键值对，也有一个key对应多个value的键值对；
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko)
Chrome/69.0.3497.100 Safari/537.36；判断客户端浏览器的类型以及版本，解决浏览器兼容性问题

Referer:...				记录本次请求的来源，作用：防盗链以及统计

3.请求空行：就是一个换行

4.请求体(只有post请求才有请求体)
```

## 3.Request对象获取请求行/头的方法

* Request对象原理：**服务器将浏览器发送过来的数据封装到了Request对象身上，只要是获取浏览器发送过来的数据就找Request对象**
* Request对象获取请求行/头方法

```java
1.请求行：
	* 获取请求方式：String getMethod()
    * 获取请求路径：
    	String getContextPath():获取虚拟路径
    	String getRequestURI(): 统一资源标识符
    	StringBuffer getRequestURL(): 统一资源定位符，具体到盘符下的哪个资源
    	String getServletPath():Servlet的访问路径
    * 获取get请求参数，url后面的参数：String getQueryString()
    * 获取协议版本：String getProtocol()
    * 获取远程客户端ip地址：String getRemoteAddr()
2.请求头：
	String getHeader(String name):根据一个key获取一个value
	Enumeration getHeaderNames() :获取所有请求头名称
	Enumeration getHeaders(String name) : 根据一个key获取多个value
3.请求空行
4.请求体：(只有post请求才有请求体)
    BufferedReader getReader() //获取文本信息
    ServletInputStream getInputStream() //获取二进制内容
```

## 4.Request对象获取请求参数方法

* **获取请求参数**

```java
1.String value=request.getParameter(String name);//获取一个name对应的value值
2.String values=request.getParameterValues(String name);//获取一个name对应的多个value值
3.Map<String,String[]> map=request.getparameterMap();//map集合的key表示请求参数name值；map集合的value表示请求参数值封装到数组中
```

* **请求参数中文乱码问题**

```java
原因：request缓冲区字符集不支持中文，字符默认是iso-8859-1;
解决：在获取请求参数之前设置缓冲区字符集：request.setCharacterEncoding("utf-8");
```

**注意：在tomcat8.0之后，get请求不会发生中文乱码，但是post请求参数会发生乱码问题；**

## 5.Request对象请求转发和作为域对象

* **请求转发实现页面跳转**

```java
请求转发api：request.getrequestDispatcher("/转发的路径").forward(request,response);
特点：
	1.请求转发是一次请求一次响应
	2.请求转发浏览器地址栏不会发生改变
	3.请求转发路径不带虚拟路径u
```

* **request作为域对象存值、取值、移除值**

```java
域对象：有一定作用范围的对象
作用范围：一次请求范围，请求转发时一次请求范围内
创建和销毁时间：当访问服务器被创建，当响应结束之后服务器会销毁request对象；

```

