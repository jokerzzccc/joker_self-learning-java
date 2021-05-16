# Day07-IO

# 流

- 概念：内存与存储设备之间传输数据的通道
- 分类：
  - 按方向（重点）：
    - 输入流：将<存储设备> 中的内容读入到<内存>中
    - 输出流：将<内存>中的内容写入到<输出设备>中
  - 按单位：
    - **字节流**：以字节为单位，可以读写所有数据。
    - **字符流**：以字符为单位，只能读写文本数据。
  - 按功能：
    - **字节流**：具有实际传输数据的读写功能。
    - **过滤流**：在节点流的基础之上增强功能。

## 字节流

### 字节流的**父类**（抽象类）：

###### InputStream:(字节输入流)

- 方法：read(),close(),...

###### OutputStream：（字节输出流）

- 方法：write(),close(),...

### 字节流的**子类**

#### 文件字节流

##### **FileInputStream**

- public int read()`//从输入流中读取一个字节数据，返回读到的字节数据，如果达到文件末尾，返回-1。

- public int read(byte[] b)`//从输入流中读取字节数组长度的字节数据存入数组中，返回实际读到的字节数；如果达到文件的尾部，则返回-1。

- ```java
  /**
   * FileInputStream
   *
   * @author joker_chen
   */
  public class Demo01 {
      public static void main(String[] args) throws Exception {
          //创建FileInputStream，并指定文件路径
          FileInputStream fileInputStream = new FileInputStream("D:\\2021java学习文件\\mdFile\\file.txt");
  
          //读取文件
          fileInputStream.read();
  //        //2.1 读取单个字节
  //        int data = 0;
  //        read()，读取到没有的时候会返回-1
  //        while ((data =fileInputStream.read()) != -1){
  //            System.out.print((char) data);
  //        }
          //2.2 一次读取多个字节
  
          byte[] bytes = new byte[1024];
          int count =  0;
          while ((count = fileInputStream.read(bytes)) != -1){
              System.out.println(new String(bytes,0,count));
          }
  
          //关闭
          fileInputStream.close();
          System.out.println("");
          System.out.println("over");
      }
  }
  
  ```


##### FileOutputStream

- public void write(int b)`//将指定字节写入输出流。

- public void write(bute[] b)`//一次写多个字节，将b数组中所有字节，写入输出流。

- ```java
  /**
   * FileOutputStream
   *
   * @author joker_chen
   */
  public class Demo02 {
      public static void main(String[] args) throws Exception {
          FileOutputStream fileOutputStream = new FileOutputStream("D:\\2021java学习文件\\mdFile\\file.txt",true);
          //2 写入文件
          //2.1单个写入
  //        fileOutputStream.write(   97);
  //        fileOutputStream.write('b');
  //        fileOutputStream.write('c');
          //2.2 一次性写入多个字符，就是调用不同的write()
          String str = "test";
          fileOutputStream.write(str.getBytes());
              //每次执行都会覆盖原有的文本，若要不覆盖，就在构造方法里，append 设置为 true，变成追加
  
          fileOutputStream.close();
          System.out.println("excute over");
  
      }
  }
  ```

##### 字符文件流案例：复制文件

```java
/**
 *文件字节流案例：复制文件
 * @author joker_chen
 */
public class Demo03 {
    public static void main(String[] args) throws  Exception{
        FileInputStream fileInputStream = new FileInputStream("D:\\2021java学习文件\\mdFile\\file.txt");
        FileOutputStream fileOutputStream = new FileOutputStream("D:\\2021java学习文件\\mdFile\\file2.txt");
        //一边读，一边写
        byte[] bytes = new byte[1024];
        int count = 0;//count保存实际读取的个数
        while ((count = fileInputStream.read(bytes)) != -1){
            fileOutputStream.write(bytes,0,count);//防止文件大于bytes，就要设置off(起始)，len(结束长度)，的值
        }

        fileInputStream.close();
        fileOutputStream.close();
        System.out.println("duplicate over");
    }
}

```

#### 字节缓冲流

