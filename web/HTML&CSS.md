# HTML

## 1.HTML基本概念语法

* 基本概念：超文本标记语言，主要是用来网页设计；
* 基本语法

```html
1.标签分类：单标签和双标签
	双标签：<开始标签></结束标签>，例如<html></html>
	单标签：<br/>或<br> 表示换行
2.标签嵌套：只能包裹嵌套，不能交叉嵌套
	包裹嵌套：<html><head></head></html>  正确写法
	交叉嵌套：<html><head></html></head>  错误写法
3.标签的组成：<标签名 属性名="属性值" 属性名="属性值" ...></标签名>
```

## 2.html标签-文件标签

> 打开离线手册-->左侧"HTML"-->左下方"HTML 标签列表" -->标签功能(功能排序)

## 3.html标签-文本标签

```html
1.注释：<!--注释内容 --> 快捷键：ctrl+/或者ctrl+shift+/
2.标题标签：h1~h6 
	align属性：表示水平对齐方式，取值有：left,center,right
3.段落标签：<p align="right">自带换行和空行效果</p>
4.换行标签：<br>
5.水平分割线：<hr align="center" width="500" size="50" color="red">
	width:宽度，取值有像素px或者百分比
6.加粗标签：<b></b>
7.斜体标签：<i></i>
8.下划线标签：<u></u>
9.字体标签：<font color="red" size="5"></font>
	size:字体大小，取值范围1~7，超出了范围就等于最大或者最小值
	color:颜色：可以使用英文单词也可以使用16进制数，例如：red、green、#ff0000，如果16进制数两两相等，那么可以简写成#f00
10.居中标签：<center>内容</center>
```

## 4.html标签-图片标签

* ##### img标签的src路径问题

```html
<img src="图片路径" width="" height="" alt="代替图片的文字" align="left">
	align属性：图片与周围内容的对齐方式，顶部对齐、底部对齐、居左、居右
内网路径：自己项目中的图片路径
	绝对路径：带盘符的路径，例如：C:\workspace-idea\2_html\day06_html\img\cs10003.jpg
	相对路径：相对当前html文件所在目录(路径)，相对路径以./或者../开头，./表示当前路径，../表示上一级路径
	<img src="./img/cs10003.jpg">当前html文件所在目录中有一个img目录； ./可以省略
	<img src="../img/cs10003.jpg">当前html文件的父目录中有一个img目录
```

## 5.html标签-列表标签

```html
有序列表：<ol></ol>   order list
	<ol type="序号" start="起始值">  <!-- type取值：1,A,a,I,i-->
        <li>列表项</li>
		<li>列表项</li>
</ol>
无序列表：<ul></ul>   unorder list
	<ul type="列表项前面的符号"> <!-- type取值：disc,square,circle-->
      <li>列表项</li>
      <li>列表项</li>
</ul>
```

##### 注意：li列表项中可以放任何东西，包括文本内容、图片、列表...

## 6.html标签-链接标签

```html
<a href="要跳转的路径" target="">超链接内容体</a>
target属性：
	_self:在当前窗口打开新页面，默认值；
	_blank:在新的窗口打开新页面
<a href="3_文本案例.html"><h1>3_文本案例.html</h1></a><br>
<a href="3_文本案例.html"><img src="img/logo2.png"></a><br>
<a href="img/cs10003.jpg"><img src="img/cs10003.jpg" width="100" height="100"></a>
```

##### 注意：href路径问题和img标签的src路径写法一样，超链接标签的内容体可以是任意的东西包括文本内容、图片

## 7.html标签-块标签

```html
HTML 块元素：能够自动换行的元素就是块元素：h1~h6、p、ul、div...
HTML 内联元素：也叫行元素，默认不能自动换行的元素就是行元素，i、u、b、font、img...
```

## 8.html标签-表格标签

* ##### 定义表格

