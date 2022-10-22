## Start
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
Output
```
NEW
RUNNABLE
startingt1
```


## join
想要让t1线程完成，就调用t1.join()
使用join
```java
import lombok.extern.slf4j.Slf4j;

@Slf4j(topic = "joinDemo")
public class threadJoinDemo {
    static int r =1;
    public static void main(String[] args) throws InterruptedException {

        log.debug("create thread");
        Thread t = new Thread(()->{
            log.debug("start");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            log.debug("running");
            log.debug("end");
            r = 10;
        },"t1");
        t.start();
        t.join();
        log.debug("r is: "+r);

    }
}
```

```
20:45:37.783 [main] DEBUG joinDemo - create thread
20:45:37.827 [t1] DEBUG joinDemo - start
20:45:38.838 [t1] DEBUG joinDemo - running
20:45:38.838 [t1] DEBUG joinDemo - end
20:45:38.838 [main] DEBUG joinDemo - r is: 10
```

不使用join
```java
import lombok.extern.slf4j.Slf4j;

@Slf4j(topic = "joinDemo")
public class threadJoinDemo {
    static int r =1;
    public static void main(String[] args) throws InterruptedException {

        log.debug("create thread");
        Thread t = new Thread(()->{
            log.debug("start");
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            log.debug("running");
            log.debug("end");
            r = 10;
        },"t1");
        t.start();
//        t.join();
        log.debug("r is: "+r);

    }
}
```

```
20:47:32.060 [main] DEBUG joinDemo - create thread
20:47:32.106 [main] DEBUG joinDemo - r is: 1
20:47:32.106 [t1] DEBUG joinDemo - start
20:47:33.109 [t1] DEBUG joinDemo - running
20:47:33.109 [t1] DEBUG joinDemo - end

```
