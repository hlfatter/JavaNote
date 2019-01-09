# Collection接口、泛型与Collections工具类

## Collection接口

-  **List接口**(特点：有序、有索引、元素可重复)      

  **Arraylist**：底层数据结构（数组结构），特点（查询快、增删慢）

  **LinkedList**：底层数据结构（链表），特点（查询慢、增删块）

  **Vector**：底层数据结构（数组结构），特点（查询快、增删慢）

- **Set接口**(特点 ：无序、无索引、不能重复)

  **HeshSet**：底层数据结构（哈希表结构（数组+链表+红黑树）），保证元素唯一性（重写元素的hashCode()和equals()方法）------hashCode()是Object类中用来获取任意一个对象的哈希值（地址值）

  **LinkedHashSet**：是HashSet的子类，底层数据结构：基于链表哈希表结构，既能保证元素有序（链表决定）也能保证元素唯一，保证元素唯一性（重写元素的hashCode()和equals()方法）

  **TreeSet**：

**注意**：上面所说的有序和无序指的是把元素放进去和拿出来的顺序不一样也就是放进去和拿出来的元素是否对应（对应即为有序，不对应即为无序）

## Collection集合共性地方法

```java
public boolean add(E e):把元素添加到集合末尾
public boolean remove（E e）：把元素从集合中删除
public void clear（）：清空集合中的元素
public boolean isEmpty（）：判断集合是否为空
public boolean contains（E e）：判断集合是否包含指定的元素
public Object[] toArray（）：把集合转换为一个数组。数组的元素都会被提升为Object类
public int size（）：获取集合中元素的个数
public Iterator（）：获取集合的迭代器
```

### 集合的遍历方式

```java
ArrayList<String> list = new ArrayList<> ();
list.add("hello");
list.add("java");

1. 普通for循环（只能遍历有索引的集合，可以进行增删改等操作）
for(int i = 0; i < list.size(); i++) {
	String e = list.get(i);
	System.out.println(e);
}

2. 迭代器遍历
//获取迭代器
Iterator it = list.iterator();//多态的写法
//判断是否有元素可以迭代
//boolean hasNext():判断集合中还有没有下一个元素，有就返回true，没有就返回false
// E next() 返回迭代的下一个元素
while(it.hasNext()) {
	String e = it.next();
	System.out.println(e);
}

3. 高级for循环（可以遍历任何集合，但是一般只用于获取元素）:格式（for(集合元素类型 名称： 集合){}）
for(String s : list) {
	System.out.println(s);
}
```

## 集合泛型

### 概念：

#### 什么是泛型？为什么要使用泛型？

```tex
泛型，即“参数化类型”。一提到参数，最熟悉的就是定义方法时有形参，然后调用此方法时传递实参。那么参数化类型怎么理解呢？顾名思义，就是将类型由原来的具体的类型参数化，类似于方法中的变量参数，此时类型也定义成参数形式（可以称之为类型形参），然后在使用/调用时传入具体的类型（类型实参）。

泛型的本质是为了参数化类型（在不创建新的类型的情况下，通过泛型指定的不同类型来控制形参具体限制的类型）。也就是说在泛型使用过程中，操作的数据类型被指定为一个参数，这种参数类型可以用在类、接口和方法中，分别被称为泛型类、泛型接口、泛型方法。
```

#### 泛型的好处

 1. 把运行时期的错误转移到编译时期  

 2. 可以避免强制转换的麻烦

    

#### 泛型的使用

​	三种使用方式：泛型类、泛型接口、泛型方法

##### 泛型类

​	用于类的定义中，被称为泛型类。通过泛型可以完成对一组类的操作对外开放相同的接口。典型的就是各种容器类，如：List、Set、Map。

​	泛型类的基本写法

```java
class 类名称<泛型标识： 可以随便写任意标识号，标识指定的泛型的类型>{
  private 泛型标识 /*（成员变量类型）*/ var; 
  .....

  }
}
```

