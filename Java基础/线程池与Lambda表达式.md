# 线程池、Lambda表达式

# 等待唤醒机制

## 线程间通信

**概念：**多个线程在处理同一个资源，但是处理的动作（线程的任务）却不相同

**为什么要处理线程间通信：**

​	多个线程并发执行时，在默认情况下CPU是随机切换线程的，当我们需要多个线程来共同完成一件任务，并且我们希望他们有规律的执行，那么多线程之间需要一些协调通信，以此来帮我们达到多线程共同操作一份数据

如何保证线程间通信有效利用资源：**等待唤醒机制**

## 等待唤醒机制

​	就是在一个线程进行了规定操作后，就进入等待状态（**wait()**）,等待其他线程执行完他们的指定代码后 再将其唤醒（**notify()**）;在有多个线程进行等待时，如果需要，可以使用notifyAll()来唤醒所有的等待线程

wait/notify 就是线程间的一种协作机制

**等待唤醒中的方法**

1. wait：线程不再活动，不再参与调度，进入 wait set 中，因此不会浪费 CPU 资源，也不会去竞争锁了，这时的线程状态即是 WAITING。它还要等着别的线程执行一个**特别的动作**，也即是“**通知（notify）**”在这个对象上等待的线程从wait set 中释放出来，重新进入到调度队列（ready queue）中
2. notify：则选取所通知对象的 wait set 中的一个线程释放；例如，餐馆有空位置后，等候就餐最久的顾客最先入座。
3. notifyAll：则释放所通知对象的 wait set 上的全部线程。

> 注意：
>
> 如果能获取锁，线程就从WAITING状态变成RUNNABLE状态
>
> 否则，从wait set 出来，又进入entry  set, 线程就从WAITING状态又变成BLOCKED状态



**调用wait和notify方法需要注意的细节**

1. wait方法与notify方法必须要由同一个锁对象调用。因为：对应的锁对象可以通过notify唤醒使用同一个锁对象调用的wait方法后的线程。
2. wait方法与notify方法是属于Object类的方法的。因为：锁对象可以是任意对象，而任意对象的所属类都是继承了Object类的。
3. wait方法与notify方法必须要在同步代码块或者是同步函数中使用。因为：必须要通过锁对象调用这2个方法。

例子：

```java
文字分析：包子铺线程生产包子，吃货线程消费包子。当包子没有时（包子状态为false），吃货线程等待，包子铺线程生产包子（即包子状态为true），并通知吃货线程（解除吃货的等待状态）,因为已经有包子了，那么包子铺线程进入等待状态。接下来，吃货线程能否进一步执行则取决于锁的获取情况。如果吃货获取到锁，那么就执行吃包子动作，包子吃完（包子状态为false），并通知包子铺线程（解除包子铺的等待状态）,吃货线程进入等待。包子铺线程能否进一步执行则取决于锁的获取情况。

代码演示：
public class BaoZi {
     String  pier ;
     String  xianer ;
     boolean  flag = false ;//包子资源 是否存在  包子资源状态
}
```

```java
吃货线程类：
public class ChiHuo extends Thread{
    private BaoZi bz;

    public ChiHuo(String name,BaoZi bz){
        super(name);
        this.bz = bz;
    }
    @Override
    public void run() {
        while(true){
            synchronized (bz){
                if(bz.flag == false){//没包子
                    try {
                        bz.wait();
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
                System.out.println("吃货正在吃"+bz.pier+bz.xianer+"包子");
                bz.flag = false;
                bz.notify();
            }
        }
    }
}
```

```java
包子铺线程类：
public class BaoZiPu extends Thread {

    private BaoZi bz;

    public BaoZiPu(String name,BaoZi bz){
        super(name);
        this.bz = bz;
    }

    @Override
    public void run() {
        int count = 0;
        //造包子
        while(true){
            //同步
            synchronized (bz){
                if(bz.flag == true){//包子资源  存在
                    try {

                        bz.wait();

                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }

                // 没有包子  造包子
                System.out.println("包子铺开始做包子");
                if(count%2 == 0){
                    // 冰皮  五仁
                    bz.pier = "冰皮";
                    bz.xianer = "五仁";
                }else{
                    // 薄皮  牛肉大葱
                    bz.pier = "薄皮";
                    bz.xianer = "牛肉大葱";
                }
                count++;

                bz.flag=true;
                System.out.println("包子造好了："+bz.pier+bz.xianer);
                System.out.println("吃货来吃吧");
                //唤醒等待线程 （吃货）
                bz.notify();
            }
        }
    }
}

```

```java
测试类：
public class Demo {
    public static void main(String[] args) {
        //等待唤醒案例
        BaoZi bz = new BaoZi();

        ChiHuo ch = new ChiHuo("吃货",bz);
        BaoZiPu bzp = new BaoZiPu("包子铺",bz);

        ch.start();
        bzp.start();
    }
}
```

# 线程池

## 概念

​	就是一个容纳多个线程的容器，其中线程可以容纳多个线程的容器，其中的线程可以反复使用，省去了频繁创建线程对象的操作，无需反复创建线程而消耗过多资源

