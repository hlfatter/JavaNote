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
api:
	存值：request.setAttribute(String name,Object value);
	取值：Object value=request.getAttribute(String name);
	移除值：request.removeAttribute(String name);
```

# 6.BeanUtils

* ## Beanutils工具类

```java
作用：给javabean属性赋值
javabean：以前的标准java类，要求：
	* 私有的成员变量
	* 空参构造
	* public类型的getter和setter方法
javabean的属性：不同于成员变量，是getter和setter方法去掉set和get剩余部分

常用方法：
	BeanUtils.setProperty(javabean对象.属性名.属性值);
	String value=BeanUtils.getProperty(javabean对象.属性名);
	//1.获取请求参数,并且封装请求参数
        Manager manager=new Manager();
        try {
                        	       BeanUtils.populate(manager,request.getParameterMap());//很常用
        } catch (IllegalAccessException e) {
        e.printStackTrace();
        } catch (InvocationTargetException e) {
        e.printStackTrace();
        }
```

**注意：html表单的name属性值要和javabean的属性一致**

# Response

## 1.http协响应内容

```java
1.响应行：
	协议版本  状态码   状态描述
	HTTP/1.1  200		ok
	常见的状态码：
		200：响应成功了
		302：重定向
		304：使用缓存
		404：访问资源不存在，也就是url路径错误
		405：get请求没有对应的doGet方法，post请求没有对应的doPost方法；
		500：服务器java代码有异常，一般是XxxException
2.响应头：一般都是一个key对应一个value的键值对，也有一个key对应多个value的键值对；
	Content-Type:告诉浏览器响应的数据类型以及采用的字符集
	Content-disposition:告诉浏览器以文件下载的形式解析响应内容；
	location:告诉浏览器重定向到哪个资源页面
3.响应空行
4.响应体：响应的内容
```

## 2.response对象重定向实现页面跳转

`api：response.sendRedirect("/虚拟路径/资源页面名词");`

* **请求转发和重定向的区别：**

```java
1.请求转发是一次请求一次响应，重定向两次请求两次响应
2.请求转发地址栏不会发生改变，重定向地址栏会改变
3.请求转发路径不包含虚拟路径，重定向路径包含虚拟路径
4。如果跳转前后需要使用request域对象进行数据传递，那么只能使用请求转发，否则用重定向
```

## 3.Web中页面访问路径问题

```java
相对路径：相对当前页面访问路径中的所在目录，相对路径以./(当前目录)或者../(上一级目录)开头，使用相对路径要清除当前页面的访问路径和要跳转资源的访问路径
	当前页面路径：http://localhost:8080/UManager/loginServlet
	要跳转的路:http://localhost:8080/UManager/list.html
	相对路径写法：./list.html
绝对路径：以/开头，默认省略了http://localhost:8080，
	写绝对路径的关键：虚拟路径
	客户端路径：从客户端访问服务器的路径就是客户端路径，需要写虚拟路径
	服务端路径：服务器内部跳转的路径，请求转发
	
```

**总结：目前使用绝对路径，处理请求转发之外，其他的都要写虚拟路径**

## 4.response响应内容以及中文乱码问题

```java
1.使用字符流响应中文：
	response.getWriter().write("响应内容");
2.字符流中文乱码问题
	原因：
		1.response缓冲区默认字符集是iso-8859-1,不支持中文
		2.response缓冲区字符集和浏览器采用的字符集不一致
	解决办法：设置response缓冲区和浏览器采用的字符集都是utf-8即可，在获取字符流之前设置；
	response.setContentType("text/html;charset=utf-8");
3.字节流响应中文：
	response.getOutputStream().write("响应内容".getBytes("字符集"))
4.字节流中文乱码问题
	原因：浏览器字符集和编码的字符集不一致
	解决：告诉浏览器采用的字符集和编码字符集一致
	使用response.setContentType("text/html;charset=utf-8")也可以解决字节流响应中文乱码问题
```

**总结：不管是字节流还是字符流响应中文，只要在获取流对象之前调用response的setContentType方法即可**

​	**response.setContentType("text/html;charset=utf-8")**

* 验证码换一换的功能

```html
1.在.html页面中显示验证码
<a href="javascript:refershcode()"><img src="..." title="看不清点击刷新" id="vcode"/></a>
2.实现换一换功能
function refreshCode() {
	//实现验证码换一换
document.getElementById("vcode").src="...?time="+new Date().getTime();
}
```

## 5.ServletContext作为域对象存值、取值、移除值

```java
1.作用范围：整个web项目
2.创建和销毁时间：服务器启动的时候会为每个web项目创建一个独有的ServletContext对象，服务器关闭或者项目被移除之后销毁
3.存值、取值、移除值API：
	存值：request.setAttribute(String name, Object value);
	取值：Object value=request.getAttribute(String name);
	移除值：request.removeAttribute(String name);
```

## 6.ServletContext对象读取文件IO流路径

```java
1.读取src中的文件资源：
	1.1.类加载器：
		String path=类名.class.getClassLoader().getResource("d.txt").getPath();
		InputStream is = 类名.class.getClassLoader().getResourceAsStream();
2.读取web中的文件资源，只能使用ServletContext对象读取
	String realPath = servletContext.getRealPath();
	InputStream is = servletContext.getResourceAsStream();
	InputStream is = servletContext.getResourceAsStream();
```

## 7.response对象实现文件下载

* 修改工具类

```java
public class DownLoadUtils{
    public static String getFileName(String agent, String fileName) throws UnsupportedEncodingException{
        if(agent,contains("MSIE")) {
            //IE浏览器
            fileName= URLEncoder.encode(filename, "utf-8");
            fileName = filename.replace("+"," ")
        }else if (agent.contains("Firefox")) {
            // 火狐浏览器（修改了）
            Base64.Encoder encoder = Base64.getEncoder();
            filename ="=?utf-8?B?"+ encoder.encodeToString(filename.getBytes("utf-8")) +"?
            =";
        } else {
            // 其它浏览器
            filename = URLEncoder.encode(filename, "utf-8");
        }
            return filename;
    } 
}
```

