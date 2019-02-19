# Filter&Listener

## 1.过滤器的概念和入门案例

* 过滤器过滤的对象和作用：web中的过滤器是过滤请求对象和响应对象，可以用于登录验证、统一字符集编码、过滤敏感词汇等通用操作
* 入门案例

```java
@WebFilter("/*")//拦截路径
public class FilterDemo1 implements Filter{
    public void init(FilterConfig config) {}
    public void distory() {}
    public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException{
        System.out.println("过滤器的doFilter方法执行了...");
        //放行
        chain.doFilter(req,ressp);
    }
}
```

## 2.Filter过滤器的相关配置

### 2.1.web.xml配置Filter过滤器

```xml
<filter>
	<filter-name>demo1</filter-name>
    <filter-class>cn.xiaohuang.web.FilterDemo1</filter-class>
</filter>
<filter-mapping>
	<filter-name>demo1</filter-name>
    <!-- 拦截路径 -->
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

### 2.2.过滤器的执行流程和生命周期

```java
生命周期：
	当服务器启动的时候会创建filter对象，执行init方法初始化数据，该方法只执行一次；该方法只执行一次；每当filter拦截到请求时都会执行一次doFilter方法，该方法会执行多次，用于处理拦截之后的操作；当服务器正常关闭时，filter会被销毁，销毁之前会执行destory方法用于释放资源
执行流程：
	在doFilter方法中，拦截到请求之后执行chain.doFilter(req, resp);前面的代码，响应回来之后执行chain.doFilter(req, resp);后面的代码
```

### 2.3.过滤器的url-pattern配置：拦截路径

```java
1.完全路劲匹配：拦截的路径要和写的要一模一样：以/开头，不包含*通配符 例如：/user表示拦截访问user资源
2.目录匹配：以/开头，以/*结尾，拦截的时候*表示任意字符：例如：/user/*表示拦截user目录下的所有资源
3.扩展名匹配：以*开头，固定的后缀名结尾，例如：*.do *.abc表示拦截.do或者.abc结尾的资源
```

### 2.4.过滤器拦截方式配置

```java
@WebFilter(value="/*",dispatcherTypes={DispatcherType.FORWARD,DispatcherType.REQUEST})
DispatcherType.FORWARD:拦截转发的请求
DispatcherType.REQUEST:拦截客户端请求，客户端发过来的请求
```

## 3.过滤器案例

### 3.3.案例1：登录验证

* 案例分析

![1547642657116](C:\Users\ADMINI~1\AppData\Local\Temp\1547642657116.png)

* LoginFilter代码实现

```java
public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException{
    //1.从session中获取manager信息
    HttpServletRequest request = (HttpServletRequest) req;
    Manager manager = (Manager) request.getSession().getAttribute("manager");
    //判断manager是否为null
    if(manager == null) {
        //没有登录，保存错误信息，跳转到登录页面展示错误信息的同时让客户先登录
        request.setAttribute("errorMsg","你还没有登录，请登录后再访问")；
        request.getRequestDispatcher("/login.jsp").forward(request,resp);
    }else {
        //已经登录 放行，运行访问
        //放行
        chain.doFilter(req, resp);
    }
}
```

### 3.2.案例2：统一解决请求和响应中文乱码问题

```java
public void doFilter(ServletRequest req, ServletResponse resp, FilterChain chain) throws ServletException, IOException{
    //解决请求和响应中文乱码
    req.setCharacterEncoding("utf-8");
    resp.setContentType("text")
    //放行
    chain.doFilter(req, resp);
}
```

### 3.3.案例3：过滤敏感词汇

* 案例分析

![1547644405435](C:\Users\ADMINI~1\AppData\Local\Temp\1547644405435.png)

* 动态代理分析

![1547644427427](C:\Users\ADMINI~1\AppData\Local\Temp\1547644427427.png)

* 动态代理代码演示

```java
public class ProxyTest{
    public static void main(String[] args){
        //创建真实对象
        Lenovo lenovo = newLenovo();
        /**2.创建代理对象，摆摊
            * ClassLoader loader:创建代理对象的Class对象，一般和真实对象使用同一个类加载器
            Class<?>[] interfaces:代理对象要和真实对象实现相同的接口
            InvocationHandler h：代理对象的任何方法被调用了，接口实现的invoke方法就执行了，
            */
        ClassLoader loader=lenovo.getClass().getClassLoader();
        Class<?>[] interfaces = lenovo.getClass().getInterfaces();
        Salecomputer proxy = (SaleComputer) Proxy.newProxyInstance(loader, interfaces, new InvocationHandler() {
          /**
            * 代理对象改怎么，代理对象要做的是就是invoke方法的内容
            * @param proxy 代理对象
            * @param method 代理对象的哪个方法被调用
            * @param args 代理对象被调用方法需要的参数值
            * @return
            * @throws Throwable
            */  
            @Override
            public Object invoke(Object proxy,Method method, Object[] args) throws Throwable{
                //System.out.println("invoke...");
                //调用真实对象的对应方法
                String computer = (String) method.invoke(lenovo, args);
                if(computer.contains("联想")) {
                    //"联想"就相当于敏感词汇
                    computer = computer.replace("联想","**")
                }
                return computer;
            }else {
                //通过反射的方式调用真实对象对应的方法
                Object value = method.invoke(lenovo, args);
                return value;
            }
        });
        //调用代理对象的方法
        String computer = proxy.sale(8000);
        proxy.show();
        System.out.println("computer = " + computer);
    }
}
```

* 过滤敏感词汇的过滤器代码实现

```java

```

## 4.监听器

### 4.1.使用思路

```java

```

### 4.2.监听器的分类

```java
1.监听3个域对象的创建、销毁：
	ServletContextListener、ServletRequestListener、HttpSessionListener
2.监听3个域对象存值、移除值、替换值(key一样，将原来的值替换掉)(3个)
    ServletContextAttributeListener、ServletRequestAttributeListener、HTTPSessionAttributeListener
3.监听session域中存javabean的状态变化(绑定/解绑、钝化/活化)(2个)
    绑定：往session域中存javabean对象
    解绑：往session域中移除javabean对象
    	使用的监听器是：HTTPSessionBindingListener
    钝化：javabean对象随着session序列化到硬盘中
    活化：javabean对象随着session反序列化到内存中
    	使用的监听器是：HTTPSessionActionListener
```