- 提高IO效率，减少访问磁盘的次数；
- 数据存储在缓冲区中。flush可以将缓存区的内容写入文件，也可以直接close。

##### BufferedInputStream

```java
/**
 * BufferedInputStream
 * @author joker_chen
 */
public class Demo04 {
    public static void main(String[] args) throws Exception {
        FileInputStream file = new FileInputStream("D:\\2021java学习文件\\mdFile\\file.txt");
        BufferedInputStream buffer = new BufferedInputStream(file);//buffer默认大小是8192

        //读取
//        int data = 0;
//
//        while ((data = buffer.read()) != -1){
//            System.out.println((char) data);
//        }
        byte[] buf = new byte[1024];//自定义buffer的大小
        int count = 0;
        while ((count = buffer.read(buf)) != -1){
            System.out.println(new String(buf,0,count));
        }


        buffer.close();//只需关闭buffer,前面的file也会被关闭
    }
}
```



##### BufferedOutputStream

```java
/**
 * BufferedOutputStream
 * @author joker_chen
 *
 */
public class Demo05 {
    public static void main(String[] args) throws Exception {
        FileOutputStream file_out = new FileOutputStream("D:\\2021java学习文件\\mdFile\\buffer.txt");
        BufferedOutputStream buffer_out = new BufferedOutputStream(file_out);
        //写入文件
        for (int i = 0; i < 10; i++) {
            buffer_out.write("hello\r".getBytes());//写入8k缓冲区,而在硬盘里看不见，
            buffer_out.flush();//每执行一次，就刷新一次，刷新到硬盘，防止数据的丢失
        }
        //关闭，调用close() 时，内部会自动调用 flush(),
        buffer_out.close();
    }
}
```

#### 对象流

- 增强了缓冲区功能
- 增强了读写8种基本数据类型和字符串功能
- 增强了读写对象的功能：
  - readObjec() 从流中读取一个对象，反序列化
  - WriteObject(Object obj) 向流中写入一个对象，序列化

- 使用流传输对象的过程，称为序列化、反序列化

创建对象类（Student）

```java
/**
 * Student 类
 * @author joker_chen
 * 要实现序列化，必须实现 Serializable 接口，里面啥也没有，写上就行
 */
public class Student implements Serializable {

    private static final long serialVersoinUID = 100L;//serialVersoinUID 序列化版本号ID

    private String name;
    private int age;

    public Student(){}

    public Student(String name, int age) {
        this.name = name;
        this.age = age;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    @Override
    public String toString() {
        return "Student{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```



###### ObjectOutpuStream (序列化)

```java
/**
 * 使用 ObjectOutputStream 实现序列化（写入）
 * 注意事项：
 * （1）序列化类（Student）必须实现 Serializable 接口
 * （2）序列化类中的，对象属性，要求实现 Serializable 接口
 * （3）序列化版本号 ID，保证序列化的类和反序列化的类是同一个类
 * （4）使用 transient (瞬间的) 修饰属性，这个属性不能序列化：比如 public transient int age;
 * （5）静态属性不能序列化
 * （6）序列化多个对象，可以借助集合（如：ArrayList）实现
 *
 */
public class Demo06 {
    public static void main(String[] args) throws Exception {
        FileOutputStream fos = new FileOutputStream("D:\\2021java学习文件\\mdFile\\serial.txt");
        ObjectOutputStream oos = new ObjectOutputStream(fos);

        //序列化：写入操作
        Student joker = new Student("joker", 20);
        oos.writeObject(joker);

        oos.close();
        System.out.println("serializable over");

    }
}
```

###### ObjectInputStream(反序列)

```java
/**
 * 使用 ObjecInputSteam 实现反序列化 （读取重构成对象）
 *
 * @author joker_chen
 */
public class Demo07 {
    public static void main(String[] args) throws Exception {
        FileInputStream fis = new FileInputStream("D:\\2021java学习文件\\mdFile\\serial.txt");
        ObjectInputStream ois = new ObjectInputStream(fis);

        //读取文件（反序列化）
        Student stu = (Student) ois.readObject();
//        Student stu2 = (Student) ois.readObject();//不能读多个

        ois.close();
        System.out.println("over");

        System.out.println(stu.toString());

    }
}
```

