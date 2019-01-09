# ArrayList

## 与数组Array的区别

- 数组的长度不可以发生改变。  

- 但是ArrayList集合的长度是可以随意变化的。  

    对于ArrayList来说，有一个尖括号<E>代表泛型。  
    泛型： 也就是装在集合当中的所有元素，全都是统一的什么类型  

   **注意：泛型只能是引用类型，而不是基本类型**。  

    如果希望向集合ArrayList当中存储基本数据类型，必须使用基本类型对应的包装类：  
    基本类型 包装类（引用类型，包装类都位于java.lang包下）  
    **byte(Byte) **

   **short(Short)**

   **int(Integer)**

   **long(Long)**

   **float(Float)** 

  **double(Double)** 

  **char(Character)**

  **boolean(Boolean)**  
      
  从JDK1.5+开始，支持自动装箱，拆箱。
  **自动装箱**：基本类型 --> 包装类型  
  **自动拆箱**：包装类型-->基本类型
      
  **注意事项：**  
  对于ArrayList集合来说，直接打印得到的不是地址值，而是内容。  
  如果内容为空，得到的是空的中括号：[]

```
public class DemoArrayList{
    public static void main (String[] args) {
        //创建了一个ArrayList集合，集合的名称是list，里面装的全都是String字符串类型的数据 
        //备注：从JDK1.7+开始，右侧的尖括号内部可以不写内容
        Arraylist<String> list = new ArrayList<>();
        System.out.println(list);//[]
        //向集合当中添加一些数据，需要用到add方法。
        list.add("赵丽颖");
        System.out.println(list);//[赵丽颖]
        list.add("迪丽热巴");
        list.add("古力娜扎");
        System.out.println(list);//[赵丽颖，迪丽热巴，古力娜扎]
        list.add(100);//错误写法，因为在初始化一个ArrayList的时候已经定义为一个字符串类型
    }
}
```
## ArrayList集合常用方法和遍历

### ArrayList当中的常用方法有：

`public boolean add(E e):（eg：boolean success = list.add("柳岩");）`，向集合当中添加元素，参数类型和泛型（E）一致。  
**备注：对于ArrayList集合来说，add添加操作一定是成功的，所以返回值可用可不用。但是对于其他集合来说，不一定成功。**  
`public E get(int index)`:从集合当中获取元素，参数是索引编号，返回值就是对应位置的元素。  
`public E remove(int index)`:从集合当中删除元素，参数是索引编号，返回值就是被删除掉的元素。  
`public int size（）`：获取集合的尺寸长度，返回值是集合中包含的元素个数。

`public E set(int index, E e)`:修改指定索引位置的元素并返回被修改的元素  
`public boolean isEmpty()`:判断集合是否为空，空位true，不空false  
`public boolean contains(E e)`:判断集合是否包含。包含为true，不包含为false
#### 集合也可以作为方法参数被拿进去，所传递的是集合的地址值
```
/*
题目：
定义以指定打印集合的方法（ArrayList类型作为参数），使用｛｝扩起集合，使用@分隔每个元素
格式参照｛元素@元素@元素｝。
*/
public class DemoArrayListPrint{
    public static void main (String[] args) {
        ArrayList<String> list = new ArrayList<> ();
        list.add("张三丰")；
        list.add("张三")；
        list.add("李四")；
        System.out.println(list);//[张三丰，张三，李四]
        public static void printArraylist（ArrayList<String> list）{
            System.out.println("{");
            for (int i = 0; i < list.size(); i++) {
                String name = list.get(i);
                if (i == list.size() -1) {
                    System.out.println(name + "}");
                }
                System.out.print(name + "@");
            }
        }
    }
}
```
#### 集合可以作为方法的参数也可以作为方法的返回值，当作为参数的时候传递进去的是地址值当作为返回值的时候返回回来的也是地址值
```
public class DemoArrayReturn {
    public static void main (String[] args) {
        ArrayList<Integer> bigList = new ArrayList<> ();
        Random r = new Random();
        for (int i = 0; i < 20; i++) {
            int num = r.nextInt(bound:100) + 1;//1~100
            biglist.add(num);
        }
        ArrayList<Integer> smallList = getSmallList(bigList);
        System.out.println("偶数的个数为："+ smallList.size());
        for (int i = 0; i <smallList.size();i++) {
            System.out.println(smallList.get(i));
        }
    }
    public static ArrayList<Integer> getSamllList(Arraylist<Integer> biglist) {
        ArrayList<Integer> smallList = new ArrayList<> ();
        for (int i = 0; i < bigList.size(); i++) {
            int num = bigList.get(i);
            if(num %2 == 0) {
                smallList.add(num);
            }
            
        }
        return smallList;
    }
}
```