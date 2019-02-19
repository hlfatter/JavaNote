## 1.jsp的九大内置对象

* 4个域对象+response对象

```java
pageContext request session application + response

pageContext对象的作用：
	1.作为域对象存值取值移除值
		域对象：当前jsp页面，只要跳转走了都无法获取
	2.可以获取另外8个内置对象
```

## 2.MVC开发模式

* MVC开发模式：

```java
1.M:Model,模型。JavaBean
	完成具体的业务操作，如：查询数据库，封装对象
2.V:View,视图。JSP或者html
	展示数据
3.C:Controller,控制器，Servlet
	获取用户的请求
	调用模型
	将数据交给视图进行展示，也就是做出响应
```

* MVC开发模式分析图

![1547119280646](C:\Users\ADMINI~1\AppData\Local\Temp\1547119280646.png)

## 3.EL表达式

* EL表达式的语法和作用

```java
语法：
	${表达式};
作用：简化jsp页面java代码的书写，或者说代替jsp页面输出脚本<%=表达式%>
```

* **获取数据**

```java
前提条件：是获取域对象中的数据
语法：${域范围.键名称}
域范围对象：4个域对象对应4个域范围，pageScope、requestScope、sessionScope、applicationScope
域范围可以省略不写，如果省略不写，那么默认是从最小的域范围往大一级的域范围开始查找，直到找到为止

EL表达式获取对象属性
	${域范围.键名称.属性名}  本质上调用的是对象的getter方法
EL获取list集合、map集合中的数据
	list集合：${域范围.键名称[索引]};索引是从0开始
	map集合：${域范围.键名称.map的key}或者${域范围.键名称["map的key"]};
	例如：${requestScope.map["aaa.ccc"]}map集合有特殊的keyaaa.ccc
	注意事项：map集合的key必须是字符串
```

* EL表达式的内置对象以及empty运算符

```java
1.empty运算符：
	${empty 对象名}	可以 ${对象==null}
	${notempty 对象名}	可以 ${对象！=null}
结合三元运算符使用：${empty 对象名?"值1":"值2"}
2.EL表达式的内置对象：4个域范围(pageScope、requestScope、sessionScope、applicationScope)+cookie+pageContext
	cookie内置对象：${cookie.cookie名称.value}
	复选框选中：${empty cookie.remember?"":"checked"}
	pageContext:在EL中获取另外8个内置对象
		作用：动态获取虚拟路径：${pageContext.request.contextPath}
```

**注意：jsp中的pageContext对象和EL中的pageContext对象是同一个对象；pageScope和pageContext完全不一样，pageContext是域对象，在jsp和EL中都可以用，pageScope是EL中定义的一个表示page范围的对象，不是域对象**

## 4.JSTL的使用步骤

* 使用步骤

```java
1.导入开发jar包：javax.servlet.jsp.jstl.jar和jstl-impl.jar;
2.需要在jsp页面使用taglib指令引入标签库
	<%@ taglib prefix="c" url="http://java.sun.com/jsp\jstl/core" %>
3.使用标签库中的标签；
```

* if标签

```html
<c:if test="${not empty list}">
	遍历集合...
</c:if>
<br>
<c:if test="${number % 2 != 0}">
	${number}为奇数
</c:if>
<c:if test="${number % 2 == 0}">
	${number}为偶数
</c:if>

相当于：${number}${number % 2 != 0?"奇数":"偶数"}
```

* choose标签

```html
<c:choose>
	<c:when test="${number >= 1 && number <=3}">第一季度</c:when>
	<c:when test="${number >= 4 && number <=6}">第二季度</c:when>
    <c:when test="${number >= 7 && number <=9}">第三季度</c:when>
    <c:when test="${number >= 10 && number <=12}">第四季度</c:when>
    <c:otherwise>数字输入有误</c:otherwise>
</c:choose>
```

- foreach标签

```html
fori遍历：
	<c:forEach begin="1" end="10" var="i" step="2" varStatus="s">
	${i}>${s.count}<br>
</c:forEach>
	属性：
		begin:开始值
		end:结束值
		var:临时变量
		step:步长
			varStatus:循环状态对象(可选)
			index:容器中元素的索引，从0开始
			count:循环次数，从1开始
foreach遍历：
	<c:foreach items="${list}" var="str" varStatus="s">
		${s.index} ${s.count} ${str}<br>
</c:foreach>
	属性：
		items:容器对象
		var:容器中元素的临时变量
		varStatus:循环状态对象
		index:容器中元素的索引，从0开始
		count:循环次数，从1开始
```

* 小案例

```html
<body>
    <%
       List list = new ArrayList();
       list.add(new User("张三",23,new Date()));
       list.add(new User("李四",24,new Date()));
       list.add(new User("王五",25,new Date()));
       request.setAttribute("list",list);
       %>
        <table border="1" width="500" align="center">
            <tr>
            	<th>编号</th>
                <th>姓名</th>
                <th>年龄</th>
                <th>生日</th>
            </tr>
            <%--每遍历一次就是一个tr--%>
                <c:forEach item="${list} var="user varStatus="s">
                <tr>
                    <td>${s.count}</td>
                    <td>${s.name}</td>
                    <td>${s.age</td>
                    <td>${s.birStr}</td>
                    </tr>
                </c:forEach>
        </table>
</body>
```

**总结**

```java
1.记住用户功能：入手点：在LoginServlet登录成功之后判断用户是否勾选"记住用户名"
2.jsp九大内置对象：4个域对象+response对象
	pageContext、request、session、application、response
	pageContext对象的作用：
		1.作为域对象存值、取值、移除值
		2.获取另外8个内置对象，在EL中使用
3.MVC开发模式：
	作用：将代码按照功能进行分层
	M:(model层)：操作数据库、封装数据，一般是普通的java类
	V:(view层)：战术数据，一般是jsp/html；
	C:(controller层)：负责获取浏览器发送的请求，调用model层拿到结果，最后响应给view层展示，一般是Servlet
4.EL表达式：代替jsp页面输出脚本，EL主要是用来从域对象中获取数据展示在页面上
	语法：${表达式}
	执行运算：
		三元运算符：${number % 2!= 0?"奇数":"偶数"}、${empty cookie.remember?"":"checked"}
	从域对象中获取数据
		语法：${域范围.键名称} //常见的结果可能是 字符串、javabean对象、list集合
		结果是javabean对象：${域范围.域名称.javabean属性名}
```

