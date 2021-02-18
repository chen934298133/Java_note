## &#127800; Properties
- Properties类表示一组持久的属性。 Properties可以保存到流中或从流中加载。 属性列表中的每个键及其对应的值都是一个字符串。

<details>
<summary>&#127808; View The Points &#127808;</summary>
  
- java.lang.Object 
  - java.util.Dictionary<K,V> 
    - java.util.Hashtable<Object,Object> 
      - java.util.Properties 
- 构造方法
  - Properties() 创建一个没有默认值的空属性列表。  
  - Properties(Properties defaults) 创建具有指定默认值的空属性列表。  
- 常用方法
  - String getProperty(String key) 使用此属性列表中指定的键搜索属性。  
  - String getProperty(String key, String defaultValue) 使用此属性列表中指定的键搜索属性。  
  - void list(PrintStream out) 将此属性列表打印到指定的输出流。  
  - void list(PrintWriter out) 将此属性列表打印到指定的输出流。  
  - void load(InputStream inStream) 从输入字节流读取属性列表（键和元素对）。  
  - void load(Reader reader) 以简单的线性格式从输入字符流读取属性列表（关键字和元素对）。  
  - void loadFromXML(InputStream in) 将指定输入流中的XML文档表示的所有属性加载到此属性表中。  
  - Enumeration<?> propertyNames() 返回此属性列表中所有键的枚举，包括默认属性列表中的不同键，如果尚未从主属性列表中找到相同名称的键。   
  - Object setProperty(String key, String value) 致电 Hashtable方法 put 。  
  - void store(OutputStream out, String comments) 将此属性列表（键和元素对）写入此 Properties表中，以适合于使用 load(InputStream)方法加载到 Properties表中的格式输出流。  
  - void store(Writer writer, String comments) 将此属性列表（键和元素对）写入此 Properties表中，以适合使用 load(Reader)方法的格式输出到输出字符流。 
  - void storeToXML(OutputStream os, String comment) 发出表示此表中包含的所有属性的XML文档。  
  - void storeToXML(OutputStream os, String comment, String encoding) 使用指定的编码发出表示此表中包含的所有属性的XML文档。  
  - Set<String> stringPropertyNames() 返回此属性列表中的一组键，其中键及其对应的值为字符串，包括默认属性列表中的不同键，如果尚未从主属性列表中找到相同名称的键。 

</details>

```java
package IO;

import java.io.*;
import java.util.Properties;
import java.util.Set;

/**
 * Properties可以保存到流中或从流中加载。
 */
public class properties_Test {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();
        //
        properties.setProperty("z1","21");
        properties.setProperty("z2","22");
        properties.setProperty("z3","23");
        System.out.println(properties.toString());

        // 遍历
        // 1. keySet
        // 2. entrySet
        // 3. stringPropertyNames
        Set<String> set = properties.stringPropertyNames();
        for (String str : set){
            System.out.println(str + " == " + properties.getProperty(str));
        }

        // 和流有关的方法 写出
        PrintWriter pw = new PrintWriter("H:\\Git\\Java_note\\IO\\Test\\properties.md");
        properties.list(pw);    // 将properties的内容写到 pw 中
        pw.close();     // 自带flush

        // store 方法 保存写出
        FileOutputStream fos = new FileOutputStream("H:\\Git\\Java_note\\IO\\Test\\properties.properties");
        properties.store(fos,"test");
        fos.close();    // 自带flush

        // load 方法 加载写入
        Properties properties1 = new Properties();
        FileInputStream fis = new FileInputStream("H:\\Git\\Java_note\\IO\\Test\\properties.properties");
        properties1.load(fis);
        fis.close();
        System.out.println(properties1.toString());
    }
}
```