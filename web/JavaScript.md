# 一、JavaScript基础

## 1.JavaScript概念、作用和引用方式

* 概念：JavaScript是一门客户端脚本语言，可以用于制作页面动画，表单校验以及和服务器交互

* JavaScript的组成部分

  ![1545448052960](C:\Users\ADMINI~1\AppData\Local\Temp\1545448052960.png)

* 引入方式

```js
1.内部js：将js代码写在html页面的一组script标签中
	<script>
    	alert("内部js")
	</script>
2.外部js：
	第一步:定义一个以.js结尾的文件，该文件中写js代码，不包含<script>标签
	第二步:在html页面通过script标签的src属性引入js文件
    <script src="js/a.js"><script>
```

**注意1：script标签可以写在任意标签内部，影响的只是先后加载顺序；一般开发中是放在body结束标签之前**

**注意2：一组script标签中如果引用外部的js，那么这组script标签中就不能再写js代码，需要重新使用一组新的script标签**

## 2.JavaScript变量和数据类型

* **数据类型**

```javascript
1.原始数据类型(基本数据类型)：
    number:整数/小数/NaN
    string:字符串 "abc" "a" 'abc'
    boolean:true和false
    null:一个对象为空的占位符
    undefined:未定义
2.引用数据类型：对象
	var date = new Date();
```

* **定义变量。使用var关键字** 

## 3.JavaScript运算符和流程控制

**注意：for循环快捷键：itar回车正着遍历或者ritar回车就是倒着遍历**

* 案例-九九乘法表

```javascript
<head>
    <meat charset="UTF-8">
    <title>title</title>
	<style>
        td{
            border:1px solid;
        }
	</style>
	<script>
        document.write("table align='center' cellspacing='0'");
		//1.完成基本的for循环嵌套，展示乘法表
        for(var i=1; i <= 9; i++) {
            document.write("<tr>");
            for(var j=1; j <= 9; j++) {
                //document.write();
                //输出 
                document.write("<td>"+i+"*"+j+"="+(i*j)+"&nbsp;&nbsp;&nbsp;"+"<td>")
            }
            //输出换行
            document.write("</tr>")
        }
		//完成表格嵌套
		document.write("</table>");
	</script>
</head>
```

## 4.JavaScript函数

* **函数定义的两种方式**

```javascript
方式1：普通函数
    function 函数名(形参列表) {
        //方法体
    }
    调用方法：函数名(传递的实参)
    例如：
    function add(m,n) {
        return m+n;
    }
    调用方法:add(10,20);
方法2：匿名函数，需要将匿名函数赋值给一个变量，变量名就是函数名
    var 变量名=function (形参列表){
        //方法体
    }
    调用方法：变量名(传递的实参)
    例如：
    var add=function(m,n) {
        return m+n;
    }
    调用方法：add(10,20);
```

**注意1：申明函数时使用function关键字，形参不需要使用var，返回值类型也不需要写；**

**注意2：JavaScript中的函数是一个引用数据类，是一个function类型的对象，函数名就是对象名**

**注意3：在JavaScript中同名的函数只会覆盖，没有重载；函数的调用只和函数名相关和参数无关。调用函数时所有的参数都会被arguments内置对象接收**

**注意4：如果函数没有使用return返回结果，那么默认是返回undefined**

## 5.JavaScript常用对象

* 定义数组的4种方式

```javascript
var arr=[];  //创建一个长度为0的数组 var arr=[1,"hello",true,null]
var arr1=new Array();  //创建一个长度为0的数组
var arr2=new Array(size);   //创建一个长度为size大小的数组，size为数字
var arr3=new Array(值1,值2,....,值n);   //创建数组，并初始化
```

**数组的特点：数组的长度是可变的，可以存储任意类型的数据；**

* 正则对象

```javascript
1.创建正则对象
    var regExp=new RegExp("正则表达式字符串");  //不推荐使用
    //直接量方式
    var regExp=/正则表达式内容/    //不要在正则表达式外面使用引号
    例如：var regExp=/^\w+([-+.]\w+)*@\w+([-.]\w+)*\.\w+([-.]\w+)*$/;
    说明：^表示以xxx开头，$表示以xxx结尾
2.进行校验
调用正则对象的test("要校验的字符串");
例如：var flag = regExp.test("zhang@drfghjk");

```

* Global对象(全局对象)：这个对象的方法不需要使用对象调用，直接写方法就行

```javascript
1.编码和解码
	编码：
    	encodeURI():对于特殊符号：/ = ? 不进行编码，只对中文编码
		encodeURIComponent():对于特殊符号: / = ?也进行编码 
    解码：
    decodeURI()：
    decodeURIComponent()： (了解)
    URI：所有的路径都可以叫做URI，比如：文件的磁盘路径，网络地址路径，邮箱路径...
    URL:特指网络地址路径，地址特点是以http://开头或者以https://
2.字符串类型的数字转number类型的数字
	parseInt
    转换原则：从第一个字符开始转，直到碰到无法转换的字符就结束转换。例如："123ab1"-->123
3.isNaN():判断一个数是不是NaN
4.eval(字符串):将字符串当做JavaScript脚本代码执行
```

