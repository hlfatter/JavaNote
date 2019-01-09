# Tomcat&servlet

## 1.web相关概念

```java
1.软件架构
	1.C/S：客户端/服务器端
	2.B/S：浏览器/服务器端
2.资源分类
	1.静态资源：所有用户访问后，得到的结果都是一样的，称为静态资源。静态资源可以直接被浏览器解析
	2.动态资源：每个用户访问后，得到的结果可能不一样。称为动态资源。动态资源被访问后，需要先转换为静态资源，贼返回给浏览器
3.网络通信的三要素
	1.IP:电子设备在网络中的唯一标识，也叫主机名
	2.端口:应用程序在计算机中的唯一标识
	3.传输协议:规定了数据传输的规则
		*tcp：安全协议，速度稍慢
		*udp：不安全协议，速度快
```

## 2.Tomcat的启动问题

```java
1.安装：解压tomcat安装包到非中文目录下就可以使用
2.启动：双击bin/startup.bat,启动cmd窗口不要关闭
3.测试是否启动成功：在浏览器中输入：http://localhost:8080
4.卸载：删除解压后的tomcat即可
5.关闭：正常关闭
	方法1：双击bin/shutdown.bat;
	方式2：在启动窗口ctrl+c
```

* **启动tomcat的两种异常：现象都是启动窗口一闪而过**

```java
原因1：jdk的环境变量没有配置好，JAVA_HOME没有配置
解决：配置JAVA_HOME环境变量
	鼠标右键计算机属性-->高级环境变量-->系统变量-->新建
	变量名：JAVA_HOME
	变量值：jdk的根目录
原因2：端口号冲突，一般是启动了多个tomcat或者之前的tomcat没有正常关闭进程还在
异常信息（log/catalina.2018-12-17.log）：aused by: java.net.BindException: Address already in
use: bind
解决1：关闭占用8080端口的程序，找到占用8080端口的程序，在cmd中输入netstat -ano查看端口号找到pid，在
任务管理器中找打指定pid的进程并结束，tomcat是java程序；
解决2：修改自己的端口号；
```

## 3.项目的3种发布方式

```java
1.直接将项目放到webapps目录下即可

2.配置conf/server。xml文件
	在<Host>标签体中配置
		<Context doBase="D:\hello" path="/hehe">
		docBase:项目存放的路径
		path：虚拟目录
3.在conf\Catalina\localhost创建任意名称的xml文件，在文件中编写
	<Context docBase="D:\hello"/>
	虚拟目录：xml文件的名称
```

* 访问路径解析

![1546523301631](C:\Users\ADMINI~1\AppData\Local\Temp\1546523301631.png)

协议：浏览器和服务器遵循的规范

## 4.Servlet概述以及实现思路

* 概念：servlet是运行在Web服务器中的小型java程序。作用是：servlet通常通过HTTP接收和响应来自Web客户端的请求
* 创建Servlet的基本思路

```java
1.创建一个java类实现Servlet接口
public class ServletDemo1 implements Servlet{...}
2.配置Servlet
	2.1在web.xml中配置Servlet
	<servlet>
		<!--取别名-->
		<servlet-name>ServletDemo1</servlet-name>
		<!--Servlet全类名-->
		<servlet-class>com.itheima.ServletDemo1</servlet-class>
	</servlet>
	<!--配置Servlet访问信息-->
	<servlet-mapping>
		<!--使用别名-->
		<servlet-name>ServletDemo1</servlet-name>
		<!--Servlet的访问路径-->
		<url-pattern>/abc</url-pattern>
	</servlet-mapping>
	2.2.使用注解配置(很常用)
		@WebServlet("/servletDemo1")
		public class ServletDemo1 implements Servlet {...}
```

## 5.Servlet生命周期

* 生命周期：当我们第一次访问Servlet时，服务器会创建Servlet对象，调用其init()方法，我们可以在service()方法中接收浏览器发送过来的请求以及对浏览器作出响应；当服务器正常关闭或者项目从服务器中移除之后Servlet会被销毁，destory()方法被调用。
* 配置启动时加载

```java
原因：如果init()方法中加载的配置文件耗时比较长，那么就会导致第一个访问的用户访问时间很长，影响用户体验
解决：配置启动时加载，在servlet标签中配置<load-on-startup>2</load-on-startup>
```

## 6.idea与tomcat相关配置

* web项目是依赖于tomcat环境

## 7.手动发布项目

```java
1.导入项目
	添加tomcat依赖：Project Structture --> Modules --> Dependencies -->+-->Library-->自己的tomcat
2.添加web目录标记，目的是让idea认识web目录，才能将web目录中的东西进行发布（如果没有蓝色的点就需要做）
	Project Structure-->facets-->+-->Web--->选中自己的模块(项目)
3.将项目编译成发布版的项目；（必须的）
	Project Structure-->Artifacts--->+-->Web Appliation Exploded（第二个）--->from modules--->
选中自己的模块(项目)
4.添加到tomcat中进行发布（必须的）
	Edit configuration--->选中自己的tomcat--->Deployment--->+--->artifact--->设置旁边的虚拟路径
```



