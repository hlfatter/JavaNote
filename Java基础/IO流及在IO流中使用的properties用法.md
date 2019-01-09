# IO流与properties用法

## IO流

### IO流的分类

​	根据数据的流向分为：**输入流和输出流**

* **输入流**：把数据从`其他设备`上读取到`内存`中的流

* **输出流**：把数据从`内存`上读取到`其他设备`上的流

  根据数据的类型分为：**字节流和字符流**

* **字节流**：以字节为单位，读写数据的流

  ​	读写字节数据（任何数据都是字节、文本、图片、视频、音频...）

  ​	`InputStream(字节输入流)`

  ​		--`FileInputStream(读取字节)`

  ​	`OutputStream(字节输出流)`

  ​		--`FileOutputStream(写入字节)`

* **字符流**：以字符为单位，读写数据的流

  ​	`Reader:字符输入流`

  ​		--`FileReader(读字符)`

  ​	`Writer:字符输出流`

  ​		--`FileWriter(写字符)`

#### IO流读写数据的步骤

 	1. 创建流对象（搭桥）
 	2. 读写数据（过桥）
 	3. 释放资源（拆桥）

#### 字节输出流（OutputStream）

​	字节输出流的基本共性功能方法：

* `public void close()`:关闭此输出流并释放与此流相关联的任何系统资源
* `public void flush()`:刷新此输出流并强制任何缓冲的输出字节被写出
* `public void write(byte[] b)`:将b.length字节从指定的字节数组写入此输出流
* `public void write(byte[] b, int off, int len)`:从指定的字节数组写入len字节，从偏移量off开始输出到此输出流
* `public abstract void write(int b)`:将指定的字节输出流

#### FileOutputStream类

##### 构造方法

* `public FileOutputStream(File file)`:创建文件输出流以写入由指定的File对象表示的文件

* `public FileOutputStream(String name)`:创建文件输出流以指定的名称写入文件

  当创建一个流对象时，必须传入一个文件路径。该路径下，如果没有这个文件，会创建文件。如果有，则会清空这个文件的数据

```java
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
/*【数据追加续写】
FileOutputStream(pathname):再次运行新内容会覆盖原数据
* FileOutputStream(pathname, boolean append):如果令boolean append为true则会在原数据后面进行续写
* 如果为false则会覆盖原数组。而FileOutputStream(pathname)就相当于为false的情形
* */

public class Demo01 {
    public static void main(String[] args) throws IOException {
        FileOutputStream fos = new FileOutputStream("day09-code\\a.txt");
        fos.write(97);
        byte[] b = {97, 98, 99};
        fos.write(b);
        fos.write("\r\n\t".getBytes());//"\r\n"代表换行，需要先把字符串转换成字节再写入文件
        fos.write("19960708".getBytes());//把字符串转换成字节再写到文件中调用String中的getBytes()方法
        fos.write("\r\n\t".getBytes());
        fos.write(b, 0,1);//把字节数组b中从0索引开始写入长度为1的字节到文件中
        fos.close();//释放资源

    }
}
```

#### 字节输入流【InputStream】

* `public void close()`:关闭此输入流并释放与此流相关联的任何系统资源
* `public abstract int read()`:从输入流读取数据的下一个字节
* `public int read(byte[] b)`:从输入流中读取一些字节数，并将它们存储到字节数组b中

#### FileInputStream类

##### 构造方法

* `FileInputStream(File file)`:通过打开与实际文件的连接来创建一个FileInoutStream，该文件由文件系统中的File对象file命名

* `FileInputStream(String name)`:通过打开与实际文件的连接来创建一个FileInputStream，该文件由文件系统中的路径名name命名

  字节流文件复制【很重要】

```java
//创建流对象
FileInputStream fis= new FileInputStream("day09-code\\a.txt");
FileOutputStream fos = new FileOutputStream("day09-code\\b.txt");
//一边读一边写
byte[] bs = new byte[1024];
int len;//记录每次读取的有效个数
while((len = fis.read(bs)) != -1) {
    //每次读取的数据，会保存到bs数组中
    //从bs数中，写入有效个数从0开始，写len个
    fos.write(bs, 0 , len);
}
//释放资源
fos.close();
fis.close();
```

​	字节流读取中文（乱码）的问题

​		中文在不同的编码表中占用的字节数是不一样的

​			GBK编码表： 汉字占2个字节

​			UTF-8编码表： 汉字占3个字节

​		中文乱码问题：由于一个文本文件中可能出现汉字，也有可能出现字母或者标点符号，他们占用的字节数不一样。采用一种固定的读法(一次读2个，或者一次读取3个，都读不正确)，这样乱码就产生了。

#### 字符输入流【Reader】

* `public void close()`:关闭此流并释放与此流相关联的任何系统资源
* `public int read()`:从输入流读取一个字符
* `public int read(char[] ch)`:从输入流中读取一些字符，并将它们存储到字符数组ch中

#### FileReader类

#####构造方法

