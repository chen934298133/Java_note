## &#127800; 1. 流的概念
内存与存储设备之间传输数据的通道

## &#127800; 2. 流的分类
1. 按方向
    - 输入流：将<存储设备>中的内容读到<内存>中
    - 输出流：将<内存>中的内容写到<存储设备>中
2. 按单位
    - 字节流：以字节为单位，可以读写所有数据
    - 字符流：以字符为单位，只能读写文本数据
3. 按功能
    - 节点流：具有实际传输数据的读写功能
    - 过滤流：在节点流的基础之上增强功能
    
## &#127800; 3. 字节流

```java
//InputStream 字节输入流
public int read(){}
public int read(byte[] b){}
public int read(byte[] b, int off, int len){}

// OutputStream 字节输出流
public void write(int n){}
public void write(byte[] b){}
public void write(byte[] b, int off, int len){}
```

### &#127803; 3.1 文件字节流
**优点**： 容量小时高效<br>
**缺点**： 容量大时低效(需要多次)<br>
如：接水时喝水时用杯子方便，洗澡时还用杯子需要跑很多趟

#### 文件输入流(FileInputStream)
<details>
<summary> &#127808; FileInputStream JDK &#127808;</summary>

###### JDK
- 构造方法
  - **FileInputStream(File file)**: 通过打开与实际文件的连接创建一个 FileInputStream ，该文件由文件系统中的 File对象 file命名。  
  - **FileInputStream(FileDescriptor fdObj)**: 创建 FileInputStream通过使用文件描述符 fdObj ，其表示在文件系统中的现有连接到一个实际的文件。 
  - **FileInputStream(String name)**: 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。  
<br>
- 常用方法
  - **int available()**: 返回从此输入流中可以读取（或跳过）的剩余字节数的估计值，而不会被下一次调用此输入流的方法阻塞。   
  - **void close()**: 关闭此文件输入流并释放与流相关联的任何系统资源。   
  - **protected void finalize()**: 确保当这个文件输入流的 close方法没有更多的引用时被调用。 
  - **FileChannel getChannel()**: 返回与此文件输入流相关联的唯一的FileChannel对象。 
  - **FileDescriptor getFD()**: 返回表示与此 FileInputStream正在使用的文件系统中实际文件的连接的 FileDescriptor对象。   
  - **int read()** 
从该输入流读取一个字节的数据。 
  - **int read(byte[] b)**: 从该输入流读取最多 b.length个字节的数据为字节数组。  
  - **int read(byte[] b, int off, int len)** 
从该输入流读取最多 len字节的数据为字节数组。 
  - **long skip(long n)**: 跳过并从输入流中丢弃 n字节的数据。 
</details>

<br>

```java
package IO;

import java.io.FileInputStream;

/**
 * FileInputStream
 * 文件字节输入流
 */
public class InputStream {
    public static void main(String[] args) throws Exception {
        // 1. 创建FileInputStream，并指定问价路径
        FileInputStream in = new FileInputStream("H:\\Git\\Java_note\\IO\\3.字节流\\FileInputStream.md");
        // 2. 读取文件

// 单字节读取（为什么用 char data 不行？）
//        int data;
//        while((data = in.read()) != -1){
//            System.out.println((char) data);
//        }

// 多字节读取
//        byte[] buf = new byte[3];
//        int count = in.read(buf);
//        System.out.println(new String(buf));
//        System.out.println(count);

// 优化
        byte[] buf = new byte[3];
        int count = 0;
        while ((count = in.read(buf)) != -1){
            System.out.println(new String(buf, 0 ,count));
        }


        // 3. 关闭
        in.close();
        System.out.println("执行结束！");
    }
}
```

#### 文件输出流(FileOutputStream)
<details>
<summary> &#127808; FileOutputStream JDK &#127808;</summary>

###### JDK
- 构造方法
  - **FileOutputStream(File file)**： 创建文件输出流以写入由指定的 File对象表示的文件。 
  - **FileOutputStream(File file, boolean append)**： 创建文件输出流以写入由指定的 File对象表示的文件。
  - **FileOutputStream(FileDescriptor fdObj)**： 创建文件输出流以写入指定的文件描述符，表示与文件系统中实际文件的现有连接。 
  - **FileOutputStream(String name)**： 创建文件输出流以指定的名称写入文件。 
  - **FileOutputStream(String name, boolean append)**： 创建文件输出流以指定的名称写入文件。 
