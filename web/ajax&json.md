# Ajax&Json

## 1.ajax概念以及原始js实现方式

* 概念：异步交互方式，作用：在页面不重新加载的情况下，更新页面局部数据；特点：客户端发完请求之后不需要像同步请求一样等待响应，可以继续做其他操作已经发送其他请求
* 原生js实现ajax操作步骤

```html
1.创建核心对象XMLHttpRequest
	var　xmlhttp;
	if(window.XMLHttpRequest)
    {// code for IE7+, Firefox, Chrome, Opera, Safari 
		xmlhttp=new XMLHttpRequest();
    } else{// code for IE6, IE5
		xmlhttp= new ActiveObject("Microsoft.XMLHttp");
    }
2.open设置请求数据
	xmlhttp.open("POST","ajax_test.asp",true);
	xmlhttp.setRequestHeader("Content-type","application/x-www-form-urlencoded");//post请求需要设置，get请求不需要
3.send发送请求
	xmlhttp.send("fname=Bill&lname=Gates");//参数：post请求参数，get请求参数拼接在url后面
4.处理响应结果
    xmlhttp.onreadystatechange=function(){
		//判断readyState就绪状态是否为4，判断status响应状态码是否为200
        if(xmlhttp.readyState==4&&xmlhttp.status==200){
	//获取服务器的响应结果
	var responseText = xmlhttp.responseText;
	alert(responsetext);
        }
    }
```

**注意：如果发送的是post请求，需要在send之前额外设置一个请求头：xmlhttp.setRequestHeader("Contenttype","application/x-www-form-urlencoded");**

## 2.JQuery中的ajax实现方式

### 2.1.JQuery中的$.ajax()方式实现异步请求

```html
$.ajax({
	url:"ajaxServlet",//访问路径
	//data："name=John&location=Boston",//请求参数，2种写法，可以拼接成字符串也可以封装到js对象中
	data:{name:"John",location:"Boston"}
	type:"POST",//请求方式
	dataType:"html",//期望响应的数据类型，将来ajax会根据此类型解析响应结果
success:function(msg){//成功的回调函数，参数msg表示浏览器响应回来的数据
		alert("")
	}
})
```

### 2.2.JQuery中的$.get() 和 $.post()方法实现异步请求

```html
方法介绍：
	JQuery.get(url,[data],[callback],[type])
	JQuery.post(url,[data],[callback],[type])
参数介绍：
	url：请求路径
	[data]：get请求和post请求的请求参数，没有请求参数可以省略不写，请求参数有2种写法，可以拼接成字符串也可以封装到js对象中
	[callback]：响应成功的回调函数
	[type]：期望响应的数据类型，将来ajax会根据此类型解析响应结果，相当于$.ajax()中的dataType
使用示例：
$.get("ajaxServlet","username=tom",function(data){
	alert(data);
},"text");
```

## 3.json的概念以及语法

![1548223239325](C:\Users\ADMINI~1\AppData\Local\Temp\1548223239325.png)

## 4.jaon和java对象之间相互转换

### 4.1.java对象转成json数据

```html
1.创建ObjectMapper核心对象：ObjectMapper mapper = new ObjectMapper();
2.调用ObjectMapper的writeValueAsString(java对象)方法：
	String json = mapper.writeValueAsString(java对象);
```

### 4.2.实用的注解

```html
@JsonIgnore  //忽略该属性
@JsonFormat(pattern = "yyyy-MM-dd")  //格式化，通常添加到Date类型属性上
```

## 5.在前段html页面使用serialize()方法作用

将表单中的内容拼接成一个字符串，格式为name=value&name=value...

* **注意：使用ajax发送异步请求不能使用重定向和请求转发跳转只能在前端页面使用location.href跳转**