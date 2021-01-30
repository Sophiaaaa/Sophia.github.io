<!-- TOC -->

- [线程与进程](#线程与进程)
- [使用线程池的好处](#使用线程池的好处)
- [线程安全问题](#线程安全问题)
- [守护线程](#守护线程)
- [QA](#QA)
<!-- /TOC -->

## 线程与进程
- 进程是拥有资源和独立运行的最小单位，也是程序执行的最小单位。进程之间资源**互相独立，不共享**
- **一个进程可以有多个线程**
- 同一进程中的不同线程**栈内存独立**，但是共享方法区和堆内存
- 创建线程 方法一：
```java
class MyThread extends Thread {
    @Override
    public void run() {
        // 方法体
    }
}
```
- 创建线程 方法二：
```java
class MyThread2 implements Runnable{
    @Override
    public void run() {
        //方法体
    }
}


```
- 创建线程 方法三
```java
    public static void main(String[] args) {
        Thread thread = new Thread(new Runnable() {
            @Override
            public void run() {
                //方法体
            }
        });
    }
}
```
## 线程安全问题
- 局部变量永远不会有线程安全问题，因为局部变量不共享。（一个线程一个栈）
- 实例变量在堆中，堆只有一个。
- 静态变量在方法区当中，方法区只有一个。
- 堆和方法区都是多线程共享的，所以可能会存在线程安全问题。

## synchronized出现在实例方法上，锁的一定是this
### 缺点
- 锁住的一定是this，不灵活
- 整个实例方法都要同步，可能会无故扩大同步范围，导致程序的执行效率降低。
### 优点
- 代码写的少

## 如果使用局部变量的话：
- 建议使用：StringBuilder（因为局部变量不存在线程安全问题，选择StringBuilder，而StringBuilder效率比较低

- ArrayList是非线程安全的
- Vector是线程安全的
- HashMap HashSet是非线程安全的
- HashTable是线程安全的

## synchronized三种写法：
###  第一种：同步代码块
- 灵活
```java
        synchronized (线程共享对象) {
          //同步代码块
      }
```
### 第二种：在实例方法上使用synchronized
- 表示共享对象一定是this, 并且同步代码块是整个方法体，如果一个对象都调用这个方法，需要等待锁
```java
    public synchronized 返回值类型 方法名() {
        同步代码块
      }
```
### 第三种：在静态方法中使用synchronized
-      表示找类锁
-      类锁永远只有1把
-      就算创建了100个对象，类锁也只有1把

## 应该怎么解决线程安全问题？
### 是一上来就选择线程同步(synchronized)吗？
-  系统的用户吞吐量降低，用户体验变差。在不得已的情况下再选择线程同步机制。

### 第一种方案：
-  尽量使用局部变量代替“实例变量和静态变量”
###  第二种方案：
-  如果必须使用实例变量，那么可以考虑创建多个对象，这样实例变量的内存就不共享了。（一个线程对应一个对象，100个线程对应100个对象，对象不共享，就没有数据安全问题了）
### 第三种方案：
-  如果泵使用局部变量，对象也不能创建多个，这时只能选择synchronized线程同步机制了。

## 守护线程
- 一般守护线程就是一个死循环，所有的用户线程只要结束，守护线程自动结束
- java语言分为两个大类：用户线程（如main主线程），守护线程
```java
public class ThreadTest14 {
    public static void main(String[] args) {
        Thread t = new BakDataThread();
        t.setName("备份数据的线程");
        // 启动线程之前，将线程备份类守护线程
        t.setDaemon(true);
        t.start();

        // 主线程：主线程是用户线程
        for (int i = 0; i < 10; i++){
            System.out.printf(Thread.currentThread().getName() + "---->" + i);
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}

class BakDataThread extends Thread {
    public void run(){
        int i = 0;
        while (true){
            System.out.printf(Thread.currentThread().getName() + "--->" + (++i));
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }
}
```

## QA
### 线程中run()和start() 方法的区别？
- start()方法的任务是开启一个新的栈空间，之后程序会自动给调用run()方法。run()方法如果直接使用，就不是多线程了。

## 使用线程池的好处

- **池化技术相比大家已经屡见不鲜了，线程池、数据库连接池、Http 连接池等等都是对这个思想的应用。池化技术的思想主要是为了减少每次获取资源的消耗，提高对资源的利用率。**

**线程池**提供了一种限制和管理资源（包括执行一个任务）。 每个**线程池**还维护一些基本统计信息，例如已完成任务的数量。

- **降低资源消耗**。通过重复利用已创建的线程降低线程创建和销毁造成的消耗。
- **提高响应速度**。当任务到达时，任务可以不需要的等到线程创建就能立即执行。
- **提高线程的可管理性**。线程是稀缺资源，如果无限制的创建，不仅会消耗系统资源，还会降低系统的稳定性，使用线程池可以进行统一的分配，调优和监控。