###### 序列化和反序列化注意事项

- ```
  * （1）序列化类（Student）必须实现 Serializable 接口
  * （2）序列化类中的，对象属性，的对象类，要求实现 Serializable 接口
  如：public Person obj;中的 Person 类要实现 Serializable 接口
  * （3）序列化版本号 ID，添加在序列化类中，保证序列化的类和反序列化的类是同一个类
  如：private static final long serialVersionUID = 100L;
  
  * （4）使用 transient (瞬间的) 修饰属性，这个属性不能序列化：比如 public transient int age;
  * （5）静态属性不能序列化，
  如：public static int age;
  * （6）序列化多个对象，可以借助集合（如：ArrayList）实现
  如：
  序列化：
  ArrayList<Student> arrayList=new ArrayList<Student>();
  arrayList.add(stu1);
  arrayList.add(stu2);
  oos.writeObject(arrayList);
  
  反序列化：
  ArrayList<Student> list=(ArrayList<Student>)objectInputStream.readObject();
  System.out.println(list.toString());
  
  ```

## 字节编码

- IOS-8859-1

  收录除ASCII外，还包括西欧、希腊语、泰语、阿拉伯语、希伯来语对应的文字符号。采用1个字节来表示，最多只能表示256个字符。

- UTF-8（重点）

  针对Unicode码表的可变长度字符编码。国际上使用的编码，也称为“万国码”，收录了几乎所有国家的常用字符。采用1至3个字节来表示一个字符。

- GB2312

  简体中文，采用1个或2个字节来表示字符，95年之前所采用的编码。

- GBK

  简体中文的扩充，GB2312的升级版本。

- BIG5

  台湾，繁体中文。

## 字符流

- 使用字节流读取中文字符的时候会出现乱码，因，汉字，要用3个字节表示，而字节流是用一个字节去读取一个汉字，此时就要用字符流

### 字符流的父类（抽象）

###### Reader 

- 方法：

  - public int read()`

    从流中读取单个字符，用整型来返回读取的字符；当读到流底部时返回-1

  - public int read(char[] c)`

    从流中读取字符保存到c数组中，返回读取的字符个数，当读到流底部时返回-1

  - public int read(char[] cbuf,int off,int len){} 

    抽象方法

###### Writer 

- 方法：

  - `public void write(int n)`

    写入单个字符，只能写入包含16位低阶字节的整型数值，16位高阶字节将会被忽略

  - `public void write(String str)`

    写入一个字符串

  - `public void write(char[] cbuf)`

    写入一个字符数组

### 字符流的子类

#### 文件字符流

##### FileReader 类

- public int read(char[] c) //从流中读取多个字符，将读到内容存入c数组，返回实际读到的字符数；如果达到文件的尾部，返回-1

```java
/**
 * 字符流，FileReader
 * @author joker_chen
 *
 */
public class Demo08 {
    public static void main(String[] args) throws Exception{
        FileReader fr = new FileReader("D:\\2021java学习文件\\mdFile\\ziFuLiu.txt");

        //读取
//        //2.1单个字符读取
//        int data = 0;
//        while ((data = fr.read()) != -1){
//            System.out.println((char) data);
//        }
        //2.2自建一个缓冲区，一次性读取
        char[] buf = new char[1024];
        int count = 0;
        while ((count = fr.read(buf)) != -1){
            System.out.println(new String(buf,0,count));
        }
        fr.close();
    }
}
```



##### FileWriter 类

- public void write(String str) //一次写多个字符，将b数组中所有字符，写入输出流

```java
/**
 * FileWriter
 * @author joker_chen
 */
public class Demo09 {
    public static void main(String[] args) throws Exception{
        FileWriter fr = new FileWriter("D:\\2021java学习文件\\mdFile\\fw.txt");
        //写入
        for (int i = 0; i < 10; i++) {
            fr.write("java\r");
            fr.flush();
        }
        
        fr.close();
    }
}

```

