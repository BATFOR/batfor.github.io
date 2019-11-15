# Java 8 Stream #
Java 8 API添加了一个新的抽象称为流Stream，可以让你以一种声明的方式处理数据。  

Stream 使用一种类似用 SQL 语句从数据库查询数据的直观方式来提供一种对 Java 集合运算和表达的高阶抽象。  

像其他语言py、C#等也有此特性。  

这种风格将要处理的元素集合看作一种流， 流在管道中传输， 并且可以在管道的节点上进行处理， 比如筛选， 排序，聚合等。  

元素流在管道中经过中间操作（intermediate operation）的处理，最后由最终操作(terminal operation)得到前面处理的结果。  

```java
+--------------------+       +------+   +------+   +---+   +-------+
| stream of elements +-----> |filter+-> |sorted+-> |map+-> |collect|
+--------------------+       +------+   +------+   +---+   +-------+
```

# 什么是 Stream？ #
stream（流）是一个来自数据源的元素队列并支持聚合操作  

* 元素是特定类型的对象，形成一个队列。 Java中的Stream并不会存储元素，而是按需计算。
* 数据源 流的来源。 可以是集合，数组，I/O channel， 产生器generator 等。
* 聚合操作 类似SQL语句一样的操作， 比如filter, map, reduce, find, match, sorted等。  

和以前的Collection操作不同， Stream操作还有两个基础的特征：
* Pipelining: 中间操作都会返回流对象本身。 这样多个操作可以串联成一个管道， 如同流式风格（fluent style）。 这样做可以对操作进行优化， 比如延迟执行(laziness)和短路( short-circuiting)。
 *内部迭代： 以前对集合遍历都是通过Iterator或者For-Each的方式, 显式的在集合外部进行迭代， 这叫做外部迭代。 Stream提供了内部迭代的方式， 通过访问者模式(Visitor)实现。

# 生成流 #
在 Java 8 中, 集合接口有两个方法来生成流：
* stream() − 为集合创建串行流。
* parallelStream() − 为集合创建并行流。
```java
List<String> strings = Arrays.asList("abc", "", "bc", "efg", "abcd","", "jkl");
List<String> filtered = strings.stream().filter(string -> !string.isEmpty()).collect(Collectors.toList());
```

# Stream中常用方法 #
```java
import java.util.Arrays;
import java.util.IntSummaryStatistics;
import java.util.List;
import java.util.Random;
import java.util.stream.Collectors;

public class Stream_Test {

    public static void main(String[] args) {
        //创建初始集合
        List<Integer> integers = Arrays.asList(12, 23, 34, 45, 56, 67);
        List<String> strings = Arrays.asList("as", "sd", "df", "", "fg", "qwe", "zxcd");

        //Foreach迭代,limit方法
        //以下代码片段使用 forEach 输出了10个随机数：
        //limit 方法用于获取指定数量的流。 以下代码片段使用 limit 方法打印出 10 条数据：
        Random random = new Random();
        random.ints().limit(10).forEach(System.out::println);  //ints方法会返回一个IntStream流

        //map方法
        //map 方法用于映射每个元素到对应的结果，以下代码片段使用 map 输出了元素对应的平方数：
        integers.stream().map(x->x * x).collect(Collectors.toList()).forEach(System.out::println);

        //filter 方法用于通过设置的条件过滤出元素。以下代码片段使用 filter 方法过滤出空字符串：
        strings.stream().filter(string->!string.isEmpty()).collect(Collectors.toList()).forEach(System.out::println);

        //sorted 方法用于对流进行排序。以下代码片段使用 sorted 方法对输出的 10 个随机数进行排序：
        random.ints().limit(10).sorted().forEach(System.out::println);

        //Collectors 类实现了很多归约操作，例如将流转换成集合和聚合元素。Collectors 可用于返回列表或字符串：
        //合并字符串
        System.out.println( strings.stream().collect(Collectors.joining()));

        //统计
        //另外，一些产生统计结果的收集器也非常有用。它们主要用于int、double、long等基本类型上，它们可以用来产生类似如下的统计结果。
        IntSummaryStatistics intSummaryStatistics = integers.stream().mapToInt(x -> x).summaryStatistics();
        System.out.println("列表中最大的数 : " + intSummaryStatistics.getMax());
        System.out.println("列表中最小的数 : " + intSummaryStatistics.getMin());
        System.out.println("所有数之和 : " + intSummaryStatistics.getSum());
        System.out.println("平均数 : " + intSummaryStatistics.getAverage());
    }

}
```
**统计方法**  
![](https://github.com/BATFOR/MyImg/blob/master/20191115093236.png?raw=true)  

**Stream接口相关方法**   
![](https://github.com/BATFOR/MyImg/blob/master/20191115093648.png?raw=true)
![](https://github.com/BATFOR/MyImg/blob/master/20191115093708.png?raw=true)

# 总结 #
java8的StreamApi，可依然我们以一种声明的方式处理数据，可以采用类似Sql语句的方式操作java集合。比如说筛选， 排序，聚合等。首先流中的元素是特定类型的对象
，当然Stream并不会储存元素，而是按需存取。数据源一般来自集合、数组、i/o channel等。除此之外，Stream还有两个基础特征：pipelining，也就是说中间的操作返回的还是流本身，这可以对操作进行优化；
内部迭代，以前我们遍历集合都是Iterator或For-Each方式，这叫外部迭代(集合显示的在外部进行迭代)，而Stream提供了内部迭代，是通过访问者模式实现。