​	eg:

```java
//此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型
//在实例化泛型类时，必须指定T的具体类型
public class Generic<T>{ 
    //key这个成员变量的类型为T,T的类型由外部指定  
    private T key;

    public Generic(T key) { //泛型构造方法形参key的类型也为T，T的类型由外部指定
        this.key = key;
    }

    public T getKey(){ //泛型方法getKey的返回值类型为T，T的类型由外部指定
        return key;
    }
}

//泛型的类型参数只能是类类型（包括自定义类），不能是简单类型
//传入的实参类型需与泛型的类型参数类型相同，即为Integer.
Generic<Integer> genericInteger = new Generic<Integer>(123456);

//传入的实参类型需与泛型的类型参数类型相同，即为String.
Generic<String> genericString = new Generic<String>("key_vlaue");
Log.d("泛型测试","key is " + genericInteger.getKey()); //泛型测试: key is 123456
Log.d("泛型测试","key is " + genericString.getKey());  //泛型测试: key is key_vlaue


```

​	定义的泛型类，在使用泛型的时候如果传入泛型实参，则会根据传入法人泛型实参做相应的限制，此时泛型才会起到本应该起到的限制作用。如果不传入泛型类型实参的话，在泛型类中使用泛型的方法或成员变量定义的类型可以为任何的类型。

比如这个例子：

```java
Generic generic = new Generic("111111");
Generic generic1 = new Generic(4444);
Generic generic2 = new Generic(55.55);
Generic generic3 = new Generic(false);

Log.d("泛型测试","key is " + generic.getKey());  //泛型测试: key is 111111
Log.d("泛型测试","key is " + generic1.getKey()); //泛型测试: key is 4444
Log.d("泛型测试","key is " + generic2.getKey()); //泛型测试: key is 55.55
Log.d("泛型测试","key is " + generic3.getKey()); //泛型测试: key is false
```

##### 泛型接口

​	泛型接口的基本写法

```java
//定义一个泛型接口
public interface Generator<T>{
    public T next();
}
```

​	当实现泛型接口的类未传入泛型实参时：

```java
/**
 * 未传入泛型实参时，与泛型类的定义相同，在声明类的时候，需将泛型的声明也一起加到类中
 * 即：class FruitGenerator<T> implements Generator<T>{
 * 如果不声明泛型，如：class FruitGenerator implements Generator<T>，编译器会报错："Unknown class"
 */
class FruitGenerator<T> implements Generator<T>{
    @Override
    public T next() {
        return null;
    }
}
```

​	当传入实参时：

```java
/**
 * 传入泛型实参时：
 * 定义一个生产器实现这个接口,虽然我们只创建了一个泛型接口Generator<T>
 * 但是我们可以为T传入无数个实参，形成无数种类型的Generator接口。
 * 在实现类实现泛型接口时，如已将泛型类型传入实参类型，则所有使用泛型的地方都要替换成传入的实参类型
 * 即：Generator<T>，public T next();中的的T都要替换成传入的String类型。
 */
public class FruitGenerator implements Generator<String> {

    private String[] fruits = new String[]{"Apple", "Banana", "Pear"};

    @Override
    public String next() {
        Random rand = new Random();
        return fruits[rand.nextInt(3)];
    }
}
```

##### 泛型方法

​	泛型方法，是在调用方法的时候指明泛型的具体类型

```java
/**
 * 泛型方法的基本介绍
 * @param tClass 传入的泛型实参
 * @return T 返回值为T类型
 * 说明：
 *     1）public 与 返回值中间<T>非常重要，可以理解为声明此方法为泛型方法。
 *     2）只有声明了<T>的方法才是泛型方法，泛型类中的使用了泛型的成员方法并不是泛型方法。
 *     3）<T>表明该方法将使用泛型类型T，此时才可以在方法中使用泛型类型T。
 *     4）与泛型类的定义一样，此处T可以随便写为任意标识，常见的如T、E、K、V等形式的参数常用于表示泛型。
 */
public <T> T genericMethod(Class<T> tClass)throws InstantiationException ,
  IllegalAccessException{
        T instance = tClass.newInstance();
        return instance;
}
```