<br>
- 常用方法
  - **close()**: 关闭此文件输出流并释放与此流相关联的任何系统资源。 
  - **finalize()**: 清理与文件的连接，并确保当没有更多的引用此流时，将调用此文件输出流的 close 方法。 
  - **FileChannel getChannel()**: 返回与此文件输出流相关联的唯一的 FileChannel 对象。 
  - **FileDescriptor getFD()**: 返回与此流相关联的文件描述符。 
  - **void write(byte[] b)**: 将 b.length 个字节从指定的字节数组写入此文件输出流。 
  - **void write(byte[] b, int off, int len)**: 将 len 字节从位于偏移量 off 的指定字节数组写入此文件输出流。
  - **void write(int b)**: 将指定的字节写入此文件输出流。
</details>


```java
package IO;

import java.io.FileOutputStream;
import java.nio.charset.StandardCharsets;

public class OutputStream {
    public static void main(String[] args) throws Exception {
        // 1. 创建问价字节输出流对象
        FileOutputStream out = new FileOutputStream("H:\\Git\\Java_note\\IO\\3.字节流\\test.md");
        // 2. 写入文件
        out.write(97);
        out.write('c');
        out.write('h');
        out.write('e');
        out.write('n');

        String string = "hello!";
        out.write(string.getBytes());
        // 3. 关闭
        out.close();
        System.out.println("执行关闭！");
    }
}
```

### &#127803; 3.2 图片复制案例
```java
package IO;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;

public class ImageCopy {
    public static void main(String[] args) throws Exception {
        FileInputStream in = new FileInputStream("H:\\Git\\Java_note\\IO\\3.字节流\\1-200306224U3.jpg");
        FileOutputStream out = new FileOutputStream("H:\\Git\\Java_note\\IO\\3.字节流\\test.jpg");
        // 一边读一边写
         byte[] buf = new byte[1024];
         int count = 0;
         while ((count = in.read(buf)) != -1){
             out.write(buf, 0, count);
         }
         in.close();
         out.close();
        System.out.println("closed!");

    }
}
```

## &#127800; 4 字节缓冲流
### 缓冲流：BufferedInputStream/ BufferedOutputStream
**优点**：
- 提高IO效率，减少访问磁盘次数
- 数据存储在缓冲区中，flush是将缓冲区的内容写入文件中，也可以直接close
- 即容量大时高效 如：接水时喝水时用杯子方便，洗澡时用水壶高效。<br>
### &#127803; 4.1 BufferedInputStream
<details>
<summary> &#127808; BufferedInputStream JDK &#127808;</summary>

**JDK**
- java.lang.Object 
  - java.io.InputStream 
    - java.io.FilterInputStream 
      - java.io.BufferedInputStream 
- 内部成员变量
  - protected byte[]  **buf**: 存储数据的内部缓冲区数组。 
  - protected int  **count**: 索引一大于缓冲区中最后一个有效字节的索引。  
  - protected int  **marklimit**: mark方法调用后，最大超前允许，后续调用 reset方法失败。  
  - protected int  **markpos**: pos字段在最后一个 mark方法被调用时的值。
  - protected int   **pos**: 缓冲区中的当前位置。  
- 构造方法
  - **BufferedInputStream(InputStream in)**：创建一个 BufferedInputStream并保存其参数，输入流 in ，供以后使用。 
  - **BufferedInputStream(InputStream in, int size)**：创建 BufferedInputStream具有指定缓冲区大小，并保存其参数，输入流 in ，供以后使用。 
- 常用方法
  - **int available()** :返回从该输入流中可以读取（或跳过）的字节数的估计值，而不会被下一次调用此输入流的方法阻塞。  
  - **void close()**:关闭此输入流并释放与流相关联的任何系统资源。  
  - **void mark(int readlimit)**:见的总承包 mark的方法 InputStream 。  
  - **boolean markSupported()**: 测试这个输入流是否支持 mark和 reset方法。  
  - **int read()**: 见 read法 InputStream的一般合同。 
  - **int read(byte[] b, int off, int len)**: 从给定的偏移开始，将字节输入流中的字节读入指定的字节数组。 
  - **void reset()**: 见 reset法 InputStream的一般合同。  
  - **long skip(long n)**: 见 skip法 InputStream的一般合同。  
</details>

```java
package IO.BufferStream;

import java.io.BufferedInputStream;
import java.io.FileInputStream;
import java.io.IOException;

public class InputStream {
    public static void main(String[] args) throws IOException {
        // 1 创建BufferedInputStream
        FileInputStream fis = new FileInputStream("H:\\Git\\Java_note\\IO\\3.字节流\\BufferedInputStream.md");
        BufferedInputStream bis = new BufferedInputStream(fis);
        // 2 读取
        int data = 0;
        // 此方法使用 BufferedInputStream 内部自带缓存区，大小为 DEFAULT_BUFFER_SIZE = 8192
        while((data = bis.read()) != -1){
            System.out.print(((char) data));
        }

        // 用自己创建的缓冲流
        byte[] buf = new byte[1024];
        int count = 0;
        while((count = bis.read(buf)) != -1){
            System.out.print((new String(buf, 0, count)));
        }

        // 3 关闭
        bis.close();
    }
}
```

