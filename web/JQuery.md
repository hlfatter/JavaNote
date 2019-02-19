# JQuery基础

## 1.JQuery概念和快速入门

* 概念：一个js框架，封装了原生的js代码。作用：简化javascript代码书写；

`将jquery-3.3.1.min.js复制到项目的js目录中，在html中引入jquery-3.3.1.min。js;`

## 2.JQuery核心函数

### 2.1.JQuery核心函数

* 核心函数1：JQuery(callback)

```html
$(function(){
	//当页面加载完成之后匿名函数会被执行
})
```

* 核心函数2：Query(html/js对象)

```html
1.当传递的是html代码时：创建html代码对应的元素对象
	var div=$("<div><p>Hello</p></div>")
2.当传递的是js对象时，将js对象转换成jquery
	var jqueryObj=$(js对象)
```

* 核心函数3：JQuery(selector)

`根据css选择器查找标签对应的元素对象；例如：$("#div");$("p")`

* 总结：这三个核心函数其实都是同一个函数jquery();也叫$()函数，仅仅只是传递的参数不一样而已，传递的参数不一样作用就不一样

### 2.2.js对象和jquery相互转换

```html
java中String(js对象)和StringBuilder(jquery对象)区别
	String:普通字符串
	StringBuilder:带缓冲区的高级字符串
String和StringBuilder相互转换
	String---->StringBuilder : StringBuilder sb = new StringBuilder(Str);
	StringBuilder----->String : String str = sb.toString();

js对象：之前通过document获取的对象都是js对象，document.getElementByXXXxx(参数)；
jquery对象：通过$("css选择器")或者$(js对象)等获取的对象就是jquery对象；可以将jquery对象看成是js对象的包装对象；但是js对象和jquery对象的属性和方法不能相互混用；但是两者之间可以相互转换

js对象-----> jquery对象： $(js对象)只有是jquery对象才能调用html()方法
jquery对象-----> js对象：jquery对象[0]或者jquery对象get(0),只有是js对象才可以调用innerHTML属性
```

## 3.JQuery事件绑定以及样式控制

### 3.1.事件绑定

```html
1.绑定事件：
    jquery对象.click(function(){
		//当元素被单击之后，匿名函数会执行
    })；
	jquery对象.blur(function(){
		//当元素失去焦点之后，匿名函数会执行
    })；
	jquery对象.submit(function(){
		//当submit按钮被点击之后，匿名函数会执行
		return true/false;//返回true表示允许提交，false表示阻止提交
    })；
2.使用代码触发事件
	$("p").click();
```

### 3.2.样式控制

```html
获取样式值：
	var value=jquery对象.css("样式名);
设置一组样式：
	jquery对象.css("样式名","样式值");例如：$("p").css("color","red");
设置多组样式：将多组样式先封装到js对象中，将js对象传递给jquery的css方法
	jquery对象.css({"样式名":"样式值","样式名":"样式值"})
```

## 4.JQuery选择器

```html
1.基本选择器
	* 标签选择器(元素选择器)
		语法：$("html的标签名") 获取所有匹配标签名称的元素
	* id选择器
		语法：$("#id的属性值") 获得与指定id属性值匹配的元素
	* 类选择器
		语法：$(".class的属性值") 获得与指定的class属性值匹配的元素
	* 并集选择器
		语法：$("选择器1,选择器2...") 获取多个选择器选中的所有元素
2.层级选择器
	* 后代选择器
		语法：$("A B") 选择A元素内部的所有B元素
	* 子选择器
		语法：$("A > B") 选择A元素内部的所有B子元素
3.属性选择器
	* 属性名称选择器
		语法：$("A[属性名]") 包含指定属性的选择器
	* 属性选择器
		语法：$("A[属性名='值']") 包含指定属性等于指定值的选择器
4.过滤选择器
	* 首元素选择器
		语法：	：first 获得选择的元素中的第一个元素
	* 尾元素选择器
		语法：	：last 获得选择对的元素中的最后一个元素
	* 非元素选择器
		语法：	：not(selector) 不包括指定内容的元素
5.表单过滤选择器
	* ：checked选择器
		语法：	：checked 获得单选/复选框选中的元素
	* ：selected选择器
		语法：	：selected 获得下拉框选中的元素
```

## 5.Jquery的DOM操作

### 5.1.内容操作

```html
1.val()方法：获取/设置input输入框的value值
	获取：jquery对象.val();不传参表示获取input输入框的value值
	设置：jquery对象.val("字符串内容");传参表示设置input输入框的value值
2.html()方法：获取/设置标签内容体html代码,和JavaScript中innerHTML属性作用一样
	获取：jquery对象.html(),不传参表示获取标签内容体html代码
	设置：jquery对象.html(),传参表示设置标签内容体html代码
3.text()方法：获取/设置标签内容体文本内容,和JavaScript中innerTEXT属性作用一样
	获取：jquery对象.text(),不传参表示获取标签体中的文本内容
	设置：jquery对象.text("文本内容"),传参表示设置标签体中的文本内容
```

### 5.2.属性操作

