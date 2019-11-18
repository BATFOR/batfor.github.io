在Java8以前，关于日期时间处理的API( java.util.Date 、java.util.Calendar、java.util.TimeZone、java.sql)有很多不理想的地方，如：  
* 非线程安全
* 设计很差
* 时区处理麻烦   

但新的日期时间API中，做了很好的优化，这些类都位于java.time包下，其中有两个比较重要的API：
* Local(本地) − 简化了日期时间的处理，没有时区的问题。
* Zoned(时区) − 通过制定的时区处理日期时间。

## 本地相关API ##
```java
import java.time.*;
import java.time.format.DateTimeFormatter;
import java.time.format.FormatStyle;
import java.time.temporal.TemporalUnit;

public class NewDateTimeTest {
    public static void main(String[] args) {
        //日期类
        LocalDate now = LocalDate.now();
        System.out.println("当前日期为:" + now);
        //比较两个日期大小
        System.out.println("当前日期在在2020年12月12日后？->"+now.isAfter(LocalDate.of(2020,12,12)));//当前日期在在2020年12月12日后？->false

        //时间类
        LocalTime localTime = LocalTime.now();
        System.out.println("当前时间为:" + localTime);

        //日期时间类
        LocalDateTime localDateTime = LocalDateTime.now();
        //格式化输出
        DateTimeFormatter dateTimeFormatter = DateTimeFormatter.ofPattern("yyyy年MM月dd日 HH时mm分ss秒");
        System.out.println("当前日期时间为:" + localDateTime.format(dateTimeFormatter)); //当前日期时间为:2019年11月18日 15时24分42秒


        //Duration类，用于计算两个LocalTime或两个LocalDateTime的时间差，
        Duration between = Duration.between(LocalTime.now(), LocalTime.of(22, 10, 55));
        System.out.println("两个时间相差：" + between.getSeconds() + "秒");

        //Period类用于计算两个LocalDate之间的时间差
        Period between1 = Period.between(LocalDate.of(2019, 11, 11), LocalDate.of(2020, 12, 12));
        System.out.println("两个日期相差：" + between1.getYears() + "年" + between1.getMonths() + "月" + between1.getDays() + "天"); //两个日期相差：1年1月1天

        //MonthDay类只包含月日信息，可用于存放生日，纪恋日等信息。
        LocalDate birthday = LocalDate.of(1999, 11, 18);
        MonthDay monthDay = MonthDay.from(birthday);
        System.out.println("今天是我的生日吗？："+monthDay.equals(MonthDay.now()));

    }
}
```
在此只是列出了一些类的少许方法，比如像日期时间**加减比较**操作、获取**特定字段信息**也是有相关方法的，大家自己用的时候去尝试吧。

## 时区API ##
```java
import java.time.ZonedDateTime;
import java.time.ZoneId;

public class Java8Tester {
   public static void main(String args[]){
      Java8Tester java8tester = new Java8Tester();
      java8tester.testZonedDateTime();
   }
       
   public void testZonedDateTime(){
       
      // 获取当前时间日期
      ZonedDateTime date1 = ZonedDateTime.parse("2015-12-03T10:15:30+05:30[Asia/Shanghai]");
      System.out.println("date1: " + date1);
               
      ZoneId id = ZoneId.of("Europe/Paris");
      System.out.println("ZoneId: " + id);
               
      ZoneId currentZone = ZoneId.systemDefault();
      System.out.println("当期时区: " + currentZone);
   }
}
```




