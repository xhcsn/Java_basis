# Java并发
## 1、什么是进程和线程
进程是程序的一次执行过程，是系统运行程序的基本单位；线程和进程类似，但是线程是一个比线程更小的执行单位。一个进程在其执行过程中可以产生多个线程。同类的多个线程共享进程的堆与方法区资源，但是每个线程拥有自己的程序计数器，虚拟机栈以及本地方法栈。因此线程被称为轻量级进程。

## 2、线程与进程的关系、区别及优缺点
一个进程中可以有多个线程，多个线程共享进程的堆与方法区资源，但是每个线程有自己的程序计数器、虚拟机栈和本地方法栈。

线程是进程划分成的更小的运行单位，线程和进程的最大的不同在于基本上各进程是独立的，而各线程则不一定，因为同一进程中的线程极有可能会互相影响。线程执行开销小，但是不利于资源的管理和维护；而进程正好相反。

## 3、程序计数器为什么是私有的
程序计数器主要有以下两个作用：
- 字节码解释器通过改变程序计数器来依次读取指令，从而实现代码的流程控制；
- 在多线程的情况下，程序计数器用于记录当前进程执行的位置，从而当线程被切换回来的时候就能够知道该线程上次运行到哪儿了。

注：如果执行native方法的话，程序计数器记录的是undefined地址，只有执行的是Java代码时程序计数器记录的才是下一条指令的地址。

因此程序计数器私有主要是为了线程切换后能够恢复到正确的执行位置。

## 4、虚拟机栈和本地方法栈为什么是私有的
- 虚拟机栈：每一个Java方法在执行的时候会同时创建一个栈帧用于存储局部变量表、操作数栈、常量池引用等信息。从方法调用直至执行完成的过程，就对应着一个栈帧在Java虚拟机栈中入栈和出栈的过程。
- 本地方法栈类似于虚拟机栈，为本地方法服务。

因此，虚拟机栈和本地方法栈私有是为了保证线程中的局部变量不被别的线程访问到。

## 5、并发和并行的区别
- 并发：在同一时间段，多个任务都在执行（单位时间内不一定同时执行）；
- 并行：在同一个时间点，多个任务都在进行

## 6、为什么要使用多线程以及使用多线程可能带来什么问题
使用多线程是为了提高CPU的利用率；使用多线程会造成内存泄漏、死锁、线程不安全等问题。

## 7、线程的生命周期和状态
new、runnable、blocked、wating、time-waiting、terminated

线程创建之后将处于新建状态，调用start()方法后开始运行，线程这时候处于ready状态。可运行状态的线程获得了CPU时间片后就处于running状态。这两个状态笼统地称为runnable状态；当线程执行wait方法以后，线程进入了wating状态，执行wait(long millis)方法后，线程将会进入time-waiting状态。当现场调用同步方法的时候，无法获取到锁的情况下，线程将会进入到blocked状态。线程在执行完之后会进入terminated状态。

## 8、什么是上下文切换
多线程编程中一般线程的个数都大于 CPU 核心的个数，而一个 CPU 核心在任意时刻只能被一个线程使用，为了让这些线程都能得到有效执行，CPU 采取的策略是为每个线程分配时间片并轮转的形式。当一个线程的时间片用完的时候就会重新处于就绪状态让给其他线程使用，这个过程就属于一次上下文切换。

任务从保存到再加载的过程就是一次上下文切换。

## 9、死锁产生的四个必要条件以及如何预防和避免线程死锁
### 死锁产生的必要条件：
- 互斥：某种资源一次只允许一个进程访问，即该资源一旦分配给某个进程，其他进程就不能再访问，直到该进程访问结束。
- 占有且等待：一个进程可以占有资源并且进行等待其他进程释放资源
- 不可抢占：不可抢占其他线程拥有的资源
- 循环等待，进程之间需要的资源形成一个循环等待的资源。
#### 预防死锁：
- 一次性申请所有的资源
- 占用部分资源的线程进一步申请其他资源时，如果申请不到，可以主动释放它占有的资源
- 靠按序申请资源来预防。按某一顺序申请资源，释放资源则反序释放。破坏循环等待条件