```java
public class GenericTest {
   //这个类是个泛型类，在上面已经介绍过
   public class Generic<T>{     
        private T key;

        public Generic(T key) {
            this.key = key;
        }

        //我想说的其实是这个，虽然在方法中使用了泛型，但是这并不是一个泛型方法。
        //这只是类中一个普通的成员方法，只不过他的返回值是在声明泛型类已经声明过的泛型。
        //所以在这个方法中才可以继续使用 T 这个泛型。
        public T getKey(){
            return key;
        }

        /**
         * 这个方法显然是有问题的，在编译器会给我们提示这样的错误信息"cannot reslove symbol E"
         * 因为在类的声明中并未声明泛型E，所以在使用E做形参和返回值类型时，编译器会无法识别。
        public E setKey(E key){
             this.key = keu
        }
        */
    }

    /** 
     * 这才是一个真正的泛型方法。
     * 首先在public与返回值之间的<T>必不可少，这表明这是一个泛型方法，并且声明了一个泛型T
     * 这个T可以出现在这个泛型方法的任意位置.
     * 泛型的数量也可以为任意多个 
     *    如：public <T,K> K showKeyName(Generic<T> container){
     *        ...
     *        }
     */
    public <T> T showKeyName(Generic<T> container){
        System.out.println("container key :" + container.getKey());
        //当然这个例子举的不太合适，只是为了说明泛型方法的特性。
        T test = container.getKey();
        return test;
    }

    //这也不是一个泛型方法，这就是一个普通的方法，只是使用了Generic<Number>这个泛型类做形参而已。
    public void showKeyValue1(Generic<Number> obj){
        Log.d("泛型测试","key value is " + obj.getKey());
    }

    //这也不是一个泛型方法，这也是一个普通的方法，只不过使用了泛型通配符?
    //同时这也印证了泛型通配符章节所描述的，?是一种类型实参，可以看做为Number等所有类的父类
    public void showKeyValue2(Generic<?> obj){
        Log.d("泛型测试","key value is " + obj.getKey());
    }

     /**
     * 这个方法是有问题的，编译器会为我们提示错误信息："UnKnown class 'E' "
     * 虽然我们声明了<T>,也表明了这是一个可以处理泛型的类型的泛型方法。
     * 但是只声明了泛型类型T，并未声明泛型类型E，因此编译器并不知道该如何处理E这个类型。
    public <T> T showKeyName(Generic<E> container){
        ...
    }  
    */

    /**
     * 这个方法也是有问题的，编译器会为我们提示错误信息："UnKnown class 'T' "
     * 对于编译器来说T这个类型并未项目中声明过，因此编译也不知道该如何编译这个类。
     * 所以这也不是一个正确的泛型方法声明。
    public void showkey(T genericObj){

    }
    */

    public static void main(String[] args) {


    }
}
```

#### 类中的泛型方法

```java
public class GenericFruit {
    class Fruit{
        @Override
        public String toString() {
            return "fruit";
        }
    }

    class Apple extends Fruit{
        @Override
        public String toString() {
            return "apple";
        }
    }

    class Person{
        @Override
        public String toString() {
            return "Person";
        }
    }

    class GenerateTest<T>{
        public void show_1(T t){
            System.out.println(t.toString());
        }

        //在泛型类中声明了一个泛型方法，使用泛型E，这种泛型E可以为任意类型。可以类型与T相同，也可以不同。
        //由于泛型方法在声明的时候会声明泛型<E>，因此即使在泛型类中并未声明泛型，编译器也能够正确识别泛型方法中识别的泛型。
        public <E> void show_3(E t){
            System.out.println(t.toString());
        }

        //在泛型类中声明了一个泛型方法，使用泛型T，注意这个T是一种全新的类型，可以与泛型类中声明的T不是同一种类型。
        public <T> void show_2(T t){
            System.out.println(t.toString());
        }
    }

    public static void main(String[] args) {
        Apple apple = new Apple();
        Person person = new Person();

        GenerateTest<Fruit> generateTest = new GenerateTest<Fruit>();
        //apple是Fruit的子类，所以这里可以
        generateTest.show_1(apple);
        //编译器会报错，因为泛型类型实参指定的是Fruit，而传入的实参类是Person
        //generateTest.show_1(person);

        //使用这两个方法都可以成功
        generateTest.show_2(apple);
        generateTest.show_2(person);

        //使用这两个方法也都可以成功
        generateTest.show_3(apple);
        generateTest.show_3(person);
    }
}
```

