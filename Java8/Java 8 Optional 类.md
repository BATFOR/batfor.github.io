# Java 8 Optional 类 #
首先Optional是一个容器对象，并且可以存放null值，这个类的引入很好的解决了空指针异常的问题。  

## Optional类的常用方法（支持链式操作） ##


序号|方法&描述
 -|-
1| static <T> Optional<T> empty()    返回空的 Optional 实例。
2| boolean equals(Object obj) 判断其他对象是否等于 Optional。
3| Optional<T> filter(Predicate<? super <T> predicate) 如果值存在，并且这个值匹配给定的 predicate，返回一个Optional用以描述这个值，否则返回一个空的Optional。
4| <U> Optional<U> flatMap(Function<? super T,Optional<U>> mapper) 如果值存在，返回基于Optional包含的映射方法的值，否则返回一个空的Optional
5| T get() 如果在这个Optional中包含这个值，返回值，否则抛出异常：NoSuchElementException
6| int hashCode() 返回存在值的哈希码，如果值不存在 返回 0。
7| void ifPresent(Consumer<? super T> consumer) 如果值存在则使用该值调用 consumer , 否则不做任何事情。
8| boolean isPresent() 如果值存在则方法会返回true，否则返回 false。
9| <U>Optional<U> map(Function<? super T,? extends U> mapper) 如果有值，则对其执行调用映射函数得到返回值。如果返回值不为 null，则创建包含映射返回值的Optional作为map方法返回值，否则返回空Optional。
10 | static <T> Optional<T> of(T value) 返回一个指定非null值的Optional。
11 | static <T> Optional<T> ofNullable(T value) 如果为非空，返回 Optional 描述的指定值，否则返回空的 Optional。
12 | T orElse(T other) 如果存在该值，返回值， 否则返回 other。
13 | T orElseGet(Supplier<? extends T> other) 如果存在该值，返回值， 否则触发 other，并返回 other 调用的结果。
14 | <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) 如果存在该值，返回包含的值，否则抛出由 Supplier 继承的异常
15| String toString() 返回一个Optional的非空字符串，用来调试  

## orElse() 和 orElseGet() 的区别 ##
* 无论Optional对象中的值**是否为空**，orElse()函数**都会执行**；而由于orElseGet()中的参数为一个Supplier方法，该方法的特点是仅在必要的时候执行，因此只有在Optional对象中的值为空时，orElseGet()中的Supplier方法才会执行。

* 由于orElse()和orElseGet()执行过程的差异，orElseGet()方法的执行效率相对而言也更快，这是因为他会跳过不必要的方法调用。  

* 因此，只有当默认值已经事先定义的情况下，才使用orElse()，否则使用orElseGet()更好。


## 总结 ##
* Optional 不是 Serializable！！！！因此，它不应该用作类的字段。如果你需要序列化的对象包含 Optional 值，Jackson 库支持把 Optional 当作普通对象
* Optional 主要**用作返回类型**。在获取到这个类型的实例后，如果它有值，你可以取得这个值，否则可以进行一些替代行为。
* 有**默认值**的情况下，使用orElse(),否则使用orElseGet()

[参考链接1](https://blog.csdn.net/xhd731568849/article/details/79532959)  
[参考链接2](https://www.jianshu.com/p/f256b54b8309)