## 10、sleep()方法和wait()方法区别和共同点
- 两者最主要的区别在于：sleep方法没有释放锁，而wait方法释放了锁
- 两者都可以暂停线程的执行：wait通常用于线程间交互/通信，sleep通常用于暂停执行
- wait()方法被调用后，线程不会自动苏醒，需要别的线程调用同一个对象上的notify()或者notifyAll()方法。sleep()方法执行完成后，线程会自动苏醒。或者可以使用wait(long timeout)超时后线程会自动苏醒。

## 11、为什么我们调用start()方法时会执行run()方法，为什么我们不能直接调用run()方法？
new一个Thread，线程进入了新建状态。调用start()方法，会启动一个线程并使线程进入了就绪状态，当分配到时间片后就可以开始运行了。start()会执行线程的相应准备工作，然后自动执行run()方法的内容，这是真正的多线程工作。但是，直接执行run()方法，会把run()方法当成一个main线程下的普通方法去执行，并不会在某个线程中执行它，所以这并不是多线程工作。

## 12、对synchronized关键字的理解
synchronized关键字解决的是多个线程之间访问资源的同步性，synchronized关键字可以保证被它修饰的方法或者代码块在任意时刻只能有一个线程执行。

## 13、synchronized关键字的三种使用方式
- 修饰实例方法：作用于当前对象实例加锁，进入同步代码块需要获得当前对象实例的锁
- 修饰静态方法：给当前类加锁，作用于类的所有对象实例，进入同步代码块前要获得当前class的锁
- 修饰代码块：指定加锁对象，对给定对象/类加锁，表示进入同步代码块前要获得给定对象的锁

## 14、手写单例模式
```java
public class Singleton{
    private volatile static Singleton uniqueInstance;
    private Singleton（）{

    }
    public static Singleton getUniqueInstance(){
        if(uniqueInstance == null){
            synchronized(Singleton.class){
                if(uniqueInstance == null){
                    uniqueInstance = new Singleton();
                }
            }
        }
        return uniqueInstance;
    }
}
```

## 15、synchronized关键字的底层原理
#### synchronized同步语句块的情况
```java
public class SynchronizedDemo{
    public void method(){
        synchronized(this){
            System.out.println("synchronize代码块");
        }
    }
}
```

synchronized同步语句块的实现使用的是monitorenter和monitorexit指令，其中monitorenter指令指向同步代码块的开始位置，monitorexit指令指向同步代码块的结束位置

当执行monitorenter时，线程视图获取锁也就是获取对象监视器的持有权

#### synchronized修饰方法的情况
```java
public class SynchronizedDemo2{
    public synchronized void method(){
        System.out.println("synchronized方法");
    }
}
```
synchronized修饰的方法并没有monitorenter指令和monitorexit指令，而是使用了ACC_SYNCHRONIZED标识，该标识指明了该方法是一个同步方法。

本质而言都是对对象监视器monitor的获取。

## 16、JDK1.6之后对synchronized关键字进行了哪些优化
优化后synchronized锁可以分为：无锁状态，偏向锁状态，轻量级锁状态和重量级锁状态，锁可以升级但是不可以降级

- 无锁：未进行加锁
- 偏向锁：针对一个进程而言，如果没有发生锁的竞争，当前对象的锁是偏向于当前进程的。
- 轻量级锁：当有两个线程竞争锁，当前锁就会进化成轻量级锁，但是此时资源的竞争不是很激烈，当A线程获取c对象，B线程获取不到c对象的锁就会进入自旋，不断尝试获取锁，因为竞争不激烈，所以在指定次数自旋内B线程就会获取轻量级锁，如果获取失败那么就会升级成重量级锁。
- 重量级锁状态：获取不到资源的线程会阻塞

## 17、synchronized和ReentrantLock的区别
- 两者都是可重入锁
- synchronized关键字依赖于JVM的实现，ReentrantLock依赖于API，即通过lock()、unlock()配合try/finally实现的
- ReentrantLock比起synchronized增加了高级功能：
  - 等待可中断，即正在等待的线程可以放弃等待转而处理其他事情
  - 可实现公平锁：ReentrantLock可以指定公平锁还是非公平锁，synchronized只能是非公平锁。所谓的公平锁就是先等待的线程先获得锁
  - 可以实现选择性通知：synchronized关键字与wait和notify方法相结合可以实现等待和通知、ReentrantLock可以通过Condition，选择性通知注册在指定Condition的所有等待线程

