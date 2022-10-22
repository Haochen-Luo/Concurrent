start后会从NEW变成RUNNABLE
```java
public class startDemo {
    public static void main(String[] args) {
        Thread t1 = new Thread(()->{
            System.out.println("starting"+Thread.currentThread().getName());
        },"t1");

        System.out.println(t1.getState());
        t1.start();
        System.out.println(t1.getState());
    }
}
```