```html
1.attr():获取/设置元素属性值
	获取属性值：var 属性值=jquery对象.attr("属性名")
	设置属性值：jquery.attr("属性名","属性值");jquery对象.attr({"属性名":"属性值","属性名":"属性值"})
	删除属性：jquery对象.removeAttr("属性名")
2.prop():获取/设置元素属性值
	获取属性值：var true=jquery对象.prop("checked");//true或者false
	设置属性值：jquery.prop("checked",true)
	删除属性：jquery对象.removeProp("属性名")
3.操作Class属性
	jquery对象.addClass("class属性值"); 添加class属性值
	jquery对象.removeClass("class属性值"); 删除class属性值
	jquery对象.toggleClass("class属性值"); 切换class属性
```

**总结：当操作的是元素的选中状态(checked、selected)时,使用prop方法;操作其他属性都使用attr()方法**

## 5.3.对标签进行CRUD操作

```html
1.添加子元素：
	父元素.append(子元素);将子元素追加到父元素内部末尾;父元素必须是jquery对象
	父元素.append("html");将html代码追加到父元素内部末尾
	子元素.append(父元素)
2.移除元素
	被移除元素.remove()
3.清空子元素
	父元素.empty()
```

## 6.总结

```html
1.jquery核心函数：$(参数) 传递的参数不一样功能就不一样
	参数：匿名函数、js对象/html、css选择器
        $(function(){
			//页面加载完成之后执行匿名函数
        })
	var jqueryObj=$("html"); 创建一个html代码对应的元素对象
	例如：var div=$("<div><p>Hello</p></div>");
	var jqueryObj=$(js对象); 将js对象转换成jquery对象
	var jqueryObj=$("css选择器"); 根据css选择器查找元素
2.js对象和jquery对象相互转换问题
	js对象：之前我们通过document获取到的元素对象
	jquery对象：使用jquery核心函数产生的对象，可以将jquery对象看成是js对象的包装对象，但是js对象和jquery对象各个方法和属性不能混用
	js对象------>jquery对象： var jqueryObj=$(js对象)
	jquery对象----->js对象 ： jquery对象[0]或者jquery对象.get(0)
3.事件绑定
	绑定单击事件
    jquery对象.click(function(){
		//当元素被单击时候，执行匿名函数
    })
4.常用选择器
	元素选择器：将标签名作为选择器名称；$("div")、$("p")
	类选择器：将.class属性值(class="c1")作为选择器名称；$(".c1")
	id选择器：将#id属性值(id="d1")作为选择器名称；$("#d1")
	组合选择器：将多个选择器使用逗号组合到一起选择元素 $("选择器1，选择器2...")
	后代选择器：查找父元素内的所有直接/间接子元素：$("选择器1 选择器2")
	子代选择器：查找父元素内的所有直接子元素
5.jquery的dom操作
	操作内容体：
		html()、text()、val();
	操作属性：
		attr()、removeAttr("属性名")； prop()、removeProp("属性名")
		操作选中状态使用prop()方法，操作其他属性使用attr()；
	对元素CRUD操作：
		添加元素：父元素.append(子元素/html);
		移除元素：被移除元素.remove();
		清空子元素：父元素.empty();
```

# JQuery高级

## 1.Jquery动画

![1548209940248](C:\Users\ADMINI~1\AppData\Local\Temp\1548209940248.png)

## 2.JQuery遍历（3种方式）

* **for方式遍历 - javascript中的遍历方式**

```html
默认快捷键： itar+回车
```

* **each()函数遍历 - jquery特有的方式**

```html
//方式1
jquery数组对象.each(function(index,element){			//index,element都是可写可不写
    //每遍历一次，匿名函数就执行一次
    //index：每次遍历的索引
    //element：每次遍历出来的元素对象，是一个js对象，等同于this
    //this：每次遍历出来的元素对象，是一个js对象
	return true/false;//返回true相当于continue，返回false相当于break；也可以不写return表示所有元素都会被遍历到
})

//方式2：很少用
$.each(数组对象.function(index,element){
	//index,element都是可写可不写
    //每遍历一次，匿名函数就执行一次
    //index：每次遍历的索引
    //element：每次遍历出来的元素对象，是一个js对象，等同于this
    //this：每次遍历出来的元素对象，是一个js对象
	return true/false;//返回true相当于continue，返回false相当于break；也可以不写return表示所有元素都会被遍历到
})
```

* **增强for循环也叫for of方式 --ECMAScript6新特性**

```html
快捷键：iter+回车
```

## 3.JQuery事件绑定

* **标准方式**

```html
1.绑定事件：
    jquery对象.click(function(){
        //当元素被单击之后，匿名函数会执行
    })
    jquery对象.blur(function()}{
        //当元素失去焦点之后，匿名函数会执行
    })
    jquery对象.submit(function(){
        //当submit按钮被点击之后，匿名函数会执行
        return true/false;//返回true表示允许提交，false表示阻止提交
    });
2.使用代码触发事件
	$("p").click();

```

* on/off绑定和解绑事件

```html
//点击按钮2给按钮1绑定一个单击事件
$("#btn2").click(function () {
    //先解绑之前的单击事件
    $("#btn1").off("click");
    //给按钮1绑定一个单击事件
    $("#btn1").on("click",function () {
    alert("给按钮1绑定一个单击事件");
    });
})
作用：给按钮1永远只绑定一个事件，jquery中允许给元素多次绑定事件；
```



