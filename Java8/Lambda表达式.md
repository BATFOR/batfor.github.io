Jdk8中Lambda模块，使java又多了一种新的编程方式，函数式编程，也就是lambda表达式。什么是函数式编程？  

## 函数式编程 ##
**函数式编程**（英语：functional programming）或称函数程序设计，又称泛函编程，是一种编程典范，它将电脑运算视为数学上的函数计算，并且避免使用程序状态以及易变对象。函数编程语言最重要的基础是λ演算（lambda calculus）。而且λ演算的函数可以接受函数当作输入（引数）和输出（传出值）。比起指令式编程，函数式编程更加强调程序执行的结果而非执行的过程，倡导利用若干简单的执行单元让计算结果不断渐进，逐层推导复杂的运算，而不是设计一个复杂的执行过程。这是维基百科给出的定义。从这个我们知道函数式编程是相对于指令式编程的一种编程典范，并且对而言具有一些优点。  


Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。使用 Lambda 表达式可以使代码变的更加**简洁紧凑**。  

## 语法 ##
```java
(parameters) -> expression
或
(parameters) ->{ statements; }
```
以下是lambda表达式的重要特征（**括号、声明、返回值**）:  
* 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
* 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号。
* 可选的大括号：如果主体包含了一个语句，就不需要使用大括号。
* 可选的返回关键字：如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值。 

## 使用样例 ##
```java
public class Java8Tester {
   public static void main(String args[]){
      Java8Tester tester = new Java8Tester();
        
      // 类型声明
      MathOperation addition = (int a, int b) -> a + b;  //简化接口实现方式
        
      // 不用类型声明
      MathOperation subtraction = (a, b) -> a - b;
        
      // 大括号中的返回语句
      MathOperation multiplication = (int a, int b) -> { return a * b; };
        
      // 没有大括号及返回语句
      MathOperation division = (int a, int b) -> a / b;
        
      System.out.println("10 + 5 = " + tester.operate(10, 5, addition));
      System.out.println("10 - 5 = " + tester.operate(10, 5, subtraction));
      System.out.println("10 x 5 = " + tester.operate(10, 5, multiplication));
      System.out.println("10 / 5 = " + tester.operate(10, 5, division));
        
      // 不用括号
      GreetingService greetService1 = message ->
      System.out.println("Hello " + message);
        
      // 用括号
      GreetingService greetService2 = (message) ->
      System.out.println("Hello " + message);
        
      greetService1.sayMessage("Runoob");
      greetService2.sayMessage("Google");
   }
    
   interface MathOperation {
      int operation(int a, int b);
   }
    
   interface GreetingService {
      void sayMessage(String message);
   }
    
   private int operate(int a, int b, MathOperation mathOperation){
      return mathOperation.operation(a, b);
   }
}
```
输出结果  
```java
10 + 5 = 15
10 - 5 = 5
10 x 5 = 50
10 / 5 = 2
Hello Runoob
Hello Google
```  

```java
import java.text.Collator;
import java.util.Arrays;
import java.util.List;
import java.util.Locale;

public class Test {
    public static void main(String[] args) {    
        List<String> list = Arrays.asList("谷歌", "腾讯", "百度", "淘宝");    
        Collator collator = Collator.getInstance(Locale.CHINA);    
        list.sort((string1, string2) -> collator.compare(string1, string2));    
        System.out.println(list);
    }
}
```
```java
public class Demo01Inner {
    public static void main(String[] args) {
        //使用匿名内部类的方式实现多线程。
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println(Thread.currentThread().getName() + "执行了");
            }
        }).start();

        //使用Lambda表达式实现多线程
        new Thread(() -> System.out.println(Thread.currentThread().getName() + "执行了")).start();
    }
}
```
```java
public class Demo03Collections {
    public static void main(String[] args) {
        //创建集合
        List<Student> list = new ArrayList<>();
        //添加元素
        list.add(new Student("嫐", 20));
        list.add(new Student("嬲", 18));
        list.add(new Student("挊", 22));
        //使用比较器排序对集合中的学生对象根据年龄升序排序
        //Collections.sort(list, new Rule());

        //使用匿名内部类
        Collections.sort(list, new Comparator<Student>() {
            @Override
            public int compare(Student o1, Student o2) {
                return o1.getAge() - o2.getAge();
            }
        });
       
        //使用Lambda表达式
        Collections.sort(list, (Student o1, Student o2) -> {
            return o1.getAge() - o2.getAge();
        });

        Collections.sort(list, (o1, o2) -> o1.getAge() - o2.getAge());

        System.out.println(list);

    }
}
```

## 注意 ##
* 变量的作用域问题：lambda 表达式内可以访问外部类的所有属性成员变量；lambda 表达式引用(不修改)外层局部变量时，要保证此局部变量必须**不可被后面的代码修改**（即隐性的具有 final 的语义）。和匿名内部类差不多。
* Lambda 表达式当中不允许声明一个与局部变量同名的参数或者局部变量。

## Lambda 表达式的使用前提: ##
* 必须有**接口**（不能是抽象类），接口中有且仅有**一个**需要被重写的抽象方法。
* 必须**支持上下文推导**，要能够推导出来 Lambda 表达式表示的是哪个接口中的内容。

总的来说，通过lambda表达式，我们可以替换以前匿名内部类的书写方式，可以简化某些接口(函数式接口)的实现。可以使用接口当做参数，然后传递 Lambda 表达式(常用)，也可以将 Lambda 表达式赋值给一个接口类型的变量。  


