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
```
thread1
thread2
thread3
```
再次运行时，结果如下所示。
```
thread1
thread3
thread2
```
第三次运行时，结果如下所示。
```
thread2
thread3
thread1
```
可以看到，每次运行程序时，线程的执行顺序可能不同。线程的启动顺序并不能决定线程的执行顺序。


加了synchronized还是无法确保按照顺序执行，因为有可能时间片先给t2
![image](https://user-images.githubusercontent.com/46443218/197407729-9e197b60-5aca-4ccc-a7d7-e23fc36817f7.png)


使用join可以保证顺序进行
```java

@Slf4j(topic = "waitDemo")
public class waitNotifyDemo {
    static Object lock = new Object();
    public static void main(String[] args) throws InterruptedException {
        for(int i =0;i<1000;i++){
        Thread t1 = new Thread(()->{
//        synchronized (lock){

            log.debug("| running");
//        }
        },"t1");
        Thread t2 =new Thread(()->{
//            synchronized (lock){
                log.debug("- running");
//            }
            },"t2");
        t1.start();
        t1.join();
        t2.start();
        t2.join();
    }
    }
}
```


线程八锁https://www.bilibili.com/video/BV16J411h7Rd?p=60&vd_source=8d4c66e89257a55f9732ff93790086b8

这里先1后2 或者先 2后1 都有可能,synchronized的作用是互斥,没法推断先后顺序
![image](https://user-images.githubusercontent.com/46443218/201550761-3930e79a-aa0f-47f0-8713-526085770ae4.png)

synchronized加在方法上，锁的是this对象
synchronized的作用是互斥，所以后面即使线程1休眠，线程2也会在其休眠时等待
这里先1后2 或者先 2后1 都有可能
![image](https://user-images.githubusercontent.com/46443218/201551043-27d35f6e-5fcc-4868-ab7a-9152ca55c1cf.png)

这种就是加在不同的对象上，没有互斥的作用，因为线程1的休眠，所以总是线程2执行优先
![image](https://user-images.githubusercontent.com/46443218/201551314-40e13eeb-e12b-4770-ac87-cddd598ec77a.png)

