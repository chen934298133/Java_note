##  &#127800; 当前时间戳 &#127800; 
```java
System.out.println("System.currentTimeMillis()::: "+System.currentTimeMillis());
System.out.println("new Date().getTime()::: "+new Date().getTime());
System.out.println("Calendar.getInstance().getTimeInMillis()::: "+Calendar.getInstance().getTimeInMillis());
// 这种方式速度最慢，这是因为Canlendar要处理时区问题会耗费较多的时间。
```

##  &#127800; Date 转时间戳 &#127800; 
```java
// Date 转时间戳
// new Date(Long long)
Date dateTemp = new Date(); // 当前date 转时间戳
Date dateParse = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("1997-3-18 22:03:36");   // 指定date 转时间戳
System.out.println("new Date().getTime()::: " + dateTemp.getTime());
System.out.println("new SimpleDateFormat().parse().getTime()::: " + dateParse.getTime());
```

##  &#127800; 时间戳 转 Date &#127800; 
```java
// 时间戳 转 Date
Long aLong = new Date(System.currentTimeMillis()).getTime();
System.out.println(new Date(aLong));// 当前时间戳 转 date
System.out.println(new SimpleDateFormat("yyyy-MM-dd").format(aLong)); // 时间戳 转 指定date
```

##  &#127800; Date、String互转 ——`SimpleDateFormat`的`format()`和`parse()`区别 &#127800; 
  - 返回值
    - format() 返回 String
    - parse() 返回 StringBuffer
  - 参数
    - format(Date date)、format(Object object)
    - parse(String str)
```java
// format(date) 返回 StringBuffer
String stringFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
System.out.println("String: SimpleDateFormat.format::: " + stringFormat);
// parse(string) 返回 Date
Date dateParse = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").parse("1997-3-18 22:03:36");
System.out.println("Date: SimpleDateFormat.parse::: " + dateParse);
```

##  &#127800; Calendar &#127800; 

```java

public class CalendarTest {
    public static void main(String[] args) throws ParseException {
        // 其日历字段已由当前日期和时间初始化：
        Calendar rightNow = Calendar.getInstance(); // 子类对象
        // 获取年
        int year = rightNow.get(Calendar.YEAR);
        // 获取月
        int month = rightNow.get(Calendar.MONTH);
        // 获取日
        int date = rightNow.get(Calendar.DATE);
        //获取几点
        int hour=rightNow.get(Calendar.HOUR_OF_DAY);
        //获取上午下午
        int moa=rightNow.get(Calendar.AM_PM);
        if(moa==1)
            System.out.println("下午");
        else
            System.out.println("上午");

        System.out.println(year + "年" + (month + 1) + "月" + date + "日"+hour+"时");
        rightNow.add(Calendar.YEAR,5);
        rightNow.add(Calendar.DATE, -10);
        int year1 = rightNow.get(Calendar.YEAR);
        int date1 = rightNow.get(Calendar.DATE);
        System.out.println(year1 + "年" + (month + 1) + "月" + date1 + "日"+hour+"时");

        System.out.println(rightNow);
        rightNow.setTime(new SimpleDateFormat("yyyy-MM-dd").parse("1997-10-26"));
        System.out.println(rightNow);
        rightNow.setTimeInMillis(new SimpleDateFormat("yyyy-MM-dd").parse("1970-12-17").getTime());
        System.out.println(rightNow);
    }
}
```


##  &#127800; mysql日期相关 &#127800; 

```sql
select curdate()-- 当前日期
,date_add(curdate(),interval -day(curdate())+1 day) -- 本月第一天
,last_day(curdate()) -- 本月最后一天
,date_add(date_add(curdate(),interval -day(curdate())+1 day),interval -1 month) -- 上月第一天
,date_add(curdate(),interval -1 month) -- 上月同期
,year(now()) -- 当前年
,


select CURRENT_TIMESTAMP(); -- 当前日期 + 时间
select current_date(); -- 当前日期
select now();
```