## 6.补充

* JavaScript中的输出方式

```javascript
1.弹窗警告
	alert("警告信息");
2.使用document对象写到浏览器页面上
	document.write("html代码");
3.在浏览器控制台输出，f12
	console.info("输出内容");console.log("输出内容");
```

# 二、JavaScript高级

## 1.DOM和事件的简单学习

* **DOM操作的基本思路**

```javascript
DOM操作：操作html页面的标签
基本思路：
	1.获取标签元素对象：var e1=document.getElementById("标签的id属性值");
	2.操作属性或者内容体
    	操作属性：元素对象.属性名="值"     var value=元素对象.属性名;(获取属性值)
		操作内容体：元素对象.innerHTML="html代码";  var value = 元素对象.innerHTML;(获取内容体)
例如:
	//1.通过id获取元素对象
	var light = document.getElementById("light");
	//获取原来的src属性值
	alert(light.src);
	//设置新的属性值
	light.src = "img/on.gif";

	//1.获取h1标签对象
	var title = document.getElementById("title");
	alert("我要换内容了")
	//2.修改内容
	title.innerHTML = ='<font color="red">不识妻美刘强东</font>';
	
```

* **事件的两种绑定方式**

```javascript
方式1：DOM绑定
	1.在html标签中添加事件属性，绑定要执行的函数
    	<img id="light" src="img/off.gif" onclick="fun()">
    2.定义一个要执行的函数
        function fun() {
			//单击img图片，该函数就执行；
        };
方式2：事件句柄
	1.获取标签元素对象：var imgEL = document.getElementById("light2");
	2.给元素的事件属性赋值一个匿名函数
    imgEL.onclick=function() {
        //单击img图片，匿名函数就执行
    };
```

**注意：获取标签元素对象之前一定要确保标签先被加载**

* 案例-灯泡开关

```javascript
/*
	知识点：
		1.给图片设置单击事件
		2.dom操作修改img的src属性
	实现思路：
		1.获取img标签元素对象
		2.给img的onclick事件属性赋值一个匿名函数
		3.在匿名函数中通过dom操作修改img的src属性
			定义一个boolean类型的变量表示开和关
			*/
//1.获取img标签元素对象
var img = document.getElementById("light")
//2.给img的onclick事件属性赋值一个匿名函数
var isLight=false;//默认情况下是关的
img.onclick=function() {
    //判断是开还是关
    if(isLight) {
        //当前是开的状态应该换成关
        img.src="img/off.gif"
    } else{
        //当前是关的状态，应该换成开
        img.src="img/on.gif"
    }
    isLight=isLight;//切换开关状态
}
```

## 2.BOM操作

* BOM：浏览器对象模型，将浏览器的各个部分封装成不同对象
* window对象：window窗口对象，使用时window可以省略

```javascript
1.弹框相关的方法：
	alert("警告信息");
	var flag=confirm("你确定要删除吗？");//用户点击"确定"按钮就返回true，点击"取消"按钮就返回false
	var value=prompt("提示要输入的信息","默认值");//返回值就是用户输入的内容，没有就返回默认值
2.定时器
	一次性定时器：
    	var id = setTimeout(参数1,参数2);//设置一次性定时器
            参数1：要执行的JavaScript代码或者函数对象
            参数2：毫秒值,表示多少毫秒之后执行
            返回值：定时器的唯一标识id，用于作为clearTimeout()参数取消定时器；
        clearTimeout(定时器id);//取消一次性定时器
	循环定时器：
    	var id=setInterval(参数1,参数2);//设置循环定时器，参数和返回值和上面一样
		clearInterval(定时器id);//取消循环定时器
	例如：
    	function fun() {//定时器要执行的代码}
        setTimeout("func()",2000);//正确
        setTimeout("func",2000);//错误
        setTimeout(func(),2000);//错误
        setTimeout(func,2000);//正确
```

* 案例-轮播图

```javascript
<body>
    <!--1.在页面上使用img标签展示图片-->
	<img id="img" src="img/banner_1.jpg" width="100%">
    <script>
        //1.设循环定时器 隔3s执行一个函数
        setInterval("fun()",3000)
		//2.定义定时器要执行的函数
		//3.1.获取img标签元素对象
		var img = document.getElementById("img");
		var index=2;
        function fun() {
			//3.在函数中修改img标签的src属性(每次设置不同的属性值)
            //3.2.设置src属性值
            index=index>3?1:index;
            img.src="img/banner_"+index+".jpg";
            index++;
        }
	</script>
</body>
```