## 18、并发编程的三个重要特性
- 原子性：一组逻辑上的操作要么全部执行要么全部不执行
- 可见性：当一个线程对共享变量进行了修改，其他的线程立即可以看到修改后的最新值
- 有序性：代码在执行的过程中禁止指令重排

## 19、ThreadLocal原理
如果创建了一个ThreadLocal变量，那么访问这个变量的每个线程都会有这个变量的本地副本，可以使用get和set方法来获取默认值或者将其值更改为当前线程所存的副本的值，从而避免了线程安全问题。

**本质**：最终的变量是放在了当前线程的ThreadLocalMap中，并不是存在ThreadLocal上。

每个Thread都具备一个ThreadLocalMap，而ThreadLocalMap可以存放以ThreadLocal为kry，Object对象为value的键值对。

## 20、ThreadLocal内存泄露问题
ThreadLocalMap中使用的Key为ThreadLocal的弱引用，而value是强引用。所以，如果ThreadLocal没有被外部强引用的情况下，在垃圾回收的时候。key会被清理掉，而value不会被清理掉。这样一来，ThreadLocalMap中就会出现key为null的Entry，如果不做任何措施的话，value永远无法被GC回收，这个时候就有可能会产生内存泄露。

## 21、为什么要使用线程池
- 降低资源消耗
- 提高响应速度
- 提高线程的可管理性

## 22、实现Runnable接口和Callable接口的区别
Runnable接口不会返回结果或者抛出检查异常，但是Callable接口会返回结果或者抛出异常

## 23、执行execute和submit方法的区别是什么
- execute方法用于提交不需要返回值的任务，所以无法判断任务是否被线程池执行成功与否
- submit方法用于提交需要返回值的任务。线程池会返回一个Future类型的对象，通过这个Future对象可以判断任务是否执行成功。并且可以通过Future的get方法来获取返回值，get方法会阻塞当前线程直到任务完成，而使用get(long timeout, TimeUnit unit)方法则会阻塞当前线程一段时间之后立即返回，这个时候有可能任务没有执行完

## 24、如何创建线程池
可以通过ThreadPoolExecutor的方式去创建，其中ThreadPoolExecutor中有三个最重要的参数:
- corePoolSize：核心线程数 定义了最小可以同时运行的线程数量
- maximumPoolSize：当队列中存放的任务达到队列容量的时候，当前可以同时运行的线程数量变为最大线程数
- workQueue：当新任务来的时候会先判断当前运行的线程数量是否达到核心线程数，如果达到的话，新任务就会被存放在队列中

其他参数：
- keepAliveTime：当线程池中的线程数量大于核心线程数量的时候，核心线程以外的线程不会立即销毁，而是会等待，直到等待的时间超过了keepAliveTime才会被回收销毁
- unit：keepAliveTime参数的时间单位
- handler：饱和策略

## 25、ThreadPoolExecutor饱和策略
如果当前同时运行的线程数量达到了最大线程数量并且队列也已经被放满的时候，ThreadPoolExecutor会针对新来的线程定义一些策略：
- ThreadPoolExecutor.AbortPolicy：来拒绝新任务的处理。
- ThreadPoolExecutor.CallerRunsPolicy：直接在调用execute方法的线程中运行被拒绝的任务，如果执行程序已关闭，则会丢弃该任务。
- ThreadPoolExecutor.DiscardPolicy：不处理新任务，直接丢弃掉。
- ThreadPoolExecutor.DiscardOldestPolicy：此策略将丢弃最早的未处理的任务请求。

## 26、手写一个简单的线程池
首先创建一个Runnable接口的实现类
```java
//MyRunnable.java

import java.util.Date;

public class MyRunnable implements Runnable {
    private String command;
    public MyRunnable(String s) {
        this.command = s;
    }
    @override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " Start. Time = " + new Date());
        processCommand();
        System.out.println(Thread.currentThread().getName() + " End. Time = " + new Date());
    }
    private void processCommand() {
        try {
            Thread.sleep(5000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

使用ThreadPoolExecutor构造函数自定义参数的方式来创建线程池。

```java
//ThreadPoolExecutorDemo.java
import java.util.concurrent.ArrayBlockingQueue;
import java.util.concurrent.ThreadPoolExecutor;
import java.util.concurrent.TimeUnit;

public class ThreadPoolExecutorDemo {

