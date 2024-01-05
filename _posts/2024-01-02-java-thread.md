---
layout: post
title: Java 多线程
---

### Java中如何使用线程
1.继承 Thread 类（本质上是实现 Runnable）

```java
Thread1 thread1 = new Thread1();
thread1.start();

static class Thread1 extends Thread {
    @Override
    public void run() {
        System.out.println("继承 Thread 类");
    }
}
```

2.实现 Runnable 接口

```java
Thread thread2 = new Thread(new Thread2());
thread2.start();

static class Thread2 implements Runnable {
    @Override
    public void run() {
        System.out.println("实现 Runnable 接口");
    }
}
```

3.实现 Callable 接口（带返回值）

```java
FutureTask<String> futureTask = new FutureTask<>(new Thread3());
Thread thread3 = new Thread(futureTask);
thread3.start();
try {
    String result = futureTask.get();
    System.out.println(result);
} catch (InterruptedException | ExecutionException e) {
    throw new RuntimeException(e);
}


static class Thread3 implements Callable<String> {
    @Override
    public String call() throws Exception {
        TimeUnit.SECONDS.sleep(1);
        return "实现 Callable 接口";
    }
}
```

4.使用线程池（推荐使用 ThreadPoolExecutor 而不是 Executors）

```java
/*
四种阻塞队列：
    ArrayBlockingQueue：基于数组、有界，按 FIFO（先进先出）原则对元素进行排序；
    LinkedBlockingQueue：基于链表，按FIFO （先进先出） 排序元素，吞吐量通常要高于 ArrayBlockingQueue；
    SynchronousQueue：每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于 LinkedBlockingQueue；
    PriorityBlockingQueue：具有优先级的、无限阻塞队列。
线程拒绝策略：
    CallerRunsPolicy：如果发现线程池还在运行，就直接运行这个线程;
    DiscardOldestPolicy：在线程池的等待队列中，将头取出一个抛弃，然后将当前线程放进去;
    DiscardPolicy：默默丢弃,不抛出异常;
    AbortPolicy：java默认，抛出一个异常（RejectedExecutionException）。
  */
ThreadPoolExecutor threadPoolExecutor = new ThreadPoolExecutor(
        2, // int corePoolSize: 核心线程数，默认线程个数为 0，只有接到任务才新建线程
        4, // int maximumPoolSize: 最大线程数
        10, // long keepAliveTime: 线程池空闲时，线程存活的时间，当线程池中的线程数大于 corePoolSize 时才会起作用
        TimeUnit.SECONDS, // TimeUnit unit: 时间单位
        new LinkedBlockingQueue<>(10), // BlockingQueue<Runnable> workQueue: 阻塞队列，当达到线程数达到 corePoolSize 时，将任务放入队列等待线程处理
        Executors.defaultThreadFactory(), // ThreadFactory threadFactory: 线程创建工厂
        new ThreadPoolExecutor.AbortPolicy() // RejectedExecutionHandler handler: 线程拒绝策略，当队列满了并且线程个数达到 maximumPoolSize 后采取的策略
);
threadPoolExecutor.submit(new Thread1());
threadPoolExecutor.submit(new Thread2());
Future<String> future = threadPoolExecutor.submit(new Thread3());
try {
    String result = future.get();
    System.out.println(result);
} catch (InterruptedException | ExecutionException e) {
    throw new RuntimeException(e);
}
```

