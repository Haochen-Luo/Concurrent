# Concurrent

Use Java and Scala to study concurrent problem.

This repo is associated with Oxford ACS concurrent algorithms and data structures


## 并发常识:

线程的执行顺序是不确定的
调用Thread的start()方法启动线程时，线程的执行顺序是不确定的。也就是说，在同一个方法中，连续创建多个线程后，调用线程的start()方法的顺序并不能决定线程的执行顺序。

例如，这里，看一个简单的示例程序，如下所示。
```java
/**
 * @description 线程的顺序，直接调用Thread.start()方法执行不能确保线程的执行顺序
 */
public class ThreadSort01 {
    public static void main(String[] args){
        Thread thread1 = new Thread(() -> {
            System.out.println("thread1");
        });
        Thread thread2 = new Thread(() -> {
            System.out.println("thread2");
        });
        Thread thread3 = new Thread(() -> {
            System.out.println("thread3");
        });

        thread1.start();
        thread2.start();
        thread3.start();
    }
}
```
在ThreadSort01类中分别创建了三个不同的线程，thread1、thread2和thread3，接下来，在程序中按照顺序分别调用thread1.start()、thread2.start()和thread3.start()方法来分别启动三个不同的线程。

那么，问题来了，线程的执行顺序是否按照thread1、thread2和thread3的顺序执行呢？运行ThreadSort01的main方法，结果如下所示。

thread1
thread2
thread3
再次运行时，结果如下所示。

thread1
thread3
thread2
第三次运行时，结果如下所示。

thread2
thread3
thread1
可以看到，每次运行程序时，线程的执行顺序可能不同。线程的启动顺序并不能决定线程的执行顺序。