#### 静态方法与泛型

​	在类中的静态方法使用泛型：**静态方法无法访问类上定义的泛型；如果静态方法操作的引用数据类型不确定的时候，必须要将泛型定义在方法上**

​	即：**如果静态方法要使用泛型的话，必须将静态方法也定义成泛型方法**

```java
public class StaticGenerator<T> {
    ....
    ....
    /**
     * 如果在类中定义使用泛型的静态方法，需要添加额外的泛型声明（将这个方法定义成泛型方法）
     * 即使静态方法要使用泛型类中已经声明过的泛型也不可以。
     * 如：public static void show(T t){..},此时编译器会提示错误信息：
          "StaticGenerator cannot be refrenced from static context"
     */
    public static <T> void show(T t){

    }
}
```

#### 泛型上下边界

​	在使用泛型的时候，我们可以为传入的泛型类型实参进行上下边界的限制，如：类型实参只准传入某种类型的父类或某种类型的子类

- 为泛型添加上边界，即传入的类型实参必须是指定类型的子类型或本身

  `? extends E    E或者E的子类`

- 为泛型添加下边界，即传入的类型实参必须是指定类型的父类型或本身

  `? super E	   E或者E的父类`

**List集合相对于Collection集合多了一些针对索引操作的方法**

```java
public add(int index,E e)
	  添加元素到指定索引位置
	public E remove(int index)
	  删除指定索引位置的元素
	public E set(int index,E e)
	  修改指定索引位置的元素
	public E get(int index)
	  获取指定索引位置的元素
```

**LinkedList相对于List多了一些针对头尾进行操作的方法**

```java
public void addFirst() 在集合的头部添加元素
	public void addLast()   在集合的尾部添加元素
	public void push()     往集合的头部添加元素（模拟压栈）
	public E removeFirst() 移除第一个元素
	public E removeLast()  移除最后一个元素
	public E pop()       移除集合中的第一个元素(模拟弹栈):即后进先出
	public E getFirst()  获取第一个元素
	public E getLast()   获取最后一个元素
```

#### 常见的数据结构

 1. 栈：先进后出（ 可以想象成一个子弹夹）  

 2. 队列：先进先出（可以想象成安检机）

 3. 数组：查询块，增删慢

 4. 链表：增删块，查询慢

 5. 红黑树：查询速度特别快

    

## Collectons工具类

​	用一个具体的代码实例去说明有几种及其用法：

```java
//首先定义一个学生类，属性有名字和年龄及里面覆盖重写toString()方法
//在ArrayList集合中存储自定义对象
ArrayList<Student> s = new ArrayList<> ();
	s..add(new Student("bbbb",19));
	s.add(new Student("zhangsan",20));
	s.add(new Student("lis",18));
	s.add(new Student("aaaa",19));
//注意：自定义对象本身是不具备排序规则的，需要我们手动指定排序规则
Collections.sort(s, new Comparator<Student>() {
		@Override
		public int compare(Student o1, Student o2) {
			//排序的规则
			//升序：左边-右边
			//降序：右边-左边
			int num=o1.getAge()-o2.getAge();

			//如果年龄相同就按照姓名排序
			if(num==0){
				//按照姓名升序排列,字符串比较应该使用comparteTo方法
				num=o1.getName().compareTo(o2.getName());
			}
			return num;
		}
	});
	
//求集合中元素的最大(小)值的方法
	Collections.max();
	Collections.min();
//打乱集合中元素的顺序
	Collections.shuffleS();
```

