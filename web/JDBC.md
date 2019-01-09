# JDBC

## 1. JDBC概念和入门程序

* 概念：jdbc是sun公司定义的一套操作数据库的规范，也就是接口。各个数据库生产商实现了这一堆接口，我们需要使用哪个数据库就用对应的驱动包即可，驱动包中都是jdbc接口的实现类

* 入门案例：

  ```java
  public class JdbcDemo{
      public static void main(String[] args) throws Exception{
          //1.导入驱动jar包
          //2.注册驱动
          Class.forName("com.mysql.jdbc.Driver");
          //3.获取数据库连接对象 假设获取连接的时候要Driver对象
          Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3","root","123");
          //4.定义sql语句
          String sql="update account set balance=2000";
          //5.获取执行sql的对象 statement
          Statement stat = conn.createStatement();
          //6.执行sql
          int count = stat.executeUpdate(sql);
          //7.处理结果
          System.out.println(count);
          //8.释放资源
          stat.close();
          conn.close();
      }
  }
  ```



## 2. JDBC中API讲解

* java.sql.DriverManager驱动管理类

  ```java
  1.注册驱动：DriverManager.registerDriver(new Driver());但是我们一般不使用这个方法，而是使用：Class.forName("com.mysql.jdbc.Driver");将核心的Driver驱动类加载进内存，从而注册驱动;
  
  2.获取连接：Connection conn = DriverManager.getConnection("jdbc:mysql://localhost:3306/db3","root","123");
  参数1：连接数据的url路径：jdbc:mysql://localhost:3306/数据库名称;如果连接的是本机数据库，那么可以简写成jdbc:mysql:///数据库名称
  参数2：用户名
  参数3：密码
  ```

* java.sql.Coeannection对象

  ```java
  1.获取执行sql的对象
  	Statement createStatement()  (有漏洞！！！)
      PreparedStatement preparedStatement(String sql)
  2.管理事务：
  	开启事务：setAutoCommit(boolean autoCommit):调用该方法设置参数为false,即开启事务
  	提交事务：commit();
  	回滚事务：rollback();
  ```

* java.sql.Statement对象

  ```java
  作用：执行sql操作
  boolean flag = statement.execute(sql);如果执行的是查询操作，返回值为true，否则返回值是false;一般不用
  int row=statement.executeUpdate(sql);一般用来执行insert、update、delete语句，返回值是影响的行数，大于0说明执行成功
  ResultSet rs= statement.executeQuery(sql);一般用来执行select语句，返回值是一个封装查询结果的对象，这个对象通常叫做结果集对象
  ```

* java.sql.ResultSet结果集对象

```sql
作用：遍历获取结果
//循环判断是否有下一行数据，如果next()方法就返回true；
while(rs.next()){
	//获取数据
	int id = rs.getInt(1);//获取第一列结果，编号从1开始  比较少用
	String name = rs.getString("name");//根据列名获取结果 很常用
	double balance = rs.getDouble("balance");
	System.out.print(id + "------" + name + "-----------" + balance);
}
```

**注意：如果值int类型的数据，也可以使用rs.getString(列名)获取；getInt("1")表示获取列名叫1的值**

## 3. 使用Statement对象执行增、删、改、查操作

* 执行增、删、改

```sql
1. 加载驱动
2. 获取连接、Connection对象
3. 通过Connection对象获取执行sql的Statement对象
4. 执行增删改sql语句，结果是int类型的影响行数
5. 处理结果
6. 释放资源
```

* 执行查询

```sql
1. 加载驱动
2. 获取连接、Connection对象
3. 通过Connection对象获取执行sql的Statement对象
4. 执行查询语句，得到ResultSet结果集对象
5.  遍历结果集，获取每一行数据，可以封装到对象中
6. 释放资源
```

## 4.JDBCUTILs工具类

```sql
public class JDBCUtils{
	//声明连接参数
	public static String url;
	public static String user;
	public static String password;
	public static String dirver;
	//1.注册驱动只需要注册一次，那么可以放在static代码块中
    static{
    	//1.加载src中配置文件jdbc.properties
    	//1.1.创建Properties对象
    	Properties properties = new Properties();
    	//1.2.调用load方法
    	InputStream is = JDBCUtils.class.getClassLoader().getResourceAsStream("jdbc.properties");
        try{
        	properties.load(is);
        	//1.3.获取参数
        	driver = properties.getProperty("driver");
        	url = properties.getProperty("url");
        	user = properties.getProperty("user");
        	password = properties.getProperty("password");
        	//2.注册驱动
        	Class.forName(driver);
            } catch(Exception e) {
            	e.printStackTrace();
            }
    }
    //2.对外提供一个获取连接的static方法
    public static Connection getConnection() throws SQLException{
    	return DriverManager.getConnection(url,user,password);
    }
    public static void close(Statement stmt, Connection conn) {
    	close(null, stmt, conn)
    }
    public static void close(ResultSet rs, Statement stmt, Connection conn) {
    	if(rs != null) {
        try{
        	rs.close();
            } catch(SQLException e) {
            	e.printStackTrace();
            }
    	}
    	if(stmt != null) {
        try{
        	stmt.close();
            } catch(SQLException e) {
            	e.printStackTrace();
            }
    	}
    	if(conn != null) {
        try{
        	conn.close();
            } catch(SQLException e) {
            	e.printStackTrace();
            }
    	}
    }
}
```

