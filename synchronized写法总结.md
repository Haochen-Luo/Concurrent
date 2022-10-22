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