    private static final int CORE_POOL_SIZE = 5;
    private static final int MAX_POOL_SIZE = 10;
    private static final int QUEUE_CAPACITY = 100;
    private static final Long KEEP_ALIVE_TIME = 1L;
    public static void main(String[] args) {

        //使用阿里巴巴推荐的创建线程池的方式
        //通过ThreadPoolExecutor构造函数自定义参数创建
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
                CORE_POOL_SIZE,
                MAX_POOL_SIZE,
                KEEP_ALIVE_TIME,
                TimeUnit.SECONDS,
                new ArrayBlockingQueue<>(QUEUE_CAPACITY),
                new ThreadPoolExecutor.CallerRunsPolicy());

        for (int i = 0; i < 10; i++) {
            //创建WorkerThread对象（WorkerThread类实现了Runnable 接口）
            Runnable worker = new MyRunnable("" + i);
            //执行Runnable
            executor.execute(worker);
        }
        //终止线程池
        executor.shutdown();
        while (!executor.isTerminated()) {
        }
        System.out.println("Finished all threads");
    }
}
```

## 27、Atomic原子类
- 基本类型
  - AtomicInteger：整形原子类
  - AtomicLong：长整型原子类
  - AtomicBoolean：布尔型原子类
- 数组类型
  - AtomicIntegerArray：整形数组原子类
  - AtomicLongArray：长整形数组原子类
  - AtomicReferenceArray：引用类型数组原子类
- 引用类型
  - AtomicReference：引用类型原子类
  - AtomicStampedReference：原子更新带有版本号的引用类型。该类将整数值与引用关联起来，可用于解决原子的更新数据和数据的版本号，可以解决使用 CAS 进行原子更新时可能出现的 ABA 问题。
  - AtomicMarkableReference ：原子更新带有标记位的引用类型
- 对象的属性修改类型
  - AtomicIntegerFieldUpdater：原子更新整形字段的更新器
  - AtomicLongFieldUpdater：原子更新长整形字段的更新器
  - AtomicReferenceFieldUpdater：原子更新引用类型字段的更新器
## 28、AtomicInteger的使用以及原理
AtomicInteger类常用方法
```java
public final int get() //获取当前的值
public final int getAndSet(int newValue)//获取当前的值，并设置新的值
public final int getAndIncrement()//获取当前的值，并自增
public final int getAndDecrement() //获取当前的值，并自减
public final int getAndAdd(int delta) //获取当前的值，并加上预期的值
boolean compareAndSet(int expect, int update) //如果输入的数值等于预期值，则以原子方式将该值设置为输入值（update）
public final void lazySet(int newValue)//最终设置为newValue,使用 lazySet 设置之后可能导致其他线程在之后的一小段时间内还是可以读到旧的值。
```

AtomicInteger类主要利用CAS(compare and swap)+volatile和native方法来保证原子操作，从而避免synchronized的高开销，执行效率大为提升。

CAS的原理是拿期望的值和原本的一个值作比较，如果相同则更新成新的值。UnSafe类的objectFieldOffset()方法是一个本地方法，这个方法是用来拿到“原来的值”的内存地址，返回值是valueOffset。另外value是一个volatile变量，在内存中可见，因此JVM可以保证任何时刻任何线程总能拿到该变量的最新值。

## 29、AQS介绍
AQS全称AbstractQueuedSynchronizer，这个类在java.util.concurrent.locks包下面。

AQS是一个用来构建锁和同步器的框架，使用AQS可以简单并且高效地构建出广泛大量的同步器。

#### AQS原理
AQS的核心思想是，如果被请求的共享资源空闲，则将当前请求资源的线程设置为有效的工作线程，并且将共享资源设置为锁定状态。如果被请求的共享资源被占用，那么就需要一套线程阻塞等待以及被唤醒时锁分配的机制，这个机制AQS是用CLH队列锁实现的，即将暂时获取不到锁的线程加入到队列中。

CLH队列是一个虚拟的双向队列，AQS是将每条请求共享资源的线程封装成一个CLH队列的一个结点来实现锁的分配

AQS使用一个int成员变量来表示同步状态，通过内置的FIFO队列来完成获取资源线程的排队工作。AQS使用CAS来对该同步状态进行原子操作实现对其值的修改。

```java
private volatile int state;//共享变量，使用volatile修饰保证线程可见性
```

#### AQS对资源的两种共享方式
- 独占：只有一个线程能够执行，如 ReentrantLock。又可分为公平锁和非公平锁：

        公平锁：按照线程在队列中的排队顺序，先到者先拿到锁
        非公平锁：当线程要获取锁时，无视队列顺序直接去抢锁，谁抢到就是谁的
- 共享：多个线程可同时执行，如 CountDownLatch、Semaphore、CyclicBarrier、ReadWriteLock

#### AQS底层使用了模板方法模式
同步器的设计是基于模板方法模式的，如果需要自定义同步器一般的方式是这样：
 -使用者继承AbstractQueuedSynchronizer并重写指定的方法
 - 将AQS组合在自定义同步器的实现中，并调用模板方法，而模板方法会调用使用者重写的方法。

--------------------------------------------------------
以 ReentrantLock 为例，state 初始化为 0，表示未锁定状态。A 线程 lock()时，会调用 tryAcquire()独占该锁并将 state+1。此后，其他线程再 tryAcquire()时就会失败，直到 A 线程 unlock()到 state=0（即释放锁）为止，其它线程才有机会获取该锁。当然，释放锁之前，A 线程自己是可以重复获取此锁的（state 会累加），这就是可重入的概念。但要注意，获取多少次就要释放多么次，这样才能保证 state 是能回到零态的。

----------------------------------------------------------------
再以 CountDownLatch 以例，任务分为 N 个子线程去执行，state 也初始化为 N（注意 N 要与线程个数一致）。这 N 个子线程是并行执行的，每个子线程执行完后 countDown() 一次，state 会 CAS(Compare and Swap)减 1。等到所有子线程都执行完后(即 state=0)，会 unpark()主调用线程，然后主调用线程就会从 await() 函数返回，继续后余动作。

#### AQS组件
- Semaphore(信号量)-允许多个线程同时访问： synchronized 和 ReentrantLock 都是一次只允许一个线程访问某个资源，Semaphore(信号量)可以指定多个线程同时访问某个资源。
- CountDownLatch （倒计时器）： CountDownLatch 是一个同步工具类，用来协调多个线程之间的同步。这个工具通常用来控制线程等待，它可以让某一个线程等待直到倒计时结束，再开始执行。
- CyclicBarrier(循环栅栏)： CyclicBarrier 和 CountDownLatch 非常类似，它也可以实现线程间的技术等待，但是它的功能比 CountDownLatch 更加复杂和强大。主要应用场景和 CountDownLatch 类似。CyclicBarrier 的字面意思是可循环使用（Cyclic）的屏障（Barrier）。它要做的事情是，让一组线程到达一个屏障（也可以叫同步点）时被阻塞，直到最后一个线程到达屏障时，屏障才会开门，所有被屏障拦截的线程才会继续干活。CyclicBarrier 默认的构造方法是 CyclicBarrier(int parties)，其参数表示屏障拦截的线程数量，每个线程调用 await() 方法告诉 CyclicBarrier 我已经到达了屏障，然后当前线程被阻塞。

#### CountDownLatch使用
CountDownLatch的作用是允许count个线程阻塞在一个地方，知道所有线程的任务都执行完毕

举例，要读取6个文件。这6个任务都没有执行顺序的依赖，因此可以定义一个线程池和count为6的CountDownLatch对象，使用线程池处理读取任务，每个线程处理完就将count-1，调用CountDownLatch对象的await方法，直到所有文件都读取完以后，才会接着执行后面的逻辑。

## 30、线程数的设置
- CPU 密集型任务(N+1)： 这种任务消耗的主要是 CPU 资源，可以将线程数设置为 N（CPU 核心数）+1，比 CPU 核心数多出来的一个线程是为了防止线程偶发的缺页中断，或者其它原因导致的任务暂停而带来的影响。一旦任务暂停，CPU 就会处于空闲状态，而在这种情况下多出来的一个线程就可以充分利用 CPU 的空闲时间。
I/O 密集型任务(2N)： 这种任务应用起来，系统会用大部分的时间来处理 I/O 交互，而线程在处理 I/O 的时间段内不会占用 CPU 来处理，这时就可以将 CPU 交出给其它线程使用。因此在 I/O 密集型任务的应用中，我们可以多配置一些线程，具体的计算方法是 2N。