## 集合综合案例

### 案例介绍

​	按照斗地主的规则，完成洗牌发牌的动作。 具体规则：

使用54张牌打乱顺序,三个玩家参与游戏，三人交替摸牌，每人17张牌，最后三张留作底牌。

### 案例分析

- 准备牌：

  牌可以设计为一个ArrayList<String>,每个字符串为一张牌。 每张牌由花色数字两部分组成，我们可以使用花色集合与数字集合嵌套迭代完成每张牌的组装。 牌由Collections类的shuffle方法进行随机排序。

- 发牌

  将每个人以及底牌设计为ArrayList<String>,将最后3张牌直接存放于底牌，剩余牌通过对3取模依次发牌。

- 看牌

  直接打印每个集合。

**注：这部分可以在后期思想成熟后使用面向对象的思想进行代码的实现**

### 代码实现

```java
import java.util.ArrayList;
import java.util.Collections;

public class Poker {
    public static void main(String[] args) {
        /*
        * 1: 准备牌操作
        */
        //1.1 创建牌盒 将来存储牌面的 
        ArrayList<String> pokerBox = new ArrayList<String>();
        //1.2 创建花色集合
        ArrayList<String> colors = new ArrayList<String>();

        //1.3 创建数字集合
        ArrayList<String> numbers = new ArrayList<String>();

        //1.4 分别给花色 以及 数字集合添加元素
        colors.add("♥");
        colors.add("♦");
        colors.add("♠");
        colors.add("♣");

        for(int i = 2;i<=10;i++){
            numbers.add(i+"");
        }
        numbers.add("J");
        numbers.add("Q");
        numbers.add("K");
        numbers.add("A");
        //1.5 创造牌  拼接牌操作
        // 拿出每一个花色  然后跟每一个数字 进行结合  存储到牌盒中
        for (String color : colors) {
            //color每一个花色 
            //遍历数字集合
            for(String number : numbers){
                //结合
                String card = color+number;
                //存储到牌盒中
                pokerBox.add(card);
            }
        }
        //1.6大王小王
        pokerBox.add("小☺");
        pokerBox.add("大☠");	  
        // System.out.println(pokerBox);
        //洗牌 是不是就是将  牌盒中 牌的索引打乱 
        // Collections类  工具类  都是 静态方法
        // shuffer方法   
        /*
         * static void shuffle(List<?> list) 
         *     使用默认随机源对指定列表进行置换。 
         */
        //2:洗牌
        Collections.shuffle(pokerBox);
        //3 发牌
        //3.1 创建 三个 玩家集合  创建一个底牌集合
        ArrayList<String> player1 = new ArrayList<String>();
        ArrayList<String> player2 = new ArrayList<String>();
        ArrayList<String> player3 = new ArrayList<String>();
        ArrayList<String> dipai = new ArrayList<String>();	  

        //遍历 牌盒  必须知道索引   
        for(int i = 0;i<pokerBox.size();i++){
            //获取 牌面
            String card = pokerBox.get(i);
            //留出三张底牌 存到 底牌集合中
            if(i>=51){//存到底牌集合中
                dipai.add(card);
            } else {
                //玩家1   %3  ==0
                if(i%3==0){
                  	player1.add(card);
                }else if(i%3==1){//玩家2
                  	player2.add(card);
                }else{//玩家3
                  	player3.add(card);
                }
            }
        }
        //看看
        System.out.println("令狐冲："+player1);
        System.out.println("田伯光："+player2);
        System.out.println("绿竹翁："+player3);
        System.out.println("底牌："+dipai);  
	}
}
```