##### 字符流案例：复制文件

- 字符流只能复制文本文件，不能复制图片或二进制文件，因为图片没有字符编码
- 字节流可以复制任何文件

```java
/**
 * 使用FileReader 和FileWriter 复制文本文件。只能复制文本文件，不能复制图片或二进制文件，因为图片没有字符编码
 * 字节流可以复制任意文件
 * @author joker_chen
 */
public class Demo10 {
    public static void main(String[] args) throws Exception {
        //创建
        FileReader fr = new FileReader("D:\\2021java学习文件\\mdFile\\fw.txt");
        FileWriter fw = new FileWriter("D:\\2021java学习文件\\mdFile\\fw2.txt");
        //读写
        int data = 0;
        while ((data = fr.read()) != -1){
            fw.write(data);
            fw.flush();
        }
        fr.close();
        fw.close();
    }
}
```

#### 字符缓冲流

-  缓冲流：BufferedReader/BufferedWriter
  - 高效读写
  - 支持输入换行符
  - 可一次写一行、读一行

###### BufferedReader

```java
/**
 * 使用字符缓冲流读取文件
 * BufferedReader
 *
 * @author joker_chen
 */
public class Demo11 {
    public static void main(String[] args) throws Exception{
        //创建缓冲流
        FileReader fr = new FileReader("D:\\2021java学习文件\\mdFile\\fw.txt");
        BufferedReader br = new BufferedReader(fr);
        //读取
//        //第一种方式
//        char[] buf = new char[1024];
//        int count = 0;
//        while ((count = br.read(buf)) != -1){
//            System.out.println(new String(buf,0,count));
//
//        }
        //第二种方式,一行一行的读取
        String line = null;
        while ((line = br.readLine()) != null){
            System.out.println(line);
        }

        br.close();
    }
}
```

###### BufferedWriter

```java
/**
 * BufferedWriter的使用
 * @author joker_chen
 */
public class Demo12 {
    public static void main(String[] args) throws Exception{
        //创建
        FileWriter fw = new FileWriter("D:\\2021java学习文件\\mdFile\\bw.txt");
        BufferedWriter bw = new BufferedWriter(fw);
        //写入
        for (int i = 0; i < 10; i++) {
            bw.write("好好学习，天天向上");
            bw.newLine();//写入一个换行符
            bw.flush();
        }

        bw.close();
    }
}
```

#### 打印流(PrintWriter)

```java
/**
 * PrintWriter 的使用
 *
 * @author joker_chen
 */
public class Demo13 {
    public static void main(String[] args) throws Exception{
        //创建
        PrintWriter pw = new PrintWriter("D:\\2021java学习文件\\mdFile\\pw.txt");
        //打印
        pw.println(97);//打印的是什么样，文件里就是什么样
        pw.println(true);
        pw.println(3.14);
        pw.println('a');

        pw.close();
        System.out.println("over");

    }
}
```

#### 转换流

- 也称桥转换流：InputStreamWriter/OutputStreamWriter
  - 可将字符流转换为字节流
  - 可设置字符 的编码方式
  - 注：硬盘里是字节存储，内存里是字符存储

##### InputStreamWriter

```java
**
 * 转换流 InputSteamReader 的使用，可以指定使用的编码
 * @author joker_chen
 *
 */
public class Demo14 {
    public static void main(String[] args) throws Exception {
        //创建
        FileInputStream fis = new FileInputStream("D:\\2021java学习文件\\mdFile\\pw.txt");
        InputStreamReader isr = new InputStreamReader(fis, StandardCharsets.UTF_8);

        //读取
        int data = 0;
        while ((data = isr.read()) != -1){
            System.out.print((char) data);
        }

        isr.close();

    }
}
```

##### OutputStreamWriter

```java
/**
 * OutputStreamWriter 写入文件，指定编码
 *
 */
public class Demo15 {
    public static void main(String[] args) throws Exception{
        FileOutputStream fos = new FileOutputStream("D:\\2021java学习文件\\mdFile\\fos.txt");
        OutputStreamWriter osw = new OutputStreamWriter(fos, "utf-8");
        //写入
        for (int i = 0; i < 10; i++) {
            osw.write("我是joker\n");
            osw.flush();
        }

        osw.close();

    }
}
```