```html
<!--border:边框的粗细；width:表格宽度；align:水平对齐方式；cellspacing:单元格之间的间隙；cellpadding:单元格边框和内容之间的间隙；bgcolor:背景颜色-->
<table border="1" width="300" align="center" cellspacing="0" bgcolor="red">
    <tr>
        <th></th>
        <th></th>
        <th></th>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td></td>
    </tr>
</table>
td和th的区别：th通常用来表示表头，th中的内容默认加粗居中，td中的内容没有任何特殊效果
```

* ##### 合并单元格思路：操作的是td标签

```html
1.确定是合并行还是合并列，确定合并几个单元格
2.在合并的第一个单元格td上添加colspan(合并列)或者rowspan(合并行)属性，合并几个单元格属性值就是ji
3.将被合并的单元格删掉
```

##### 注意：一般情况下table-->tr--th/td,th/td中可以放任何东西

## 9.html标签-表单标签

* form标签：定义数据采集的范围，只有在form标签内部的表单项才会被提交给服务器；

```html
<form action="数据提交的路径" method="提交方式：get/post"> <!-- 默认是get提交方式-->
    表单项...
</form>
action:数据提交的路径也就是数据要提交到哪个服务器程序
method:提交方式，提交方式有7种，但是常用的只有get和post，区别是：
	1.get提交方式请求参数在地址栏url后面，post请求参数不在地址栏
	2.get提交方式请求参数大小有限制，post请求无限制，所以文件上传使用post请求
	3.get提交方式相对不安全，post请求相对安全一些
get提交方式参数列表：url?name=value&name=value&.... 例如：url?username=zhang&password=123
```

* ##### 表单项name属性和value属性的作用

```html
name属性的作用：
	1.表单项数据要想提交，必须要有name属性；
	2.对于radio和checkbox而言，name属性将选项分为一组，同组单选按钮有互斥效果，同一组复选框一起提交
value属性的作用：
	1.对于普通input输入框而言，value是默认值。不是提示信息，提示信息使用placeholder
	2.对于radio和checkbox而言，value属性值用于区分提交的选中项（必须要有）
```

##### 注意：所有的表单项都需要name属性，只能选不能输入的表单项必须使用value属性

* 案例

```html
<!-- 定义表单 from-->
<form action="#" method="post">
    <table border="1" align="center" width="500">
        <tr>
        	<td><lable for="username">用户名</lable></td>
            <td><input type="text" name="username" id="username" placeholder="请输入用户名"></td>
        </tr>
        <tr>
        	<td><lable for="password">密码</lable></td>
            <td><input type="paaword" name="password" id="password" placeholder="请输入密码"></td>
        </tr>
        <tr>
        	<td><lable for="email">Email</lable></td>
            <td><input type="email" name="email" id="email"></td>
        </tr>
        <tr>
        	<td><lable for="name">姓名</lable></td>
        	<td><input type="text" name="name" id="name"></lable></td>
        </tr>
		<tr>
			<td><label for="tel">手机号</label></td>
			<td><input type="text" name="tel" id="tel"></td>
		</tr>
		<tr>
			<td><label>性别</label></td>
			<td><input type="radio" name="gender" value="male" checked> 男
			<input type="radio" name="gender" value="female"> 女
			</td>
		</tr>
		<tr>
			<td><label for="birthday">出生日期</label></td>
			<td><input type="date" name="birthday" id="birthday"></td>
		</tr>
		<tr>
			<td><label for="checkcode">验证码</label></td>
			<td><input type="text" name="checkcode" id="checkcode">
			<img src="img/verify_code.jpg">
			</td>
		</tr>
		<tr>
            <td colspan="2" align="center"><input type="submit" value="注册"></td>
		</tr>
    </table>
</form>
```

## 10.CSS引入方式以及语法

* CSS概念：层叠样式表，多个样式可以共同作用在一个标签元素上；CSS主要用于美化页面
* CSS引入方式

```css
1.内联样式/行内样式：将样式代码写到html标签style属性中
	例如：<div style="color:red;font-size:160px">hello css</div>
2.页内样式：将样式代码写到一组style标签中，style标签在head标签中
	例如：
		<style>
			div{
    			color:blue;
				font-size: 30px;
			}
		</style>
3.外部样式：
	第一步：定义一个以.css结尾的文件，在该文件中书写样式代码，样式代码中不包含style标签
	第二步：在html中的head标签中通过link标签引入外部样式
		例如：<link rel="stylesheet" href="css/a.css">
```

