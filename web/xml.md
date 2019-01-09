# XML

## 1.XML基础语法

* 概念：可扩展的标记语言，即可以自定义标签
* 作用：存储是数据

```xml
1.作为配置文件使用，例如c3p0-config.xml配置文件；将来框架中还使用注解代替配置文件，但是往往注解和配置文件同时支持
2.作为网络传输的数据格式；在开发中通常使用json数据格式代替xml
```

* 入门案例

```xml
<?xml verrsion='1.0' ?>
<users>
	<user id='1'>
        <name>zhangsan</name>
        <age>23</age>
        <gender>male</gender>
        <br/>
    </user>
    <user id='2'>
        <name>lisi</name>
        <age>24</age>
        <gender>female</gender>
        <br/>
    </user>
</users>
```

* 语法：

```xml
1. xml文档的后缀名 .xml
2. xml第一行必须定义为文档声明
3. xml文档中有且仅有一个根标签
4. 属性值必须使用引号引起来
5. 标签必须正确关闭
6. xml标签名称区分大小写
```

* xml的组成部分--文档声明

```xml
<?xml version="1.0" encoding="UTF-8" ?>
文档声明中：version和encoding属性必须使用
```

## 2.XML的约束

* dtd约束

```xml
1.定义dtd约束
	<!ELEMENT students (student+) >
	<!ELEMENT student (name,age,sex)>
	<!ELEMENT name (#PCDATA)>
	<!ELEMENT age (#PCDATA)>
	<!ELEMENT sex (#PCDATA)>
	<!ATTLIST student number ID #REQUIRED>
2.在xml中引入dtd约束；<!DOCTYPE xml根标签 SYSTEM "dtd约束文件相对路径">
	<?xml version='1.0' encoding='UTF-8' ?>
	<!DOCTYPE students SYSTEM "student.dtd">
	<student>
		<student number="s001">
        	<name>zhangsan</name>
            <age>20</age>
            <sex>male</sex>
        </student>
        <student number="s002">
        	<name>lisi</name>
            <age>24</age>
            <sex>female</sex>
        </student>
</student>
```

* schema约束

```xml
1.编写schema约束
2.在xml中引入schema约束
<?xml version="1.0" encoding="UTF-8" ?>
<!--
	1.填写xml文档的根元素
	2.引入xsi前缀  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	3.引入xsd文件命名空间  xsi:schemaLocation="http://www.itcast.cn/xml student.xsd"
	4.为每一个xsd约束声明一个前缀作为标识 xmlns="http://www.itcast.cn/xml"

	说明：
		xsi:schemaLocation="名称空间 schema约束的相对路径"；名称空间需要与schema约束文件中定义的targetNamespace="名称空间"一致
 -->
<students xmlns="http://www.itcast.cn/xml"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          xsi:schemaLocation="http://www.itcast.cn/xml student.xsd">
	<student number="heima_0001">
    	<name>zhangsan</name>
        <age>20</age>
        <sex>male</sex>
    </student>
</students>

```

## 3.jsoup解析xml文件

* dom解析和sax解析的区别

```xml
1.dom解析一次性将文档加载进内存，形成一颗dom树；sax解析是基于事件将文档一行一行的加载进内存，读取到一行就触发一个事件
2.dom解析占内存加大，sax解析完一行就释放一行资源，节省了内存消耗
3.dom解析可以执行增删改查任意操作，sax只能读取文档中的数据
```

* jsoup解析的思路

```xml
1.将xml文件加载进内存解析，得到一个Document对象
2.通过Document对象获取Element对象
3.操作Element对象内容体或者属性
例如：
public class JsoupDemo{
    public static void main(String[] args) throws IOException{
		//1.将xml文件加载进内存解析，得到一个Document对象
		//1.1.通过类加载器获取src中文件路径
		String path = JsoupDemo.class.getClassLoader().getResource("b.xml").getPath();
		//1.2.调用Jsoup的静态parse方法解析
		Document document = Jsoup.parse(new File(path),"utf-8");
		//2.通过Document对象获取Element对象
		Elements elements = document.getElementsByTag("name");
		//3.操作Element对象内容体或者属性
		String name = elements.get(0).text();
		System.out.println("name="+name);
    }
}
```

* Document对象--通过css选择器获取元素对象

```xml
1.获取一个元素：
	Element element=document.selectFirst("css选择器");
	Element element=document.selectFirst("users user:nth-child(2)");//序号从1开始
2.获取多个元素：
	Elements elements=document.select("css选择器);
```

* Element对象

```xml
1.操作标签名：
	获取标签名：String tagName=element.tagName();
	设置标签名：element.tagName("新标签名");
2.操作内容体：
	获取内容体：
		String text = element.text();//获取纯文本的字符串，不带标签
		String html = element.html();//获取标签内容体html代码，可能带标签
	设置内容体：
		element.text("新内容字符串");
		element.html("新内容html代码，可以包含标签");
3.操作属性：
	获取属性值：String 属性值=element.attr("属性名");
	设置属性：element.attr("属性名","属性值");
4.获取子元素
	Elements children = element.children();//获取的是标签元素对象
5.获取父元素
	Element parent = element.attr("属性名");
6.对元素进行增删改
	增：父元素.append("<level>王者级</level>");追加一个子元素
	删：element.remove();element表示要删除的元素
	改：设置标签名：element.tagname("新标签名");
7.将内存中的数据写到硬盘中
	private static void write2Disk(Document document,String path) throws IOException{
	System.out.println(path);
	//1.创建流对象 高效的字符流
	BufferedWrite bw = new BufferedWrite(new FileWrite(path));
	//2.写数据
	//2.1写文档声明
	bw.write("<?xml version=\"1.0\>" encoding=\"utf-8\" ?>");
	bw.newLine();
	//2.2获取body标签的内容体，也就是xml的正文
	Element body = document.selectFirst("body");
	bw.write(body.html());
	//释放资源
	bw.close();
}
```