### &#127803; 4.2 BufferedOutputStream
<details>
<summary> &#127808; BufferedOutputStream JDK &#127808;</summary>

**JDK**
- java.lang.Object 
  - java.io.InputStream 
    - java.io.FilterInputStream 
      - java.io.BufferedOutputStream 
- 内部成员变量
  - protected byte[] **buf**: 存储数据的内部缓冲区。
  - protected int **count**: 缓冲区中有效字节的数量。  
- 构造方法
  - **BufferedOutputStream(OutputStream out)**: 创建一个新的缓冲输出流，以将数据写入指定的底层输出流。  
  - **BufferedOutputStream(OutputStream out, int size)**: 创建一个新的缓冲输出流，以便以指定的缓冲区大小将数据写入指定的底层输出流。  

- 常用方法
  - **void flush()**: 刷新缓冲输出流。  
  - **void write(byte[] b, int off, int len)**: 从指定的字节数组写入 len个字节，从偏移 off开始到缓冲的输出流。  
  - **void write(int b)**: 将指定的字节写入缓冲的输出流。  
</details>


```java
package IO.BufferStream;

import java.io.BufferedOutputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class OutputStream {
    public static void main(String[] args) throws IOException {
        // 1 创建BufferedInputStream
        FileOutputStream fos = new FileOutputStream("H:\\Git\\Java_note\\IO\\3.字节流\\BufferedOutputStream.md");
        BufferedOutputStream bos = new BufferedOutputStream(fos);
        // 2 写入文件
        for(int i = 0; i < 10; i ++){
            bos.write("OutputStream\r\n".getBytes());// 写入8k缓冲区
            bos.flush(); // 刷新到硬盘
        }
        // 3 关闭(若中途没有 flush，最后调用 close 时，会自动调用 flush，一次性刷新)
        bos.close();
        System.out.println("finished!");
    }
}
```

## &#127800; 5. 对象流 ObjectOutputStream(序列化) / ObjectInputStream(反序列化)
- 增强了缓冲区功能
- 增强了读写8种基本数据类型和字符串的功能
- 增强了读写对象的功能
  - `readObject()` 从流中读取一个对象
  - `writeObject(Object obj)` 向流中写入一个对象
  
**使用流传输对象的过程称为序列化、反序列化**<br>
**注意顺序!!**
1. 序列化反序列化时必须作用在同一个对象上(否则无法转换)
2. 序列化(ObjectOutputStream)
3. 反序列化(ObjectInputStream)
<details>
<summary> &#127808; Student &#127808;</summary>
  
```java
package IO;

import java.io.Serializable;

// 想要使其可序列化，必须实现'标记接口' Serializable
public class Student implements Serializable {
    private String name;
    private  int age;
    public Student(String name, int age){
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

</details>

#### &#127803; 5.1 ObjectOutputStream(序列化)
<details>
<summary> &#127808; ObjectOutputStream JDK &#127808;</summary>

##### JDK
- java.lang.Object 
  - java.io.OutputStream 
    - java.io.ObjectOutputStream 
- 构造方法
  - **protected  ObjectOutputStream()**: 为完全重新实现ObjectOutputStream的子类提供一种方法，不必分配刚刚被ObjectOutputStream实现使用的私有数据。
  - **protected ObjectOutputStream(OutputStream out)**:创建一个写入指定的OutputStream的ObjectOutputStream。
- 常用方法
  - **void close()** 关闭流。  
  - **void flush()** 刷新流。  
  - **void write(int val)** 写一个字节。  
  - ***void writeObject(Object obj) 将指定的对象写入ObjectOutputStream。*** 
  - ***void writeUTF(String str) 此字符串的原始数据写入格式为 modified UTF-8 。***  
</details>

<details>
<summary> &#127808; 其他write方法 &#127808;</summary>
  
- void write(byte[] buf) 写入一个字节数组。  
- void write(byte[] buf, int off, int len)  写入一个子字节数组。  
- void writeBoolean(boolean val) 写一个布尔值。  
- void writeByte(int val) 写入一个8位字节。  
- void writeBytes(String str) 写一个字符串作为字节序列。  
- void writeChar(int val) 写一个16位的字符。  
- void writeChars(String str) 写一个字符串作为一系列的字符。  
- protected void writeClassDescriptor(ObjectStreamClass desc) 将指定的类描述符写入ObjectOutputStream。  
- void writeDouble(double val) 写一个64位的双倍。 
- void writeFields() 将缓冲的字段写入流。  
- void writeFloat(float val) 写一个32位浮点数。  
- void writeInt(int val) 写一个32位int。  
- void writeLong(long val) 写一个64位长  
- protected void writeObjectOverride(Object obj) 子类使用的方法来覆盖默认的writeObject方法。  
- void writeShort(int val) 写一个16位短。  
- protected void writeStreamHeader() 提供了writeStreamHeader方法，因此子类可以在流中附加或预先添加自己的头。 
- void writeUnshared(Object obj) 将“非共享”对象写入ObjectOutputStream。 
</details>

```java
package IO.ObjectOutputStream;

