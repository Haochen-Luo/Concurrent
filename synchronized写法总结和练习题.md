## synchronized只要后面是同一个对象，就实现了线程访问临界区时候的加锁
synchronized this可以直接加在方法上
```java
class Test{
  public synchronized void test(){
  }
}
```

等价于

```java
class Test{
  public void test(){
    synchronized(this){
    
    }
  }
}
```

synchronized也可以直接加载类上

```java
class Test{
  public synchronized static void test(){
  
  }
}
```

```java
class Test{
  public static void test(){
    synchronized(Test.class){
    
    }
  }
}
```


## 练习题
### 情况一
```java


import lombok.extern.slf4j.Slf4j;
@Slf4j(topic = "main")
public class syncPractice {
    public static void main(String[] args) {
        Number n = new Number();
        new Thread(()->{
            log.debug("begin");
            try {
                n.a();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();

        new Thread(()->{
            log.debug("begin");
            n.b();
        }).start();
    }

}
@Slf4j(topic = "number")
class Number{
    public synchronized void a() throws InterruptedException {
        Thread.sleep(1000);
        log.debug("1");
    }
    public synchronized void b() {

        log.debug("2");
    }

}
```

这里其实t1和t2是并发的情况！虽然t1有synchronized，sleep的时候还是会交出时间片。系统启动也不一定是t1,t2的顺序，可能是t2,t1的顺序
具体可以参见线程启动顺序那个md文件

所以这里的答案是2然后1s后为1（线程2先执行），或者是停1秒然后是2（虽然线程1在休眠但是因为有锁所以t2迟迟不能行动）

```
21:54:53.950 [Thread-0] DEBUG main - begin
21:54:53.950 [Thread-1] DEBUG main - begin
21:54:54.967 [Thread-0] DEBUG number - 1
21:54:54.967 [Thread-1] DEBUG number - 2
```
```
21:34:50.446 [Thread-1] DEBUG main - begin
21:34:50.450 [Thread-1] DEBUG number - 2
21:34:50.446 [Thread-0] DEBUG main - begin
21:34:51.463 [Thread-0] DEBUG number - 1
```
```
21:56:37.849 [Thread-1] DEBUG main - begin
21:56:37.849 [Thread-0] DEBUG main - begin
21:56:37.853 [Thread-1] DEBUG number - 2
21:56:38.861 [Thread-0] DEBUG number - 1
```

### 情况二
这里Number1的a()是锁类对象，而b()中锁的是对象number12，所以不存在加锁。又因为1有sleep,所以运行结果永远是2先1后
```java
Number1 number11 = new Number1();
        Number1 number12 = new Number1();
        new Thread(()->{
            log.debug("begin");
            try {
                number11.a();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();

        new Thread(()->{
            log.debug("begin");
            number12.b();
        }).start();

@Slf4j(topic = "case1")
class Number1{
    public static synchronized  void a() throws InterruptedException {
        Thread.sleep(1000);
        log.debug("1");
    }
    public synchronized void b() {

        log.debug("2");
    }

}
```

### 情况三
这里n1,n2对应都是Number2的类对象，所以加锁成功。和情形一一样
```java
//把main里换成下面这段。
 Number2 n1 = new Number2();
        Number2 n2 = new Number2();
        new Thread(()->{
            log.debug("begin");
            try {
                n1.a();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }).start();

        new Thread(()->{
            log.debug("begin");
            n2.b();
        }).start();
@Slf4j(topic = "number")
class Number2{
    public static synchronized void a() throws InterruptedException {
        Thread.sleep(1000);
        log.debug("1");
    }
    public static synchronized void b() {

        log.debug("2");
    }

}
```
