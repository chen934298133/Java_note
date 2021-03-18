## &#127800; 获取当前时间戳

<details>
<summary> &#127809; 时间戳 &#127809; </summary>
  
```java
//方法 一
long time1 = System.currentTimeMillis()
//方法 二
long time2 = Calendar.getInstance().getTimeInMillis();
//方法 三
long time3 = new Date().getTime();
```
</details>

<details>
<summary> &#127809; 时间 &#127809; </summary>
  
```java
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//设置日期格式
String date = df.format(new Date());
System.out.println("当前时间："+date);
```
</details>

<details>
<summary> &#127809; 时间戳转换为时间 &#127809; </summary>
  
```java
//date是yyyy-MM-dd HH:mm:ss格式的String类型的时间
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date time= simpleDateFormat.parse(date);
long timeStamp= time.getTime();
System.out.println(timeStamp);
```
</details>

<details>
<summary> &#127809; 时间转换为时间戳 &#127809; </summary>
  
```java
//s是String类型的时间戳
SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
long timeStamp= new Long(s);
Date date = new Date(timeStamp);
String time= simpleDateFormat.format(date);
System.out.println(time);
```
</details>

## &#127800; Date
- 获取日期
  - new Date()
- 日期 -> 时间戳
  - new Date().getTime()
- 时间戳 -> 日期
  - new Date(dateStamp)
  
<details>
<summary> &#127809; Date &#127809; </summary>
  
```java
package Time;

import java.util.Date;

public class DateTest {
    public static void main(String[] args){

        Date date = new Date();// Thu Mar 18 21:44:23 CST 2021
        System.out.println(date);

        // 1616075063369
        System.out.println(date.getTime());

        long dateTemp = date.getTime();// 1616075063369
        date = new Date(dateTemp);//Thu Mar 18 21:44:23 CST 2021
        System.out.println(date);
    }
}
```
</details>


## &#127800; Date

- 获取格式
  - new SimpleDateFormat("......")
- 得到指定格式的日期
  - new SimpleDateFormat("......").format(new Date())
- firmat( new Date)
  - format返回的是一个StringBuffer类型的数据
  - format(Date date) 将日期格式化成日期/时间字符串。
- parse("2021-3-18 22:03:36")
  - parse()返回的是一个Date类型数据
  - parse(String source) 从给定字符串的开始解析文本以生成日期。 

<details>
<summary> &#127809; SimpleDateFormat &#127809; </summary>
  
```java
package Time;

import java.text.ParseException;
import java.text.SimpleDateFormat;
import java.util.Date;

/**
 * G 年代标志符
 * y 年
 * M 月
 * d 日
 * h 时 在上午或下午 (1~12)
 * H 时 在一天中 (0~23)
 * m 分
 * s 秒
 * S 毫秒
 * E 星期
 * D 一年中的第几天
 * F 一月中第几个星期几
 * w 一年中第几个星期
 * W 一月中第几个星期
 * a 上午 / 下午 标记符
 * k 时 在一天中 (1~24)
 * K 时 在上午或下午 (0~11)
 * z 时区
 */
public class SimpleDateFormatTest {
    public static void main(String[] args) throws ParseException {

        // 主要是构造方法 及 format()方法
        SimpleDateFormat simpleDateFormatTest = new SimpleDateFormat();
        SimpleDateFormat myFmt = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒");
        SimpleDateFormat myFmt1 = new SimpleDateFormat("yy/MM/dd HH:mm");
        SimpleDateFormat myFmt2 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");//等价于now.toLocaleString()
        SimpleDateFormat myFmt3 = new SimpleDateFormat("yyyy年MM月dd日 HH时mm分ss秒 E ");
        SimpleDateFormat myFmt4 = new SimpleDateFormat("一年中的第 D 天 一年中第w个星期 一月中第W个星期 在一天中k时 z时区 G");
        Date now = new Date();
        System.out.println(myFmt.format(now));
        System.out.println(myFmt1.format(now));
        System.out.println(myFmt2.format(now));
        System.out.println(myFmt3.format(now));
        System.out.println(myFmt4.format(now));
        System.out.println(now.toLocaleString());
        System.out.println(now.toString());

        String date = "2021-3-18 22:03:36";
        Date parse = myFmt2.parse(date);
        System.out.println("parse: " + parse);
        System.out.println("parse: " + parse.getTime());
        String format = myFmt2.format(now);
        System.out.println(format);
    }
}

```
</details>