import java.io.*;

/**
 * 实现序列化
 */
public class OutputStream {
    public static void main(String[] args) throws IOException {
        // 1. 创建对象流
        FileOutputStream fos = new 	FileOutputStream("H:\\Git\\Java_note\\IO\\3.字节流\\ObjectOutputStream.java");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        // 2. 序列化（写入操作）
        Student s1 = new Student("zs", 20);
        oos.writeObject(s1);
        // 3. 关闭
        oos.close();
        System.out.println(("序列化完毕"));
    }
}
```


#### &#127803; 5.2 ObjectInputStream(反序列化)
<details>
<summary> &#127808; ObjectInputStream JDK &#127808;</summary>

##### JDK
- java.lang.Object 
  - java.io.OutputStream 
    - java.io.ObjectOutputStream 
- 构造方法
  - **protected  ObjectInputStream()**: 为完全重新实现ObjectInputStream的子类提供一种方法，不必分配刚刚被ObjectInputStream实现使用的私有数据。
  - **protected ObjectInputStream(InputStream in)**: 创建从指定的InputStream读取的ObjectInputStream。  

- 常用方法
  - **void close()** 关闭流。  
  - **int read()** 读取一个字节的数据。 
  - **String readUTF()** 以 modified UTF-8格式读取字符串。  
  - **Object readObject()** 从ObjectInputStream读取一个对象。 
</details>

<details>
<summary> &#127808; 详细方法 &#127808;</summary>
  
- int available() 返回可以读取而不阻塞的字节数。  
- void close() 关闭输入流。  
- void defaultReadObject() 从此流读取当前类的非静态和非瞬态字段。  
- protected boolean enableResolveObject(boolean enable) 启用流以允许从流中读取的对象被替换。  
- int read() 读取一个字节的数据。  
- int read(byte[] buf, int off, int len) 读入一个字节数组。  
- boolean readBoolean() 读取布尔值。  
- byte readByte() 读取一个8位字节。  
- char readChar() 读一个16位字符。  
- protected ObjectStreamClass readClassDescriptor() 从序列化流读取类描述符。  
- double readDouble() 读64位双倍。  
- ObjectInputStream.GetField readFields() 从流中读取持久性字段，并通过名称获取它们。  
- float readFloat() 读32位浮点数。  
- void readFully(byte[] buf) 读取字节，阻塞直到读取所有字节。  
- void readFully(byte[] buf, int off, int len) 读取字节，阻塞直到读取所有字节。  
- int readInt() 读取一个32位int。   
- long readLong() 读64位长。  
- Object readObject() 从ObjectInputStream读取一个对象。  
- protected Object readObjectOverride() 此方法由ObjectOutputStream的受信任子类调用，该子类使用受保护的无参构造函数构造ObjectOutputStream。  
- short readShort() 读取16位短。  
- protected void readStreamHeader() 提供了readStreamHeader方法来允许子类读取和验证自己的流标题。  
- Object readUnshared() 从ObjectInputStream读取一个“非共享”对象。  
- int readUnsignedByte() 读取一个无符号的8位字节。 
- int readUnsignedShort() 读取无符号16位短。  
- String readUTF() 以 modified UTF-8格式读取字符串。  
- void registerValidation(ObjectInputValidation obj, int prio) 在返回图之前注册要验证的对象。  
- protected 类<?> resolveClass(ObjectStreamClass desc) 加载本地类等效的指定流类描述。  
- protected Object resolveObject(Object obj) 此方法将允许ObjectInputStream的受信任子类在反序列化期间将一个对象替换为另一个对象。  
- protected 类<?> resolveProxyClass(String[] interfaces) 返回一个代理类，它实现代理类描述符中命名的接口; 子类可以实现此方法从流中读取自定义数据以及动态代理类的描述符，从而允许它们为接口和代理类使用备用加载机制。  
- int skipBytes(int len) 跳过字节。  
  
</details>

```java
package IO.ObjectInputStream;