* **location对象：地址栏对象**

```JavaScript
1.获取url路径：
	var url = location.href;
2.设置url路径：（很常用），实现页面跳转
	location.href="新的url路径";
3.刷新当前页面：
	location.reload();
4.获取url后面的参数
	var param = location.search.substring(1);//调用search属性之后获取到的是包含?的参数
```

* 案例-定时跳转

``` JavaScript
<script>
    /*
    分析：
    	1.显示页面效果
    	2.倒计时读秒效果实现
    		2.1 定义一个方法，获取span标签，修改span标签体内容，时间--
			2.2 定义一个定时器，1秒执行一次该方法
		3.在方法中判断时间如果<=0，则跳转首页
		*/
    //2.倒计时读秒效果实现
    var second = 5;
	//获取倒计时时间span元素对象
	var time = document.getElementById("time");
	//定义一个方法，获取span标签，修改span标签体内容，时间
    function showTime() {
        second--;
        //判断时间如果为0，则跳转首页
        if(second<=0) {
            //跳转到首页
            location.href="http://www.baidu.com";
            //清除定时器
            clearInterval(timerId)
        }
        time.innerHTML = second+"";
    }
    //设置定时器，1秒执行一次该方法
    var timerId = setInterval("showTime();",1000);
</script>
```

## 3.DOM操作

* 概念：文档对象模型，操作页面html标签
* DOM树：浏览器将html文档加载进内存，形成一颗dom树

![1545468604899](C:\Users\ADMINI~1\AppData\Local\Temp\1545468604899.png)

* document对象-获取Element对象

```javascript
1.document.getElementById() :根据id属性值获取元素对象。id属性值一般唯一
2.document.getElementByTagName():根据元素名称获取元素对象们，返回值是一个数组
3.document.getElementByClassName():根据class属性值获取元素对象们，返回值是一个数组 
4.document.getElementByName():根据name属性值获取元素对象们，返回值是一个数组
```

* document对象-其他方法

![1545469013408](C:\Users\ADMINI~1\AppData\Local\Temp\1545469013408.png)

* Element对象

```javascript
1.element.removeAttribute("属性名")：删除属性
2.element.setAttribute("属性名","属性值")：设置属性
```

* Node对象

```javascript
方法：
	父元素.appendChild(子元素对象):向节点的子节点列表的结尾
    父元素.removeChild(子元素对象)：删除(并返回)当前节点的指定子节点
    replaceChild():用新节点替换一个子节点
属性：
	元素对象.parentNode 返回节点的父节点
```

* <a href="javascript:void(0)" id="del">删除子节点的作用：

  "javascript:"表示后面可以写JavaScript代码，执行的是一个void(0)函数，该函数表示什么都不返回什么都不做，这样就阻止了超链接的href属性页面跳转

* **innerHTML属性**

```javascript
1.获取内容体html代码
	var html=元素对象.innerHTML;
2.设置内容体html代码
	元素对象.innerHTML="";
3.追加内容体
	元素对象.innerHTML+=""
innerTEXT:设置内容体文本，浏览器将设置的内容不会将html代码解析
```

* **操作样式**

```javascript
1.设置少数样式值
	元素对象.style.fontSize="20px";
	元素对象.style.color="red";
2.设置大量样式值
	1.定义css样式，使用类选择器，例如.c1{……}
	2.使用代码给元素添加class属性值：元素对象.className="c1";
```

## 4.事件

```javascript
1.单击事件：onclick
    元素对象.onclick=function(){
        //单击之后要执行的代码
    }
2.失去焦点事件：onblur
	方式1：<input name="username" id="username" onblur="fun();">
     方式2：元素对象.blur=function() {
        //失去焦点要执行的代码;
    }
3.页面加载完成事件：onload
	方式1：<body onload="fun()"></body>
	方式2：window.onload=function() {
    //页面加载完成之后要执行的代码;
}
4.表单提交事件:onsubmit
	方式1：DOM绑定
    	<form action="" method="" onsubmit="return fun()">
            其他表单项...
            <input type="submit" value="提交">
        </form>
        function fun() {
            //表单校验的代码
            return true/false;//返回true表示运行提交，返回false表示阻止提交，默认运行提交
        }
	方式2：事件句柄
        form元素对象.onsubmit=function() {
            //表单校验的代码
            return true/false; ///返回true表示运行提交，返回false表示阻止提交，默认运行提交
        }
    方式3：
    	<form action="#" id="form">
            其他表单项...
            <input type="submit" value="提交" onclick="return fun();">
        </form>
        function fun() {
            //表单校验的代码
            return true/false;/返回true表示运行提交，返回false表示阻止提交，默认运行提交
        }
5.内容改变事件：onchange,一般用于下拉列表内容改变
        元素对象.onchange=function() {
            //内容改变之后要执行的代码
        }
```





