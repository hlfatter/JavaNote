# String

## java.lang.String类代表字符串

  ### API当中说，Java程序中的所有字符串值都作为此类的实例实现

  ###   其实就是说，程序当中所有双引号串，都是String类的对象。（就算没有new，也照样是）

  #### 字符串的特点：

1. 字符串的内容永不改变。【重点】
2. 正是因为字符串不可改变，所以字符串是可以共享使用的。
3. 字符串的效果相当于char[]字符数组，但是底层原理是byte[]字节数组。  
  
####创建字符串的常见3+1种方式。  

三种构造方法：  
`public String()`:创建一个空白字符串，不含任何内容。  
`public String(char[] array)`,根据字符串数组的内容，来创建对应的字符串。  
`public String(byte[] array)`,根据字节数组的内容，来创建对应的字符串。  
 还有一种直接创建  
**字符串常量池：程序当中直接写上的双引号字符串，就在字符串常量池中**  
对于基本类型来说，==是进行数值的比较。  
对于引用类型来说，==是进行地址值的比较。  
如果确实需要字符串的内容比较，可以使用两种方法：  
`public boolean equals (Object obj)`:参数可以是任何对象，只有参数是一个字符串并且内容相同的才会给true；否则返回false  
**注意事项**

1. 任何对象都能用Object进行接收。
2. equals方法具有对称性，也就是a.equals(b)和b.equals(b)项目一样。
3. 如果比较双方一个常量一个变量，推荐把常量字符串写在前面。  
推荐："abc".equals(str)  不推荐：str.equals("abc")  
  
`public boolean equalsIgnoreCase(String str)` :忽略大小写，进行内容比较（只有英文才区分大小写）  
**String当中与获取相关的常用方法有：** 
`public boolean startWith(String str)`:判断字符串是不是以它为开始的
`public int Length()`:获取字符串当中含有的字符个数，拿到字符串长度。  
`public String concat(String str)`:将当前字符串和参数字符串拼接成为返回值新的字符串。  
`public char charAt(int index)`:获取指定索引位置的单个字符。（索引从0开始）  
`public int indexOf(String str)`:查找参数字符串在本字符串当中首次出现的索引位置，如果没有返回-1值。  
`trim()`： 把字符串的前后空白去掉  
`compareTo()`：按照字典顺序（a~z）比较字符串的大小（小：排在前面  大：排在后面）如果它的返回值是正数则左边比右边大（如果前面几个是一样的则说明是长度大，如果前面不相同，则说明是字母的大小顺序）如果为零则说明两个字符串相等。

**字符串的截取方法：**  
`public String substring(int index)`:截取从参数位置一直到字符串末尾，返回新字符串。  
`public String substring(int begin, int end)`:截取从begin开始,一直到end结束，中间的字符串。  
**备注：[begin,end),包含左边，不包含右边**  

**String当中与转换相关的常用方法有：**  
`public char[] toCharArray()`：将当前字符串拆分成为字符数组作为返回值。  
`public byte[] getBytes()`：获取当前字符串底层的字节数组。  
`public String replace(CharSequence oldString, CharSequence newString)`:将所有出现的老字符串替换成为新的字符串，返回替换之后的结果新字符串。  
**备注：CharSequence意思就是说可以接受字符串类型。**  
**分割字符串的方法：**
`public String[] split(String regex)`:按照参数的规则，将字符串切割分成为若干部分。  
**注意事项：**  
split方法的参数其实是一个“正则表达式”，今后学习。  
今天要注意：如果按照英文句点“.”进行切分，必须写“ \\\\. ”  
**字符串的拼接还可以通过“+”来实现**  
**字符串转化大小写**：  
toUpperCase()转成大写  
toLowerCase()转成小写
