### 经典并发错误及解决

- 错误原因: count++和count--并不是原子操作

没有代码，共享count变量，结果会错误
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