# File类

- 概念：代表物理盘符中的 一个文件或者文件夹
- 方法：
  - createNewFile()
  - mkdir() 创建一个新目录
  - delete()
  - exists()
  - getAbsolutePath()
  - getName()
  - getParent()
  - isDirectory()
  - isFile()
  - length()
  - listFiles()列举目录中所有内容
  - renameTo()修改文件名
- File类的使用：
  - 分隔符
  - 文件操作
  - 文件夹操作

## 分隔符

- ```java
  public class Demo01 {
      public static void main(String[] args) throws Exception{
  //        separator();
  //        fileOperation();
          directoryOperation();
      }
      //分隔符
  
      public static void separator(){
          System.out.println("路径分隔符" + File.pathSeparator );
          System.out.println("名称分隔符" + File.separator);
      }
  ```

  

## 文件操作

```java
//文件操作
    public static void fileOperation() throws Exception{
        //创建文件 file.createNewFile()
        File file = new File("D:\\2021java学习文件\\mdFile\\File_text.txt");//创建了文件对象，但实际上没有文件
        if (!file.exists()){//如果文件不存在，就创建
            boolean b = file.createNewFile();//如果文件已经存在，就会返回false，并不创建文件
            System.out.println("result: " + b);
        }

        //删除文件
        //2.1 直接删除
//        System.out.println("delete result: " + file.delete());
        //2.2 使用jvm退出时删除
//        file.deleteOnExit();
//        Thread.sleep(5000);

        //获取文件信息
        System.out.println("获取文件绝对路径：" + file.getAbsolutePath());
        System.out.println("获取路径" + file.getPath());//写什么路径就是什么路径
        System.out.println("获取文件名称"+ file.getName());
        System.out.println("获取父目录" + file.getParent());
        System.out.println("获取文件长度" + file.length());
        System.out.println("文件创建时间" + new Date(file.lastModified()));//本来是long 的时间，转换成date

        //判断
        System.out.println("是否可写：" + file.canWrite());
        System.out.println("是否是文件" + file.isFile());
        System.out.println("是否隐藏" + file.isHidden());
    }
```



## 文件夹操作

```java
//文件夹操作
    public static void directoryOperation() throws Exception{
        //1 创建文件夹
        File dir = new File("d:\\aaa\\bbb\\ccc");
        if (! dir.exists()){
            //dir.mkder() //只能创建单级目录
            System.out.println("创建结果： "+dir.mkdirs());//创建多级目录
        }

        //2 删除
        //2.1 直接删除
//        System.out.println("删除结果： " + dir.delete());//只能删除最底层的目录，且只能删除空目录
        //2.2 使用jvm删除
//        dir.deleteOnExit();
//        Thread.sleep(5000);//只是为了能够看见删除的过程，过5s之后再删除

        //3 获取文件夹信息
        System.out.println("获取绝对路径：" + dir.getAbsolutePath());
        System.out.println("获取路径" + dir.getPath());//写的什么路径就是什么路径
        System.out.println("获取文件名称 " + dir.getName());//获取最底层的文件名称
        System.out.println("获取父目录 " + dir.getParent());
        System.out.println("获取创建时间 " + new Date(dir.lastModified()).toLocaleString());

        //4 判断
        System.out.println("是否是文件夹"+dir.isDirectory());
        System.out.println("是否是隐藏" + dir.isHidden());

        //5 遍历文件夹
        System.out.println("============");
        File dir2 = new File("D:\\2021java学习文件\\learnphoto");

//        String[] list = dir2.list();//可以用list() 方法，返回一个字符串数组，也可以用ListFiles() 方法，返回一个File数组
//        for (String str :
//                list) {
//            System.out.println(str);
//        }
        File[] dir3 = dir2.listFiles();
        for (File file :
                dir3) {
            System.out.println(file.getName());
        }

    }
}
```

## 案例1：递归遍历文件夹