**注意：link标签必须要有rel="stylesheet"属性，否则引入的样式无序；快捷键：写link然后ctrl+alt+空格提示**

* CSS基本语法

```css
选择器{
    属性名：属性值;
    ...
}
选择器：筛选哪些标签被样式作用
```

### CSS常用选择器

* **基础选择器**

```css
1.id选择器：选择具体的id属性值的元素，建议在一个html页面中id值唯一
    语法:#id属性值{}
2.元素选择器：选择具有相同标签名称的元素
	语法:#标签名称{}
3.类选择器：选择具有相同的class属性值的元素
	语法:.class属性值{}
```

**注意优先级关系：id选择器>类选择器>元素选择器**

* 扩展选择器

```css
1.通配符选择器：所有标签元素都会被选择
	语法：*{样式代码}  例如：*{color:blueviolet}
2.组合选择器/并集选择器：将多个选择器使用逗号并列到一起作用
    语法：选择器1,选择器2,...{样式代码}
    例如：
        h2,.cDiv{
            color:blue;
            font-size:50px;
        }
3.属性选择器：筛选具有相同属性的元素
    语法：选择器[属性名]{样式代码}   选择器[属性名="属性值"]{样式代码}
    例如：
    /*input[type]{
        color:red;
    }*/
    input[type="text"]{
        color:red;
    }
4.后代选择器：筛选某些元素的直接或者间接子元素(筛选出直接或者间接的孩子)
	语法：选择器1 选择器2{样式代码}
	例如：
        div p{
            color:green;
            font-size:50px;	
        }
5.子元素选择器：筛选某些元素的直接子元素
    语法：选择器1>选择器2{样式代码}
    例如：
        div>p{
            color:green;
            font-size:50px;	
        }
6.伪类选择器：表示标签的不同状态，常用于超链接的不同状态
    a:link {color: #FF0000} /* 未访问的链接 */
    a:visited {color: #00FF00} /* 已访问的链接 */
    a:hover {color: #FF00FF} /* 鼠标移动到链接上 */
    a:active {color: #0000FF} /* 选定的链接 */
7.过滤选择器：过滤第几个子元素
    语法：选择器1 选择器2:first-child{样式代码} 表示找到第一个孩子
    语法：选择器1 选择器2:last-child{样式代码} 表示找到最后一个孩子
    语法：选择器1>选择器2:first-child{样式代码} 表示找到第一个孩子
    语法：选择器1>选择器2:last-child{样式代码} 表示找到最后一个孩子
        例如：
        div>p:first-child{
        color: green;
        font-size: 50px;
        }
        div>p:last-child{
        color: green;
        font-size: 50px;
        ｝
```

### CSS常用属性

```css
1.字体、文本
    font-size:字体大小
    color:文本颜色
    text-align:水平对齐方式
    line-height:行高，可以用来使文本垂直居中（使其值都等于height）
    去掉下划线:text-decoration:none
2.背景
	background：url("img/logo.") no-repeat center;
3.边框
	border:设置边框，复合属性 常用用法：border:1px solid red; border-left:1px solid red;
	border-radius:5px;设置圆角
4.尺寸
	width:宽度
	height:高度
5.盒子模型：控制布局
	margin:外边距，可以设置4个值，分别代表上、右、下、左；如果设置1个值，那么代表4个外边距；margin可
以设置负值
	padding:内边距，不能为负值
	默认情况下内边距会影响整个盒子的大小
	box-sizing:border-box;设置盒子的属性。让width和height就是最终盒子的大小
	水平居中：margin：auto；
	垂直居中：vertical-align:middle;
6.浮动：作用：让块级元素共占一行；几个元素共占一行就给这几个元素设置浮动
	float:浮动 块级元素默认是独占一行的，如果想让块级元素共占一行，那么就使用浮动属性
	float:left
	float:right
```

