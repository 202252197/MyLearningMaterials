## JUC概述
### 什么是JUC
在Java中，线程部分是一个中重点，JUC就是java.util.concurrent工具包的简称。这是一个处理线程的工具包，JDK1.5开始出现的。
### 线程和进程概念
#### 进程与线程
**进程（Process）** 是计算机中的程序关于某数据集合上的一次运行活动，是系统进行资源分配和调度的基本单位，是操作系统结构的基础。在当代面向线程设计的计算机结构中，进程是线程的容器。程序是指令、数据及其组织形式的描述，进程是程序的实体。

**线程（Thread）** 是操作系统能够进行运算调度的最小单位。它被包含在进程之中，是进程中的实际运作单位，一条线程指的是进程中一个单一顺序的控制流，一个进程中可以并发多个线程，每条线程并行执行不同的任务。

**总结来说：**
进程：指在系统中正在运行的一个应用程序；程序一旦运行就是进程；进程是资源分配的最小单位。
线程：系统分配处理器时间资源的基本单元，或者说进程之内独立执行的一个单元执行流。线程是程序执行的最小单位。
#### 线程的状态
**线程状态枚举类**
``` JAVA
public enum State {  
 /**  
 * Thread state for a thread which has not yet started. */ 
 NEW,  (新建)
  
 /**  
 * Thread state for a runnable thread.  A thread in the runnable * state is executing in the Java virtual machine but it may * be waiting for other resources from the operating system * such as processor. */ 
 RUNNABLE,  （准备就绪）
  
 /**  
 * Thread state for a thread blocked waiting for a monitor lock. * A thread in the blocked state is waiting for a monitor lock * to enter a synchronized block/method or * reenter a synchronized block/method after calling * {@link Object#wait() Object.wait}.  
 */ 
 BLOCKED,  （阻塞）
  
 /**  
 * Thread state for a waiting thread. * A thread is in the waiting state due to calling one of the * following methods: * <ul> *   <li>{@link Object#wait() Object.wait} with no timeout</li>  
 *   <li>{@link #join() Thread.join} with no timeout</li>  
 *   <li>{@link LockSupport#park() LockSupport.park}</li>  
 * </ul> * * <p>A thread in the waiting state is waiting for another thread to * perform a particular action. * * For example, a thread that has called {@code Object.wait()}  
 * on an object is waiting for another thread to call * {@code Object.notify()} or {@code Object.notifyAll()} on  
 * that object. A thread that has called {@code Thread.join()}  
 * is waiting for a specified thread to terminate. */ 
 WAITING,  （等待）
  
 /**  
 * Thread state for a waiting thread with a specified waiting time. * A thread is in the timed waiting state due to calling one of * the following methods with a specified positive waiting time: * <ul> *   <li>{@link #sleep Thread.sleep}</li>  
 *   <li>{@link Object#wait(long) Object.wait} with timeout</li>  
 *   <li>{@link #join(long) Thread.join} with timeout</li>  
 *   <li>{@link LockSupport#parkNanos LockSupport.parkNanos}</li>  
 *   <li>{@link LockSupport#parkUntil LockSupport.parkUntil}</li>  
 * </ul> */ 
 TIMED_WAITING,  （设置超时时间等待）
  
 /**  
 * Thread state for a terminated thread. * The thread has completed execution. */ 
 TERMINATED;  （终结）
}
```
#### wait/sleep的区别
（1）sleep是Thread的静态方法，wait是Object的方法，任何对象实例都能调用。
（2）sleep不会释放锁，它也不需要占用锁，wait会释放锁，但调用它的前提是当前线程占有锁（即代码要在synchronized中）。
（3）它们都可以被interrupted方法中断。
#### 并发与并行
**串行模式：**
串行表示所有任务都一一按先后顺序进行。串行意味着必须先装完一车柴才能运送这个车柴，只有运送到了，才能卸下这车柴，并且只有完成了这三个步骤，才能进行下一个步骤。
串行是一次只能取得一个任务，并执行这个任务。
**并行模式**
并行意味着可以同时取得多个任务，并同时去执行所取得的这些任务。并行模式相当于将长长的一条队列，划分成了多条短队列，所以并行缩短了队列任务的长度。并行的效率从代码层次上强依赖于多进程/多线程代码。从硬件角度上则依赖于多核CPU。
**并发**
并发（concurrent）指的是多个程序可以同时运行的现象，更细化的是多进程可以同时运行或者多指令可以同时运行。但这不是重点，在描述并发的时候也不会去扣这种字眼是否精确，并发的重点在于它是一种现象，并发描述的是多进程同时运行的现象，但实际上，对于单核心CPU来说，同一时刻只能运行一个线程。所以，这里的“同时运行”表示的不是真的同一时刻有多个线程运行的现象，这是并行的概念，而是提供一种功能让用户看起来多个程序同时运行起来了，但实际上这些程序中的进程不是一直霸占CPU的，而是执行一会停一会。
要解决大并发问题，通常是将大任务分解成多个小任务，由于操作系统对进程的调度是随机的，所以切分成多个小任务后，可能会从任意一个小任务执行。这可能会出现一些现象：
+  可能会出现一个小任务执行了多次，还没开始下个任务的情况。这时一般会采用队列或类似的数据结构来存放各个小任务的成果。
+  可能出现还没准备好第一步就执行第二个的可能。这时，一般采用多路复用或异步的方式，比如只有准备好产生了事件通知才执行某个任务。
+  可以多进程/多线程的方式并行执行这些小任务。也可以单进程/单线程执行这些小任务，这时很可能要配合多路复用才能达到较高的效率。

并发：同一时刻多个线程在访问同一资源，多个线程对一个点。
			例子：春运抢票、电商秒杀...
			
并行：多项工作一起执行，之后再汇总。
			例子：泡方便面，电水壶烧水，一边撒调料倒入桶中

## Lock接口
