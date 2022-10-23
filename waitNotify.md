
注意，只有成为monitor的owner才能调用下面三个方法
```
obj.wait()
obj.notify()
obj.notifyAll()
```
不然会有java.lang.IllegalMonitorStateException

![image](https://user-images.githubusercontent.com/46443218/197408890-cdb3c684-9dcf-40d0-8868-8f9d2ea0f5e2.png)