import IO.Student;
import java.io.FileInputStream;
import java.io.ObjectInputStream;

public class InputStream {
    public static void main(String[] args) throws Exception {
        // 1. 创建对象流
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("H:\\Git\\Java_note\\IO\\3.字节流\\Object.txt"));
        // 2. 读取文件（反序列化）
        Student s = (Student) ois.readObject();
        // 3. 关闭
        ois.close();
        System.out.println(("执行完毕!"));
        System.out.println((s.toString()));
    }
}
```
###### 注意点
- 某个类要想序列化必须实现 Serializable 接口
- 序列化类中对象属性要求实现 Serializable 接口
- 序列化版本号 ID : 保证序列化的类和反序列化的类是同一个类
  - `private static final long serialVersionUID = 1L;`
- 使用 transient 修饰属性，这个属性就不能序列化
- 静态属性不能序列化
- 序列化多个对象，可以借助集合来实现

<details>
<summary> &#127808; 集合实现 &#127808;</summary>
  
```java
public class OutputStream {
    public static void main(String[] args) throws IOException {
        // 1. 创建对象流
        FileOutputStream fos = new 	FileOutputStream("H:\\Git\\Java_note\\IO\\3.字节流\\Object.txt");
        ObjectOutputStream oos = new ObjectOutputStream(fos);
        // 2. 序列化（写出操作）
        ArrayList<Student> list = new ArrayList<>();
        Student s1 = new Student("zs", 20);
        Student s2 = new Student("ls", 24);
        list.add(s1);
        oos.writeObject(list);
        // 3. 关闭
        oos.close();
        System.out.println(("序列化完毕!"));
        System.out.println(list.toString());
    }
}
```
```java
public class InputStream {
    public static void main(String[] args) throws Exception {
        // 1. 创建对象流
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("H:\\Git\\Java_note\\IO\\3.字节流\\Object.txt"));
        // 2. 读取文件（反序列化）
//        Student s = (Student) ois.readObject();
        // 3. 关闭
        ArrayList list = (ArrayList) ois.readObject();
        ois.close();
        System.out.println(("反序列化执行完毕!"));
        System.out.println((list.toString()));
    }
}
```
</details>

## &#127800; 6. 编码方式(编码方式不同即会出现乱码)
- UTF-8 : 国际编码
- DB2312 : 简体中文
- GBk :  简体中文扩充
- BIG5 ： 繁体中文
  
## &#127800; 7. 字符流
-  为什么需要字符流?
    - 当文件是类似于汉字一样，一个汉字占3个字节时，字节流无法正常读取。
```java
public class FileInputStream_Test {
    public static void main(String[] args) throws IOException {
        FileInputStream fis = new FileInputStream("H:\\Git\\Java_note\\IO\\3.字节流\\test.md");
        int data = 0;
        while ((data = fis.read()) != -1){
            // 一个字节一个字节读，但是一个汉字是3个字节，无法正常读取。
            System.out.print((char) data);
        }
    }
}
```
#### &#127803; 6.1 父类(Reader、Writer)
##### 6.1.1 字符输出流 abstract Writer
 
<details>
<summary> &#127808; Writer JDK &#127808;</summary>

- 构造方法
  - protected  Writer() 创建一个新的人物流作家，其关键部分将在作者本身上同步。  
  - protected  Writer(Object lock) 创建一个新的字符流写入器，其关键部分将在给定对象上进行同步。  
- 常用方法
  - Writer append(char c) 将指定的字符附加到此作者。
  - abstract void close() 关闭流，先刷新。  
  - abstract void flush() 刷新流。  
  - void write(char[] cbuf) 写入一个字符数组。
</details>

##### 6.1.2 字符输入流 abstract Reader

<details>
<summary> &#127808; Reader JDK &#127808;</summary>
  
- 构造方法
  - protected  Reader() 创建一个新的字符流阅读器，其关键部分将在阅读器本身上同步。  
  - protected  Reader(Object lock) 创建一个新的字符流阅读器，其关键部分将在给定对象上同步。 
- 常用方法
  - abstract void close() 关闭流并释放与之相关联的任何系统资源。 
  - void mark(int readAheadLimit) 标记流中的当前位置。  
  - int read() 读一个字符  
  - boolean ready() 告诉这个流是否准备好被读取。  
  - void reset() 重置流。  
  - long skip(long n) 跳过字符  
</details>
  
##### 6.2.1 Reader 子类 FileReader

<details>
<summary> &#127808; FileReader JDK &#127808;</summary>
  
- java.lang.Object 
  - java.io.Reader 
    - java.io.InputStreamReader 
      - java.io.FileReader 
- 构造方法
  - FileReader(File file) 创建一个新的 FileReader ，给出 File读取。  
  - FileReader(FileDescriptor fd) 创建一个新的 FileReader ，给定 FileDescriptor读取。 
  - FileReader(String fileName) 创建一个新的 FileReader ，给定要读取的文件的名称。 
- 常用方法
  - 继承父类 InputStreamReader 的方法
</details>
  
```java
package IO.FileReader;