### Java线程原理
![Java线程原理](https://cdn.jsdelivr.net/gh/luyeeya/picx-images-hosting@master/programming/Java线程原理.4crhbm33ovm0.png)

### Java线程的生命周期
Java 线程有 6 种状态：

![Java线程状态](https://cdn.jsdelivr.net/gh/luyeeya/picx-images-hosting@master/programming/Java线程状态.3dhvwntpd6m0.png)

1.`NEW`：初始状态，线程被构建，但是还没有调用 start 方法

```java
NewThread newThread = new NewThread();
System.out.println(newThread.getState());

static class NewThread extends Thread {}
```

2.`RUNNABLE`：运行状态，Java 线程对操作系统中的`就绪`和`运行`两种状态的统称

```java
RunnableThread runnableThread = new RunnableThread();
runnableThread.start();

static class RunnableThread extends Thread {
    @Override
    public void run() {
        System.out.println(this.getState());
    }
}
```

3.`WAITING`: 等待状态

```java
WaitingThread waitingThread = new WaitingThread();
waitingThread.start();
TimeUnit.SECONDS.sleep(1);
System.out.println(waitingThread.getState());

static class WaitingThread extends Thread {
    @Override
    public void run() {
        synchronized (WaitingThread.class) {
            try {
                WaitingThread.class.wait();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

4.`TIMED_WAITING`: 超时等待，超时以后自动返回

```java
TimedWaitingThread timedWaitingThread = new TimedWaitingThread();
timedWaitingThread.start();
TimeUnit.SECONDS.sleep(1);
System.out.println(timedWaitingThread.getState());

static class TimedWaitingThread extends Thread {
    @Override
    public void run() {
        try {
            TimeUnit.SECONDS.sleep(5);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }
}
```

5.`BLOCKED`: 锁阻塞（因为没有抢占到锁而阻塞）

```java
BlockedThread blockedThread1 = new BlockedThread();
BlockedThread blockedThread2 = new BlockedThread();
blockedThread1.start();
TimeUnit.SECONDS.sleep(1);
blockedThread2.start();
TimeUnit.SECONDS.sleep(1);
System.out.println(blockedThread2.getState());

static class BlockedThread extends Thread {
    @Override
    public void run() {
        synchronized (BlockedThread.class) {
            try {
                TimeUnit.SECONDS.sleep(5);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }
}
```

6.`TERMINATED`：终止状态，表示当前线程执行完毕

```java
TerminatedThread terminatedThread = new TerminatedThread();
terminatedThread.start();
TimeUnit.SECONDS.sleep(1);
System.out.println(terminatedThread.getState());

static class TerminatedThread extends Thread {}
```

**如何查看 Java 线程状态：**
```bash
# 1.找到需要查看状态的 Java 进程id
jps

# 2.查看对应进程的线程信息
jstack {进程id}
```


### 如何安全停止 Java 线程
**停止 Java 线程有两种方式：**
1. 使用 `stop()` 方法强行停止，不推荐使用，可能导致数据出现问题
2. 使用 `interrupt()` 方法停止，推荐。它的原理是从外部发送一个中断信号（标记位属性改为 `false`，通过 `isInterrupted()` 方法查看）给目标线程（如果目标线程处于阻塞状态（`WAITING`/`TIMED_WAITING`/`BLOCKED`），会将其唤醒），目标线程收到中断信号后，由其自行决定是否停止。

**安全停止线程的示例代码：**
```java
public class StopThread {
    public static void main(String[] args) throws InterruptedException {
        MyThread thread = new MyThread("StopThread");
        thread.start();
        TimeUnit.SECONDS.sleep(5);
        // 外部发送中断信号给线程
        thread.interrupt();
        TimeUnit.SECONDS.sleep(2);
        System.out.println(thread.getName() + " is " + thread.getState());
    }

    static class MyThread extends Thread {
        public MyThread(String name) {
            super(name);
        }

        @Override
        public void run() {
            while (!Thread.currentThread().isInterrupted()) {
                System.out.println(Thread.currentThread().getName() + " is " +
                        Thread.currentThread().getState());
                try {
                    TimeUnit.SECONDS.sleep(1);
                } catch (InterruptedException e) {、
                    // 抛出 InterruptedException 异常后会复位中断标记为 false
                    // 如果决定停止线程，再调用 interrupt() 将中断标记置为 true
                    Thread.currentThread().interrupt();
                }
            }
        }
    }
}
```
> interrupt() 有两个作用：
>   1.唤醒处于阻塞状态的线程
>   2.修改中断标记为 true

> 除了 InterruptedException，Thread.interrupted() 也可以复位中断标记

**在计算机领域，安全停止程序的方法：**
1. 停止接收新的请求
2. 处理完已经接收的请求
3. 停止程序


### 线程问题排查
#### CPU占用率很高，并且响应很慢
可能存在死循环：
1. `top` 找到最消耗 CPU 的进程 id
2. `top -H -p {进程id}` 找到进程里面最消耗 CPU 的线程
3. `print "0x%x" {线程id}` 转换成 16 进制
4. `jstack {进程id} | grep {16进制线程id}` 找到消耗 CPU 的线程堆栈

#### CPU占用率不高，但是响应很慢
可能存在死锁：
jps 找到进程 id，jstack 查看进程的线程信息：
```bash
"threadB" #13 prio=5 os_prio=0 tid=0x000001da24c85800 nid=0x39ac waiting for monitor entry [0x0000009c119ff000] 
   java.lang.Thread.State: BLOCKED (on object monitor)
        at dev.luyee.problem.DeadLock$DeadLockRunnableB.run(DeadLock.java:41)
        - waiting to lock <0x000000076bf6b3a8> (a java.lang.Class for dev.luyee.problem.DeadLock$DeadLockRunnableA)
        - locked <0x000000076bf6e768> (a java.lang.Class for dev.luyee.problem.DeadLock$DeadLockRunnableB)
        at java.lang.Thread.run(Thread.java:748)

"threadA" #12 prio=5 os_prio=0 tid=0x000001da24c82800 nid=0x5970 waiting for monitor entry [0x0000009c118ff000]
   java.lang.Thread.State: BLOCKED (on object monitor)
        at dev.luyee.problem.DeadLock$DeadLockRunnableA.run(DeadLock.java:24)
        - waiting to lock <0x000000076bf6e768> (a java.lang.Class for dev.luyee.problem.DeadLock$DeadLockRunnableB)
        - locked <0x000000076bf6b3a8> (a java.lang.Class for dev.luyee.problem.DeadLock$DeadLockRunnableA)
        at java.lang.Thread.run(Thread.java:748)

Found one Java-level deadlock:
=============================
"threadB":
  waiting to lock monitor 0x000001da22727fa8 (object 0x000000076bf6b3a8, a java.lang.Class),
  which is held by "threadA"
"threadA":
  waiting to lock monitor 0x000001da2272a628 (object 0x000000076bf6e768, a java.lang.Class),
  which is held by "threadB"
```


#### 内存问题：内存溢出、内存泄漏
dump 出来，用 MAT 工具分析：
1. 使用 `jmap -dump:format=b,file=heap.hprof {进程id}` 命令 dump 出来内存信息
2. 使用 `内存分析工具 MAT（Memory Analyzer）`分析 dump 出来的内存信息

> MAT 可以从 https://eclipse.dev/mat/previousReleases.php 下载，1.10.0 版本支持 jdk 1.8