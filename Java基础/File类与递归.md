# File类与递归

## File类：

- 关键单词（file：文件、directory：文件夹、path：路径）

  表示的是文件或者文件夹（目录）抽象路径。路径可以是存在的，也可以是不存在的

### 静态常量

​	由于Java语言在设计的时候是跨平台的，使用下面的常量在不同的操作系统上对路径分隔符有不同的体现

​	pathSeparator:路径分隔符

​		在Windows系统上：“；”

​		在Linux系统上：":"

​	separator:默认名称分隔符（"\"）

​		在Windows系统上："\"

​		在Linux系统上："/"

​	为了让开发的程序具备跨平台性，那么路径分隔符不能写死了

​		`String path1 = "C:\\img\\a.jpg"`//写死了，不跨平台

​		`String path2 = "C:"+File.separator+"img"+File.separator+"a.jpg"`//可以跨平台

### 相对路径和绝对路径

	#### 绝对路径：从盘符开始的路径

#### 相对路径：相对于项目的根目录

### 构造方法

​	用来创建File类的对象，该对象封装了文件\文件夹的路径（可以是存在，也可以是不存在）

​	`public File(String pathname)`

​	`public File (String praent, String child)`

​	`public File(File parent, String child)`

### 常用方法

​	`publicv String getAbsolutePath()`:获取绝对路径

​	`public String getName()`:获取文件名

​	`public String getPath()`:获取File对象封装的路径

​	`public String toString()`:获取File对象封装的路径

​	`public long length()`:获取文件的长度（不能获取文件夹的长度）

​	`public long lastModified()`:获取最后一个修改的时间（毫秒值）

### 判断方法

​	对于一个File对象而言

​	`public boolean isFile()`:它可能是文件（判断是否是文件）

​	`public boolean isDirectory()`:可能是文件夹（判断是否是文件夹）

​	`public boolean exists()`:File对象的路径有可能存在也有可能不存在（判断是否存在）

### 删除的方法

​	`public boolean delete()`注意：只能删除文件或者空的文件夹（有内容的文件夹删除不了）

### 重命名

​	`boolean renameTo(File dest)`

### 文件夹遍历

​	`String[] list()`:获取一个目录下所有的子文件和子文件夹。返回字符串数组（元素是文件或者文件夹的路径）

​	`File[] listFiles()`:获取一个目录下所有的子文件和子文件夹。返回File数组（元素是File对象）

​	注意：

  1. 如果是一个文件调用listFiles()方法，则返回null 

  2. 如果是一个空的目录调用listFiles()方法，则返回数组，但是数组中没元素

     `File[] listFiles(FileFilter filter)`

# 递归

​	就是方法自己调用自己

```java
public class Demo01 {
    public static void main(String[] args) {
        File file = new File("D:\\Program Files\\Java");
        getAllFiles(file);
    }
    public static void getAllFiles(File dir) {
        File[] files = dir.listFiles();
        if(files != null) {
        for (File file : files) {
                if(file.isFile()) {
                    if(file.getName().toLowerCase().endsWith(".java")) {
                        System.out.println(file);
                    }
                } else {
                    getAllFiles(file);
                }
            }
        }
    }
}
```

```java
public class Demo02 {
    public static void main(String[] args) {
        File file = new File("C:\\Users\\Administrator\\Desktop\\day08-code");
    }
    public static void getAllFiles(File dir) {
        File[] files = dir.listFiles(new FileFilter() {
            @Override
            public boolean accept(File pathname) {
                if(pathname.isDirectory()) {
                    return true;
                }
                return pathname.getName().endsWith(".java");
            }
        });
        if(files != null) {
            for (File file : files) {
                if (file.isDirectory()) {
                    getAllFiles(file);
                } else {
                    System.out.println(file);
                }
            }
        }
    }
}
```

```java
public class Demo03 {
    public static void main(String[] args) {
        File file = new File("C:\\Users\\Administrator\\Desktop\\day08-code");
        getAllFiles(file);
    }
    public static void getAllFiles(File dir) {
        File[] files = dir.listFiles(pathname -> {
            if(pathname.isDirectory()) {
                return true;
            }
            return pathname.getName().toLowerCase().endsWith("java");
        });
        if(files != null) {
            for (File file : files) {
                if (file.isDirectory()) {
                    getAllFiles(file);
                } else {
                    System.out.println(file);
                }
            }
        }
    }
}
```