import java.io.FileReader;
import java.io.IOException;

public class FileReader_Test {
    public static void main(String[] args) throws IOException {
        // 1. 创建FileReader 文件字符输入流
        FileReader fr = new FileReader("H:\\Git\\Java_note\\IO\\3.字节流\\test.md");
        // 2. 读取
        // 2.1
        // 方法一：单个字符读取
        int data = 0;
        while((data = fr.read()) != -1){
            System.out.print((char) data);
        }

        // 方法二：字符缓冲区读取
        char[] buf = new char[2];
        int count = 0;
        while((count = fr.read(buf)) != -1){
            System.out.println(new String(buf, 0, count));
        }

        // 3. 关闭
        fr.close();
    }
}

```
  
##### 6.2.2 Writer 子类 FileWriter
<details>
<summary> &#127808; FileWriter JDK &#127808;</summary>

- java.lang.Object 
  - java.io.Writer 
    - java.io.OutputStreamWriter 
      - java.io.FileWriter 
- 构造方法
  - FileWriter(File file) 给一个File对象构造一个FileWriter对象。  
  - FileWriter(File file, boolean append) 给一个File对象构造一个FileWriter对象。  
  - FileWriter(FileDescriptor fd) 构造与文件描述符关联的FileWriter对象。  
  - FileWriter(String fileName) 构造一个给定文件名的FileWriter对象。  
  - FileWriter(String fileName, boolean append) 构造一个FileWriter对象，给出一个带有布尔值的文件名，表示是否附加写入的数据。  

- 常用方法
  - 继承父类 OutputStreamWriter 的方法
</details>
  
```java
package IO.FileStream;

import java.io.FileWriter;
import java.io.IOException;

public class FileWriter_Test {
    public static void main(String[] args) throws IOException {
        // 1. 创建FileWriter对象
        FileWriter fw = new FileWriter("H:\\Git\\Java_note\\IO\\3.字节流\\test.md");
        // 2. 写入
        for(int i = 0; i < 10; i ++){
            fw.write("写入的内容: FileWriter_Test\r\n");
            fw.flush();
        }
        // 3. 关闭
        fw.close();
        System.out.println(("执行完毕!"));
    }
}
```

#### &#127800; 6.3 使用字符流复制文件
                              
```java
package IO.FileStream;

import java.io.FileReader;
import java.io.FileWriter;
import java.io.IOException;

/**
 * 不能复制图片或者二进制文件（因为不是字符编码文件，使用字符读取即读取的都是乱码）
 * 需使用字节流
 */
public class FileCopy {
    public static void main(String[] args) throws IOException {
        // 1. 创建
        FileReader fr = new FileReader("H:\\Git\\Java_note\\IO\\3.字节流\\test.md");
        FileWriter fw = new FileWriter("H:\\Git\\Java_note\\IO\\3.字节流\\FileCopy.md");
        // 2. 读写
        int data = 0;
        while ((data = fr.read()) != -1){
            fw.write(data);
            fw.flush();
        }
        // 3. 关闭
        fw.close();
        fr.close();
        System.out.println("执行完毕！");
    }
}
```

 
## &#127800; 8. 字符缓冲流

- 高效读写、支持输入换行符、可一次写一行读一行
###  BufferedReader / BufferedWriter
### &#127803; 8.1 BufferedReader
<details>
<summary> &#127808; BufferedReader JDK &#127808;</summary>

- java.lang.Object 
  - java.io.Reader 
    - java.io.BufferedReader 
- 构造方法
  - BufferedReader(Reader in) 创建使用默认大小的输入缓冲区的缓冲字符输入流。  
  - BufferedReader(Reader in, int sz) 创建使用指定大小的输入缓冲区的缓冲字符输入流。  
- 常用方法
  - void close() 关闭流并释放与之相关联的任何系统资源。  
  - Stream<String> lines() 返回一个 Stream ，其元素是从这个 BufferedReader读取的行。  
  - void mark(int readAheadLimit) 标记流中的当前位置。  
  - boolean markSupported() 告诉这个流是否支持mark（）操作。  
  - int **read()** 读一个字符  
  - int **read(char[] cbuf, int off, int len)** 将字符读入数组的一部分。  
  - String **readLine()** 读一行文字。  
  - boolean ready() 告诉这个流是否准备好被读取。  
  - void reset() 将流重置为最近的标记。  
  - long skip(long n) 跳过字符  
</details>

```java
package IO.BufferFileStream;