## 案例2：递归删除文件夹

```java
/**
 * 案例1：递归遍历文件夹
 * 案例2：递归删除文件夹
 * @author joker_chen
 */
public class Demo02_Case {
    public static void main(String[] args) {
//        listDir(new File("D:\\aaaa_text"));
        deleteDir(new File("D:\\aaaa_text"));
    }
    //递归遍历文件夹：先递归文件夹，再递归文件
    public static void listDir(File dir){
        File[] files = dir.listFiles();//得到的是，aaaa_text 下一级的文件夹及文件
        System.out.println(dir.getAbsoluteFile());//打印文件夹
        if (files != null && files.length > 0){//文件夹不为空（文件夹存在），且，文件长度不为0（即有文件）
            for (File file:
                 files) {
                if (file.isDirectory()){
                    listDir(file);//递归
                } else {
                    System.out.println(file.getAbsoluteFile());//打印文件
                }
            }

        }
    }
    //案例2：递归删除文件夹，先删除文件，再删除文件夹
    public static void deleteDir(File dir){
        File[] files = dir.listFiles();
        if (files != null && files.length > 0){
            for (File file :
                    files) {
                if (file.isDirectory()){
                    deleteDir(file);//遍历
                } else {
                    //删除文件
                    System.out.println(file.getAbsoluteFile() + "删除: " + file.delete());
                }
            }
        }
        //删除文件夹
        System.out.println(dir.getAbsoluteFile() + "删除： " + dir.delete());
    }
}
```



# FileFilter接口

- 只有一个方法：

  - ```
    boolean accept(File pathname)
    ```

- 当调用File类中的 listFiles() 方法时，支持传入 FileFilter 接口的实现类，对获取文件进行过滤，只有满足条件的文件才可以出现在 listFiles() 的返回值中

```java
//FileFilter 的使用
        System.out.println("============FileFilter==========");

        File dir4 = new File("E:\\精选image");
        File[] files = dir4.listFiles(new FileFilter() {
            @Override
            public boolean accept(File pathname) {
                if(pathname.getName().endsWith(".png"))
                    return true;
                return false;//false,就是默认所有的都不接受
            }
        });
        for (File file:
             files) {
            System.out.println(file.getName());
        }
```

# Properties

- Properties: 属性集合，继承了 HashTable ，是一个线程安全的集合
- 特点：
  - 1 存储属性名和属性值
  - 2 属性名和属性值都是字符串类型
  - 3 没有泛型
  - 4 和流有关
- 常用方法：

```java
/**
 * Properties 集合的使用
 *
 * @author joker_chen
 */
public class Demo01 {
    public static void main(String[] args) throws Exception{
        //1 创建
        Properties properties = new Properties();
        //添加数据
        properties.setProperty("username","zhangsan");
        properties.setProperty("age","20");

        System.out.println(properties.toString());
        //2 删除
        //3 遍历
        //3.1 使用 keySet
        //3.2 使用 entrySet
        //3.3 使用 stringPropertyNames()
        Set<String> propertyNames = properties.stringPropertyNames();
        for (String pro :
                propertyNames) {
            System.out.println(pro + "-----" + properties.getProperty(pro));
        }
        //4 和流有关的方法
//        //4.1 list()
//        PrintWriter pw = new PrintWriter("D:\\2021java学习文件\\mdFile\\pw.txt");
//        properties.list(pw);//打印到一个打印流里
//        pw.close();

//        //4.2 store() 保存
//        //属性集合的文件扩展名一般用 .properties 这种文件不能有中文
//        FileOutputStream fos = new FileOutputStream("D:\\2021java学习文件\\mdFile\\store.properties");
//        properties.store(fos,"注释");//有中文就用字节流的 store，没中文就用字符流的
//        fos.close();

        //4.3 load() 加载,重点
        Properties properties2 = new Properties();
        FileInputStream fis = new FileInputStream("D:\\2021java学习文件\\mdFile\\store.properties");
        properties2.load(fis);
        fis.close();
        System.out.println(properties2.toString());


    }
}
```