* 使用注解代替配置文件

  * 定义注解

  ```sql
  @Target(value={ElementType.TYPE,ElementType.Method})
  @Retention(RetentionPolicy.RUNTIME)
  public @interface JdbcAnno{
  	String driver() default "com.mysql.jdbc.Driver";
  	String url() default "jdbc:mysql://localhost:3306/mydb4";
  	String user() default "root";
  	String password() default "123";
  }
  ```

  * 工具类代码

  ```java
  @JdbcAnno(driver="com.mysql.jdbc.Driver")//其他属性也可以赋值，不赋值都有默认值
  public class JDBCUtils{
      //声明连接参数
      public static String url;
      public static String user;
      public static String password;
      public static String driver;
      //1.注册驱动只需要注册一次，那么可以放在static代码块中
      static{
          //1.加载注解中的参数
          JdbcAnno jdbcAnno = JDBCUtils.class.getAnnotation(JdbcAnno.class);
          //获取注解属性值
          driver=jdbcAnno.driver();
          url=jdbcAnno.url();
          user=jdbcAnno.user();
          password=jdbcAnno.password();
          //2.注册驱动
          try{
              Class.forname(driver);
          } catch(ClassNotFoundException e) {
              e.printStackTrace();
          }
      }
  }
  ```

## 5.使用PreparedStatement对象执行增、删、改、查操作

* sql注入漏洞

```sql
原因：使用Statement对象执行sql操作时，如果sql中拼接的变量包含了sql的关键字，那么就会出现sql注入漏洞
解决：不使用Statement对象，使用PreparedStatement对象执行sql操作；
使用PreparedStatement对象思路：
	1.通过Connection获取PreparedStatement对象时需要传递sql语句；sql语句中可能包含?占位符
	2.如果有?占位符，那么就需要使用setXxx()设置?占位符对应的参数；
	3.在真正执行sql操作的使用不需要再传递sql语句 pstmt.executeQuery();
PreparedStatement对象好处：
	1.防止sql注入漏洞
	2.提高sql执行效率
```

* **执行增、删、tt改操作**

```sql
1.注册驱动
2.获取连接、Connection对象
3.通过Connection对象获取PrepareStatement对象，预编译sql语句
4.如果sql语句中有?占位符，那么就需要设置?代替的参数值
5.真正执行增删改操作：int row = pstmt.executeUpdate();
6.处理结果
7.释放资源
```

* ##### 执行查询操作

```sql
1.注册驱动
2.获取连接、Connection对象
3.通过Connection对象获取PreparedStatement对象，预编译sql语句
4.如果sql中有?占位符，那么就需要设置?代替的参数值
5.真正执行查询操作：ResultSet rs = pstmt.executeQuery();
6.处理结果集，变量获取数据
7.释放资源
```

## 6.jdbc事务管理

* 事务管理api

```sql
概念：当一组sql操作要么同时成功要么同时失败，这个时候就使用事务管理
开启事务：setAutoCommit(boolean autoCommit):调用该方法设置参数为false,即开启事务，一般是执行sql之前开始事务，或者获取到连接对象之后开启事务
提交事务：commit():一般是sql都执行成功之后提交事务
回滚事务：rollback():一般在catch中回滚事务，而且catch是捕获最大的Exception异常
```

* 案例

```sql
//事务操作：转账案例
public class JDBCDemo{
	public static void main(String[] arsg) {
		Connection conn = null;
		PreparedStatement pstmt = null;
        try{
        	//1.获取连接对象
        	conn = JDBCUtils.getConnection();
        	//开启事务
        	conn.setAutoCommit(false);
        	//2.获取PreparedStatement对象，预编译sql语句
        	pstmt = conn.prepareStatement("UPDATE account SET balance=balance+? WHERE id=?")
        	//3.如果sql语句中有?占位符，那么就设置参数
        	pstmt.setDouble(1,-500);
        	pstmt.setInt(2,1);
        	//4.执行sql操作
        	pstmt.executeUpdate();
        	pstmt.setDouble(1,500);
			pstmt.setInt(2,2);
			pstmt.executeUpdate();
			//5.处理结果
			//提交事务
			conn.commit();
            } catch (Exception e) {
            	e.printStackTrace();
            	System.out.println("事务回滚了......");
            	if (conn!=null){
					try {
					conn.rollback();
					} catch (SQLException e1) {
						e1.printStackTrace();
					}
				}
                }finally{
                	JDBCUtils.close(pstmt,conn);
            }
	}
}
```