import java.io.BufferedReader;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class BufferReader_Test {
    public static void main(String[] args) throws IOException {
        // 创建缓冲流
        FileReader fr = new FileReader("H:\\Git\\Java_note\\IO\\Test\\test.md");
        BufferedReader br = new BufferedReader(fr);
        // 读取
        // 1. 第一种方式 将字符读入数组
        char[] buf = new char[1024];
        int count = 0;
        while((count = br.read(buf)) != -1){
//      br.read(buf, 0, buf.length) 只放数组，默认 0-length
            System.out.println(new String(buf, 0, count));
        }
        // 2. 第二种方式 一行一行读取
        String line = null;
        while((line = br.readLine()) != null){
            System.out.println(line);
        }
        // 3. 第三种方式 逐个字符读取
        int temp = 0;
        while ((temp = br.read()) != -1){
            System.out.print((char) temp);
        }

        // 关闭
        br.close();
    }
}
```
### &#127803; 8.2 BufferedWriter
<details>
<summary> &#127808; BufferedWriter JDK &#127808;</summary>

- java.lang.Object 
  - java.io.Writer 
    - java.io.BufferedWriter 
- 构造方法
  - BufferedWriter(Writer out) 创建使用默认大小的输出缓冲区的缓冲字符输出流。  
  - BufferedWriter(Writer out, int sz) 创建一个新的缓冲字符输出流，使用给定大小的输出缓冲区。   
- 常用方法
  - void close() 关闭流，先刷新。  
  - void flush() 刷新流。  
  - void newLine() 写一行行分隔符。  
  - void write(char[] cbuf, int off, int len) 写入字符数组的一部分。  
  - void write(int c) 写一个字符  
  - void write(String s, int off, int len) 写一个字符串的一部分。  
</details>
  
```java
package IO.BufferFileStream;

import java.io.BufferedWriter;
import java.io.FileWriter;
import java.io.IOException;

public class BufferWriter_Test {
    public static void main(String[] args) throws IOException {
        // 1. 创建BufferedWriter对象
        FileWriter fw = new FileWriter("H:\\Git\\Java_note\\IO\\Test\\BufferWriter.md");
        BufferedWriter bw = new BufferedWriter(fw);
        // 2. 写入
        for(int i = 0; i < 10; i ++){
            bw.write(i + ". 写入的内容: BufferWriter");
            bw.newLine(); // 写入一个换行符 windows \r\n linux \n
            bw.flush();
        }
        // 3. 关闭
        bw.close(); // 此时会自动关闭fw
        System.out.println("执行完毕！");
    }
}
```
                              
## &#127800; 9. 打印流 PrintWriter
- 封装了print() / println() 方法 支持写入后换行
- 支持数据原样打印
  
<details>
<summary> &#127808; PrintWriter JDK &#127808;</summary>

- java.lang.Object 
  - java.io.Writer 
    - java.io.PrintWriter 
- 构造方法
  - PrintWriter(File file) 使用指定的文件创建一个新的PrintWriter，而不需要自动的线路刷新。  
  - PrintWriter(File file, String csn) 使用指定的文件和字符集创建一个新的PrintWriter，而不需要自动进行线条刷新。  
  - PrintWriter(OutputStream out) 从现有的OutputStream创建一个新的PrintWriter，而不需要自动线路刷新。 
  - PrintWriter(OutputStream out, boolean autoFlush) 从现有的OutputStream创建一个新的PrintWriter。
  - PrintWriter(String fileName) 使用指定的文件名创建一个新的PrintWriter，而不需要自动执行行刷新。  
  - PrintWriter(String fileName, String csn) 使用指定的文件名和字符集创建一个新的PrintWriter，而不需要自动线路刷新。  
  - PrintWriter(Writer out) 创建一个新的PrintWriter，没有自动线冲洗。  
  - PrintWriter(Writer out, boolean autoFlush) 创建一个新的PrintWriter。  
- 常用方法
  - PrintWriter **append(char c)** 将指定的字符附加到此作者。 
  - boolean checkError() 如果流未关闭，请刷新流并检查其错误状态。  
  - protected void clearError() 清除此流的错误状态。  
  - void close() 关闭流并释放与之相关联的任何系统资源。  
  - void flush() 刷新流。  
  - PrintWriter format(Locale l, String format, Object... args) 使用指定的格式字符串和参数将格式化的字符串写入此写入程序。  
  - void **print(boolean b)** 打印布尔值。  
  - PrintWriter printf(String format, Object... args) 使用指定的格式字符串和参数将格式化的字符串写入该writer的方便方法。  
  - void println() 通过写入行分隔符字符串来终止当前行。  
  - void println(boolean x) 打印一个布尔值，然后终止该行。  
  - void **write(char[] buf)** 写入一个字符数组。  
**append、print、write 有多个重载方法**

</details>
  
```java
package IO.PrintWriter;