合理利用线程池能够带来三个好处：

1. 降低资源消耗
2. 提高响应速度
3. 提高线程的可管理性

## 线程池的使用

​	Java里面线程池的顶级接口是`java.util.concurrent.Executor`,但是严格意义上讲`Executor`并不是一个线程池，而只是一个执行线程的工具。真正的线程池接口是`java.util.concurrent.ExecutorService`

​	Executors类中有个创建线程池的方法：

* `public static ExecutorService newFixedThreadPool(int nThread)`:返回线程池对象
* `public Future<?> submit(Runnable task)`:获取线程池中的某一个线程对象，并执行

> Future接口：用来记录线程任务执行完毕后产生的结果

使用线程池中线程对象的步骤：

1. 创建线程池对象
2. 创建Runnable接口子类对象
3. 提交Runnable接口子类对象
4. 关闭线程池（一般不做）

Runnable实现类代码

```java
public class MyRunnable implements Runnable {
    @Override
    public void run() {
        System.out.println("我要一个教练");
        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("教练来了： " + Thread.currentThread().getName());
        System.out.println("教我游泳,交完后，教练回到了游泳池");
    }
}
```

线程池测试类

```java
public class ThreadPoolDemo {
    public static void main(String[] args) {
        // 创建线程池对象
        ExecutorService service = Executors.newFixedThreadPool(2);//包含2个线程对象
        // 创建Runnable实例对象
        MyRunnable r = new MyRunnable();

        //自己创建线程对象的方式
        // Thread t = new Thread(r);
        // t.start(); ---> 调用MyRunnable中的run()

        // 从线程池中获取线程对象,然后调用MyRunnable中的run()
        service.submit(r);
        // 再获取个线程对象，调用MyRunnable中的run()
        service.submit(r);
        service.submit(r);
        // 注意：submit方法调用结束后，程序并不终止，是因为线程池控制了线程的关闭。
        // 将使用完的线程又归还到了线程池中
        // 关闭线程池
        //service.shutdown();
    }
}
```

# Lambda表达式

## 体验Lambda的更优写法

借助`Runnable`接口的匿名内部类写法可以通过更简单的Lambda表达式达到等效：

```java
public class Demo{
    public static void main (String[] args) {
        new Thread(() -> System.out.println("多线程任务执行！")).start();//启动线程
    }
}
```

**函数式接口：有且仅有一个抽象方法的接口**
	//@FunctionalInterface 标记是否为函数式接口 
	@FunctionalInterface  
	public interface Runnable{
		public void run();
	}
	

	@FunctionalInterface  
	public interface Comparator<T>{
		public int compare(T o1,T o2);
	}

**Lambda表达式的前提**
	必须要由一个函数式接口： 有且仅有一个抽象方法的接口
	
	Lambda的标准格式
		(Type1 p1,Type2 p2)->{ return 语句体; };
	
	Lambda的缺省格式
		//1.数据类型可以省略
		(p1,p2)->{ return 语句体; };
		//2. 如果语句体只有一条语句，{}可以省略，return也可以省略
		(p1,p2)->语句体
		//3. 如果参数值有一个，()可以省略
		p1->语句体

Lambda表达式的实际运用
	把函数式接口作为方法的参数，调用方法的时候传递Lambda作为函数式接口的实现。
		函数式接口的实现方式：
			1. 匿名内部类
			2. Lambda表达式（你可以理解Lamdba是匿名内部类的简化写法）
		
	小扩展
		函数式接口在API中已经定义好了。使用接口的方法也定义好了。		
		我们要做的就是调用这些方法，传递Lamdba表达式。
	
	如下例子
		//线程任务需要Runnable接口，传递Lambda表达式
		new Thread(()->{
			//写上Rubbable接口的实现内容 
			...
		}).start();
		
		List<Integer> list=List.of(9,8,3,1,5,6);
		//排序需要Comparator接口，传递Lambda表达式进行实现(降序排列)
		Collections.sort(list,(Integer o1,Integer o2)->{
			return o2-o1;
		});
		
		//获取一个固定长度的线程池
		ExecutorService es=Executors.newFixedThreadPool(3);
		//submit(Runnable r) 提交线程任务
		es.submit(()->{
			System.out.println("线程开始执行了...");
		});

Lambda练习
	//数学运算
	@FunctionalInterface 
	public interface MathOperation{
		public int calc(int a,int b);
	}
	
	//在写一个测试类
	public class Test{
		public static void main(String[] args){
			//做加法运算
			doWork(new MathOperation(){
				public int calc(int a,int b){
					return a+b;
				}
			});
			
			//使用Lambda对上面的代码改进
			doWork((int a,int b)->{
					return a+b;
				}
			);
			
			//使用Lambda省略对上面的代码改进
			doWork((a,b)-> a+b);
		}
		
		//写一个方法，把MathOperation函数式接口作为方法的参数
		public void doWork(MathOperation m){
			int result=m.cal(3,4);
			System.out.println("运算结果为："+result);
		}
	}