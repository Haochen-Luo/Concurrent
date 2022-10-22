### 经典并发错误及解决
- 两个线程共享count变量，进行相同数目的count++和count--，预期结果是不变的，但是结果会错误
- 错误原因: count++和count--并不是原子操作，工作内存修改的值还没有写回main memory里面
- 加了synchronized,多个线程中会有一个线程A获得进入synchronized代码块的权利，即使发生时间片耗尽轮到别的线程，别的线程进入会被blocked,后面这个线程A又可以轮到并且执行。线程A完成后唤醒其他线程

```java
public class ObjectLockdemo {

    static int counter = 0;
    static Object lock = new Object();

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(()->{
            for (int i = 0;i<5000;i++){
//                synchronized (lock){
                counter++;
//                }
            }
        },"t1");

        Thread t2 = new Thread(()->{for(int j=0;j<5000;j++){
//            synchronized (lock){
                counter--;
//            }
        }},"t2");

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println(counter);
    }
}

```

加了synchronized的代码，结果运行是正确的
注意不要忘记最后t1.join();t2.join();
```java

public class ObjectLockdemo {

    static int counter = 0;
    static Object lock = new Object();

    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(()->{
            for (int i = 0;i<5000;i++){
                synchronized (lock){
                counter++;
                }
            }
        },"t1");

        Thread t2 = new Thread(()->{for(int j=0;j<5000;j++){
            synchronized (lock){
                counter--;
            }
        }},"t2");

        t1.start();
        t2.start();
        t1.join();
        t2.join();

        System.out.println(counter);
    }
}

```
![image](https://user-images.githubusercontent.com/46443218/197335855-cea19ff3-b992-431a-ac75-4316e55fe44e.png)

![image](https://user-images.githubusercontent.com/46443218/197335904-235c3d82-c83b-40cf-a34e-ee5ff7366b29.png)
