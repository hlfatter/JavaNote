# BootStrap

## 1.准备模板

* 响应式布局：同一套布局兼容不同分辨率设备；
* 使用思路--基本模板

```html
1.将资源文件拷贝到项目中：css目录、fonts目录、js目录、jquery-3.2.1.min.js也可以放到js目录中；
2.参考文档，复制模板代码，修改引入的资源路径
<!DOCTYPE html>
<html lang="zh-CN">
    <head>
        <meta charset="utf-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <!-- 上述3个meta标签必须放在最前面，任何其他内容都必须跟随其后
		<title>BootStrap 101 Template</title>
		<!-- BootStrap -->
        <link href="css/bootstrap.min.css" rel="stylesheet">
        <!-- jQuery(necessary for BootStrap's JavaScript plugins) -->
        <script src="js/jquery-3.2.1.min.js"></script>
        <!-- Include all compiled plugins (below), or include individual files as needed -->
        <script src="js/bootstrap.min.js"></script>
    </head>
    <body>
        <h1>
            你好，世界！
        </h1>
    </body>
</html>
```

## 2.栅格系统的使用和注意事项

* 使用的基本思路

```html
1.定义容器：相当于之前定义table，例如： <div class="container-fluid"></div>
	取值：class="container-fluid":两边没有留白
		class="container":居中，两边有留白
2.在容器定义行：相当于之前在表格中定义tr，例如：<div class="row"></div>
3.在行中定义列：在行中定义元素指定占的列数；class="col-设备代号-格子数量"；例如：<div class="col-lg-1">栅格</div>，一行只有12个格子
	设备代号取值：
		xs:超小屏幕 手机（小于768px）
		sm:小屏幕 平板（>=768px）
		md:中等屏幕 桌面显示器（>=992px）
		lg:大屏幕 大桌面显示器（>=1200px）
<!--1.定义容器-->
<div class="contain-fluid">
    <!--2.定义行-->
    <div class="row">
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
        <div class="col-lg-1 col-sm-2">栅格</div>
    </div>
</div>
```

* 注意事项

```html
1.一行中如果格子数目超过12，则超出部门自动换行
2.栅格类属性可以向上兼容。栅格类适用于与屏幕宽度大于或等于分界点大小的设备
3.如果真实设备宽度小于了设置栅格类属性的设备代码的最小值，会一个元素占满一整行
4.栅格系统允许兼容，嵌套原则：container->row->col->[container]->row->col->...  [container]表示可选
```

## 3.全局CSS样式、组件、插件

```css
1.全局CSS样式：
	* 按钮：class="btn btn-default"
	* 图片：
		* class="img-responsive"：图片在任意尺寸都占100%
		* 图片形状
			* <img src="..." alt="..." class="img-rounded"> :方形
			* <img src="..." alt="..." class="img-circle">  :圆形
			* <img src="..." alt="..." class="img-thumbnail"> :相框
		* 表格
			* table
			* table-bordered
			* table-hover
		* 表单
			* 给表单项添加：class="form-control"
2.组件
	* 导航条
	* 分页条
3.插件：
	轮播图
```

## 4.补充：JavaScript中自定义对象

* 方式1：构造函数

```javascript
//1.声明构造函数
function Person(name, age) {
    this.name=name;// this表示创建的Person对象
    this.age=age;
    this.study=function() {
        //必须使用匿名函数
        alert(name+"学习java");
    }
}
//2.使用new关键字创建对象
var p = new Person("张", "20");
console.log(p.name+"-----"+p.age);
//支持对象创建完成之后再添加属性
p.sex="男";
console.info(p.name+"-----"+p.age+"----"+p.sex);
//调用方法
p.study();
```

* **方法2：直接量方法，对象直接创建并初始化**

```javascript
语法：var 对象名=｛属性名：属性值,属性名：属性值,....｝;
//定义person对象
var person={
    name:"张萌",
    age:21,
    study:function(name) {
        alert(name+"学习java");
    }
};
//获取属性值
console.info(person.name+"----"+"person.age");
//支持对象创建完成之后再添加属性
person.sex="男";
conxole.info(person.name+"-----"+person.age+"---"+person.sex);
//调用方法
person.study(person.name);
```

# ECMAScript6新特性

## 1.let和const关键字

```javascript
let和const关键字
	let关键字是用来声明变量的，const关键字是用来声明常量的，常量一旦声明就不能改变；在es5.1及以前是使用var关键字来声明变量
变量的作用域也叫作用范围
	在es5.1中变量的作用域只用 全局作用域和函数作用域
    在es6中变量的作用域有 全局作用域、函数作用域和块级作用域
    
let和var的区别：
	1.var声明的变量只有全局作用域和函数作用域，而let声明的变量可以有全局作用域、函数作用域和块级作用域
    2.var声明的变量会提升到全局或者函数的最前端，就会导致var声明的变量在还没有声明之前就可以用，不严谨；
而let声明的变量不会提升到最顶端，必须先声明再能使用
	3.在for循环中推荐使用let关键字
```