## 7. JDBC连接池

* 概念：一个装有连接对象的容器

![1544939865152](C:\Users\ADMINI~1\AppData\Local\Temp\1544939865152.png)

* ###### 实现原理：所有连接池对象都实现了DataSource,所有连接池都有getConnection()方法获取连接对象；connection的close()方法不是释放资源而是归还连接对象到连接池中

* ###### c3p0连接池

```sql
使用思路：
	1.导入开发jar包：：c3p0-0.9.5.2jar和mchange-commons-java-0.2.12.jar 不要忘了mysql驱动jar包
	2.将c3p0-config.xml配置文件复制到src中，并修改连接信息（jdbcUrl、user、password）
	3.创建连接池对象：CombopooledDataSource ds=new CombopooledDataSource();
	4.获取连接：Connection conn=ds.getConnection();
```

##### 注意事项：1.配置文件必须放到src目录中；2.配置文件名称必须是c3p0-config.xml

* ##### druid连接池

```sql
使用思路
	1.导入开发jar包：druid-1.0.9.jar、不要忘了mysql的驱动jar包
	2.将配置文件复制到src目录中，一般是放在src目录中，可以使用类加载器加载配置文件
	3.通过Properties对象加载属性文件，并使用druidDataSourceFactory工厂创建连接池对象
		//创建Properties对象
		Properties pro = new Properties();
		InputStream is = DruidDemo.class.getClassLoader().getResourceAsStream("druid.properties");
		pro.load(is);
		//获取连接池对象
		DataSource ds = DruidDataSourceFactory.createDataSource(pro);
	4.获取连接：Connection conn = ds.getConnection();
```

* ##### 修改JDBCUtils工具类

```sql
public class JDBCUtils{
	private static Datasource ds;
	//1.注册驱动只需要注册一次，那么可以放在static代码块中
    static{
    	//创建Properties对象，加载配置文件
    	Properties properties = new Properties();
        try{
     properties.load(JDBCUtils.class.getClassLoader().getResourceAsStream("druid.properties"));
			//初始化连接池对象
			ds = DruidDataSourceFactory.createDataSource(properties);
            } catch(IOException e) {
            	e.printStackTrace();
                } catch(Exception e) {
                	e.printStackTrace();
                }
    }
    //2.对外提供一个获取连接池的方法
    public static DataSource getdataSource() {
    	return ds;
    }
    //3.对外提供获取连接的static方法
    public static Connection getConnection() throws SQLException{
    	//从连接池中获取连接
    	return ds.getConnection();
    }
}
```

## 8.JJDBCTemplate工具类

* 作用：大大简化jdbc操作

* ##### 实现增、删、改

```sql
//1.创建JDBCTemplate对象
JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//参数是连接池对象
//2.调用update方法
int row=template.update(String sql,Object...params);//参数1：sql语句；参数2：代替?的参数
```

* ##### 查询一条数据，封装成javabean对象

```sql
//1.创建JDBCTemplate对象
JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//参数是连接池对象
//2.调用queryForObject(String sql, new BeanPropertyRowMapper<Emp>(Emp.class),代替?的可变参数)
类型 对象名 = template.queryForObject("select * from 表名 where id=?", new BeanPropertyRowMapper<类型>(类型.class),1002)；
```

* ##### 查询多条数据，封装成List<javabean>对象

```sql
//1.创建JDBCTemplate对象
JdbcTemplate template = new JdbcTemplate(JDBCUtils.getDataSource());//参数是连接池对象
//2.调用queryForObject(String sql, new BeanPropertyRowMapper<Emp>(Emp.class),代替?的可变参数)
List<java类> list = template.query(sql, new BeanPropertyRowMapper<类型>(类型.class),3000);
```

* BeanPropertyRowMapper对象的作用：将一条查询记录封装成自己的javabean对象

* ##### 什么时候使用BeanPropertyRowMapper对象，什么时候使用xxx.class对象

```sql
BeanPropertyRowMapper将多列数据封装到自定义javabean的各个属性身上，传递xxx.class字节码对象只能将一列数据封装成对应列的数据类型
```