import java.io.FileNotFoundException;
import java.io.PrintWriter;

public class PrintWriter_Test {
    public static void main(String[] args) throws FileNotFoundException {
        // 1 创建打印流
        PrintWriter pw = new PrintWriter("H:\\Git\\Java_note\\IO\\Test\\PrintWriter.md");
        // 2 打印
        pw.println(12);
        pw.println(true);
        pw.println(3.14);
        pw.println('a');
        // 3 关闭
        pw.close();
        System.out.println("执行完毕！");
    }
}
```
  
<details>
<summary> &#127808; JDK &#127808;</summary>



</details>
  
## &#127800; 10. 转换流 PrintWriter
- 桥转换流 InputStreamReader / OutputStreamWriter
  - 可将字节流转换为字符流
  - 可设置字符的编码方式
### InputStreamReader
<details>
<summary> &#127808; InputStreamReader JDK &#127808;</summary>

- java.lang.Object 
  - java.io.Reader 
    - java.io.InputStreamReader 
- 构造方法
  - InputStreamReader(InputStream in) 创建一个使用默认字符集的InputStreamReader。  
  - InputStreamReader(InputStream in, Charset cs) 创建一个使用给定字符集的InputStreamReader。  
  - InputStreamReader(InputStream in, CharsetDecoder dec) 创建一个使用给定字符集解码器的InputStreamReader。  
  - InputStreamReader(InputStream in, String charsetName) 创建一个使用命名字符集的InputStreamReader。  
- 常用方法
  - void close() 关闭流并释放与之相关联的任何系统资源。  
  - String getEncoding() 返回此流使用的字符编码的名称。  
  - int read() 读一个字符  
  - int read(char[] cbuf, int offset, int length) 将字符读入数组的一部分。  
  - boolean ready() 告诉这个流是否准备好被读取。 

</details>
  
```java
package IO.Input_Output_Stream_Reader_writer;

import java.io.*;
/**
 * 字节流到字符流的桥
 */
public class InputStreamReader_Test {
    public static void main(String[] args) throws IOException {
        // 1 创建InputStreamReader对象
        FileInputStream fis = new FileInputStream("H:\\Git\\Java_note\\IO\\Test\\test.md");
        InputStreamReader isr = new InputStreamReader(fis, "utf-8");
        // 2 读取文件
        int data = 0;
        while((data = isr.read()) != -1){
            System.out.print((char) data);
        }
        // 3 关闭
        isr.close();
    }
}
```
  
### &#127803; 10.2 OutputStreamWriter
<details>
<summary> &#127808; OutputStreamWriter JDK &#127808;</summary>

- java.lang.Object 
  - java.io.Writer 
    - java.io.OutputStreamWriter 
- 构造方法
  - OutputStreamWriter(OutputStream out) 创建一个使用默认字符编码的OutputStreamWriter。  
  - OutputStreamWriter(OutputStream out, Charset cs) 创建一个使用给定字符集的OutputStreamWriter。  
  - OutputStreamWriter(OutputStream out, CharsetEncoder enc) 创建一个使用给定字符集编码器的OutputStreamWriter。  
  - OutputStreamWriter(OutputStream out, String charsetName) 创建一个使用命名字符集的OutputStreamWriter。  
- 常用方法
  - void close() 关闭流，先刷新。  
  - void flush() 刷新流。  
  - String getEncoding() 返回此流使用的字符编码的名称。  
  - void write(char[] cbuf, int off, int len) 写入字符数组的一部分。  
  - void write(int c) 写一个字符  
  - void write(String str, int off, int len) 写一个字符串的一部分。  

</details>


```java
package IO.Input_Output_Stream_Reader_writer;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

/**
 * 字符流到字节流的桥
 */
public class OutputStreamWriter_Test {
    public static void main(String[] args) throws IOException {
        // 1 创建OutputStreamReader对象
        FileOutputStream fos = new FileOutputStream("H:\\Git\\Java_note\\IO\\Test\\test.md");
        OutputStreamWriter osw = new OutputStreamWriter(fos, "utf-8");
        // 2 写入
        for(int i = 0; i < 10; i ++){
            osw.write("写入内容");
            osw.flush();
        }
        // 3 关闭
        osw.close();
    }
}
```
                              
<details>
<summary> &#127808; JDK &#127808;</summary>



</details>

## &#127800; 11. File流