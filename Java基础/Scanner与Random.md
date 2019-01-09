# Scanner与Random

## Scanner

### Scanner类的功能

​	可以实现键盘输入数据，到程序当中。  

  引用类型的一般步骤：  
    
    1. 导包  
    import  包路径.类名称；  
    如果要使用的目标类，和当前类位于同一个包下，则可以省略导包语句不写。  
    只有java.lang包下的内容不需要导包，其他的包都需要import语句。  
      
    2. 创建  
    类名称 对象名 = new 类名称();  
      
    3. 使用  
    对象名.成员方法名（）
```
public class Demo{
    public static void main (String[] args) {
        Scanner sc = new Scaaner(System.in);
        int num sc.nextInt();//获得键盘输入的int数字
        System.out.println("输入的int数字是：" + num);
        String str = sc.next();//获得键盘输入的字符串
        System.out.println.("输入法人字符串是：" + str);
    }
}
注 ：Scanner键盘输入的就是一个字符串，nextInt是将字符串转换成int类型.
```
## Random

### Random类的功能

​	用来生成随机数字

使用起来也是三个步骤：  

1. 导包  
import java.util.Random;  
2. 创建  
Random r = new Random();//小括号当中留空即可
3. 使用  
获取一个随机的int数字（范围是int所有范围，有正负两种）：int num = r.nextInt()  
获取一个随机的int数字（参数代表了范围，有正负两种，左bi右开区间）：int num = r.nextInt(3)实际上代表的含义是：[0,3),也就是0~2  
#### eg：
```
public class Demo{
    public static void main (String [] args) {
        Random r = new Random();
        int randomNum = r.nextInt(bound:100)+ 1; //[1,100]
        Scanner sc = new Scanner(System.in);
        while(true) {
            System.out.println("请输入你猜测的数字：");
            int guessNum = sc.nextInt();//键盘输入的猜测数字
            if(guessNum > randomNum) {
                System.out.println("太大了，请重试");
            } else  if(guessNum < randomNum) {
                System.out.println("太小了，请重试");
            } else {
            System.out.println("恭喜你，猜中了");
            break；
            }
        }
         System.out.println("游戏结束");
    }
}
```