* `FileReader(File file)`:创建一个新的FileReader，给定要读取的File对象
* `FileReader(String fileName)`:创建一个新的FileReader，给定要读取的文件的名称

##### 读取字符数据

 	1. **读取字符**：`read`方法，每次可以读取一个字符的数据，提升为int类型，读取到文件末尾，返回`-1`,循环读取
 	2. **使用字符数组读取**：`read(char[] ch)`,每次读取b的长度个字符到数组中，返回读取到的有效字符个数，读取到末尾时。返回`-1`

#### 字符输出流【Writer】

##### 构造方法

* `FileWriter(File file)`:创建一个新的FileWriter，给定要读取的File对象

* `FileWriter(String fileName)`:创建一个新的FileWriter，给定要读取的文件的名称

   **写出字符**：`write(int b)`方法，每次可以写出一个字符数据

  **关闭和刷新**

  ​	因为内置缓冲区的原因，如果不关闭输出流，无法写出字符到文件中，但是关闭的流对象是无法继续写出数据的。如果我们即想写出数据，又想继续使用流，就需要`flush`方法

* `flush`:刷新缓冲区，流对象可以继续使用

* `close`:先刷新缓冲区，然后通知系统释放资源。流对象不可以再被使用了

##### 写出其他数据

 1. **写出字符数组**：`write(char[] ch)`和`write(char[] ch, int off, int len)`,每次可以写出字符数组中的数据

 2. **写出字符串**：`write(String str)`和`write(String str, int off, int len)`,每次可以写出字符串中的数据

    字符流复制文件【很重要】

    ```java
    //利用字符流来复制文件
    FileReader fr = new FileReader("day09-code\\a.txt");
    FileWriter fw = new FileWriter("day09-code\\b.txt");
    //一次读写多个字符
    char[] ch = new char[1024];
    int len;//记录每次读取的有效个数
    while((len = fr.read(ch)) != -1) {
        fw.write(ch, 0, len);
    }
    //释放资源
    fw.close();
    fr.close();
    ```

    

    

    ## Properties集合

    ​	Properties是一个双列集合，键和值只能是字符串类型

    #### 构造方法

* `public Properties()`:创建一个空的属性列表

#### 基本的存储方法

* `public Object setProperty(String key, String value)`:保存一对属性
* `public String getProperty(String key)`:使用此属性列表中指定的键搜索属性值
* `public Set<String> stringPropertyNames()`:所有键的名称的集合

#### 与流相关的方法

* `public void load(InputStream inStream)`:从字节输入流中读取键值对
* `public void store(Writer writer, String comments) `  把Properties集合中的键值对，存储到文件中

```java
	Properties pro=new Properties();
	pro.setProperty("charset","utf-8");
	pro.setProperty("date","20080909");
	pro.setProperty("url","http://www.abidu.com");
	//把键值对存储到config.properties（配置文件）
	pro.store(new FileWriter("day09-code\\config.properties"),"hello");//hello 为注释
	//读取config.properties文件中的键值对到集合
	pro.load(new FileReader("day09-code\\config.properties"));
	System.out.println(pro);
```

​	代码演示：

```java
Properties pro=new Properties();
//添加键值对到集合
pro.setProperty("name","LOL");
pro.setProperty("date","2018-08-08");
//把集合中的键值对，存储到config.pro文件中。
pro.store(new FileWriter("day10-code\\config.pro"),null);
//把config.pro文件中的键值对，读取到Properties集合中
pro.load(new FileReader("day10-code\\config.pro"));
//获取所有的键
Set<String> keys=pro.stringPropertyNames();
for(String key:keys){
    //通过键获取值
    String value=pro.getProperty(key);
    System.out.println(key+"="+value);
}

```

​	应用：

```java
/*
    实现一个验证程序运行次数的小程序，要求如下：
    1.当程序运行超过3次时给出提示:本软件只能免费使用3次,欢迎您注册会员后继续使用~
    2.程序运行演示如下:
        第一次运行控制台输出: 欢迎使用本软件,第1次使用免费~
        第二次运行控制台输出: 欢迎使用本软件,第2次使用免费~
        第三次运行控制台输出: 欢迎使用本软件,第3次使用免费~
        第四次及之后运行控制台输出:本软件只能免费使用3次,欢迎您注册会员后继续使用~
 */
public class Test {
    public static void main(String[] args) throws IOException {
        Properties pro=new Properties();
        //每次运行程序时，就读取配置文件config.pro
        pro.load(new FileReader("day10-code\\config.pro"));
        //通过get方法获取count对应的值
        String count = pro.getProperty("count");
        int value = Integer.parseInt(count);
        if(value>0){
                      //运行一次次数-1
            value--;
            System.out.println("欢迎使用本软件,第"+(3-value)+"次使用免费~");
            //每次使用完毕之后，需要白新的值，写回配置文件中
            pro.setProperty("count",value+"");
            pro.store(new FileWriter("day10-code\\config.pro"),null);
        }else{
            System.out.println("本软件只能免费使用3次,欢迎您注册会员后继续使用~");
        }
    }
}

```