## 2.模板字符串

```javascript
也叫高级字符串，普通字符串能使用的方法模板字符串都可以用；模板字符串使用反引号``;
与普通字符串的区别：
	普通字符串中如果使用变量，那么需要使用字符串拼接，模板字符串不需要拼接，使用${}
		表达式：可以是变量、对象、执行运算、执行函数；
```

## 3.es6函数

```javascript
...表示扩展运算符，可以使用在函数的形参中，还可以用于变量的解构赋值
例如：
function add(...params) {
    //可变参数
    let sum=0;
    for(let i=0; i<params.length; i++) {
        sum+=params[i];
    }
    return sum;
}
//调用函数
let sum=add(1,2,3,4,5,6,7,8,9);
console.log(`sum=${sum}`);

```

* 箭头函数：类似于java中的lambda表达式

``` java
lambda表达式的语法：(参数列表)->{
    //方法体
}
箭头函数的语法：(参数列表)=>{
    //方法体
}
特点：
	1.如果只有一个参数，那么()可以省略;
	2.如果方法体只有一行代码，那么()、return可以省略
例如：
let add=(...params)=>{
	let sum=0;
    for(let i=0; i<params.length; i++) {
    	sum+=params[i];
    }
    return sum;
}
//调用函数
let sum=add(1,2,3,4,5,6,7,8,9);
console.log(`sum=${sum}`);
let fun=m=>alert(`2s之后执行该函数，传递进来的参数m=${m}｝`);
let add=(m,n)=>m+n;
//setTimeout("fun(100)",2000);
//fun(1000);
console.info(`结果是：${add(10,20)}`)
```

## 4.变量的解构赋值

```html
1.解构数组：将数组中的元素按照一定的模板快速的赋值个某些变量
2.解析对象：将对象中的属性按照一定的模板快速的赋值个某些变量
例如：
	/*1.解构数组：将数组中的属性按照一定的模板快速的赋值个某些变量*/
		let arr1=["hello","word",100,true];
		let arrr2=[false];
        /*let s1=arr1[0];
        let s2=arr1[1];
        let m=arr1[2];
        let f=arr1[3];*/
        /*let [s1,s2,m,f]=arr1;
        console.info(`s1=${s1}---s2=${s2}----m=${m}----f=${f}`);*/
        /* let [s1,,m,]=arr1;
        console.info(`s1=${s1}---m=${m}`);*/
        //使用扩展预算法
        /* let arr=[arr1,arr2]
        console.info(arr);*/
        //将“hello” 赋值个一个变量，将后面三个字封装到一个新数组中
        let [s1,...arr]=arr1;
        console.info(s1);
        console.info(arr);
        /* 2.解构对象：将对象中的属性按照一定的模板快速的赋值个某些变量*/
        var person={
        name:"张威",
        age:18
        }
        let {name,age}=person;
        console.info(`name=${name}---age=${age}`);
```

## 5.for...of遍历：增强for循环

```javascript
语法：快捷键  iter
        for(let 遍历名 of 数组对象) {
			//		循环体
        }
例如：
	let arr=["hello","word",100,true];
	for(let value of arr) {
			console.info(`value=${value}`)
        }
```

## 6.set和map集合

```javascript
1.set集合：单列集合，存储的数据不允许重复，和java中HashSet集合类似
2.map集合：双列集合 和java中的HashMap集合类似
例如：
	/* 1.set集合：单列集合，存储的数据不允许重复，和java中的HashSet集合类似*/
        //1.定义set集合
        // let set = new Set();
        let arr=["hello","world",100,true,false,100];
        let set=new Set(arr)
        //2.存值
        /*set.add("hello");
        set.add("world");
        set.add(100);
        set.add(true);
        set.add(false);*/
        //3,遍历取值,增强for循环
        for (let value of set) {
        console.info(value);
        }
        /*2.map集合：双列集合 和java中的HashMap集合类似*/
        //1.定义map集合
        var map = new Map();
        //2.存值
        map.set("name","张威");
        map.set("age",20);
        map.set("sex","女");
        //3.遍历取值
        //3.1 类似于java中的keySet遍历
        /*var keys = map.keys();
        for (let key of keys) {
        console.info(`${key}=${map.get(key)}`)
        }*/
        //3.2 类似于java中的entrySet方法
        var entries = map.entries();
        /*for (let entry of entries) {//entry是一个数组对象，使用索引取值
        //entry是包含key和value的键值对对象
        console.info(`${entry[0]}=${entry[1]}`)
        }*/
        //遍历出来的数组entry可以解构
        for (let [key,value] of entries) {
        //entry是包含key和value的键值对对象
        console.info(`${key}----${value}`)
        }
```



