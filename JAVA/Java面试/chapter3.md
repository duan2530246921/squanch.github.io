# 多线程
## 35.并行和并发有什么区别？
---
1. 并发（Concurrent）：指两个或多个事件在同一时间间隔内发生，即交替做不同事的能力，多线程是并发的一种形式。例如垃圾回收时，用户线程与垃圾收集线程同时执行（但不一定是并行的，可能会交替执行），用户程序在继续运行，而垃圾收集程序运行于另一个CPU上。

2. 并行（Parallel）：指两个或者多个事件在同一时刻发生，即同时做不同事的能力。例如垃圾回收时，多条垃圾收集线程并行工作，但此时用户线程仍然处于等待状态。

## 36.线程和进程的区别？
---
进程：是执行中一段程序，即一旦程序被载入到内存中并准备执行，它就是一个进程。进程是表示资源分配的的基本概念，又是调度运行的基本单位，是系统中的并发执行的单位。
线程：单个进程中执行中每个任务就是一个线程。线程是进程中执行运算的最小单位。
1. 一个线程只能属于一个进程，但是一个进程可以拥有多个线程。多线程处理就是允许一个进程中在同一时刻执行多个任务。
2. 线程是一种轻量级的进程，与进程相比，线程给操作系统带来侧创建、维护、和管理的负担要轻，意味着线程的代价或开销比较小。
3. 线程没有地址空间，线程包含在进程的地址空间中。线程上下文只包含一个堆栈、一个寄存器、一个优先权，线程文本包含在他的进程 的文本片段中，进程拥有的所有资源都属于线程。所有的线程共享进程的内存和资源。 同一进程中的多个线程共享代码段(代码和常量)，数据段(全局变量和静态变量)，扩展段(堆存储)。但是每个线程拥有自己的栈段， 寄存器的内容，栈段又叫运行时段，用来存放所有局部变量和临时变量。
4. 父和子进程使用进程间通信机制，同一进程的线程通过读取和写入数据到进程变量来通信。
5. 进程内的任何线程都被看做是同位体，且处于相同的级别。不管是哪个线程创建了哪一个线程，进程内的任何线程都可以销毁、挂起、恢复和更改其它线程的优先权。线程也要对进程施加控制，进程中任何线程都可以通过销毁主线程来销毁进程，销毁主线程将导致该进程的销毁，对主线程的修改可能影响所有的线程。
6. 子进程不对任何其他子进程施加控制，进程的线程可以对同一进程的其它线程施加控制。子进程不能对父进程施加控制，进程中所有线程都可以对主线程施加控制。
- 相同点： 进程和线程都有ID/寄存器组、状态和优先权、信息块，创建后都可更改自己的属性，都可与父进程共享资源、都不鞥直接访问其他无关进程或线程的资源。

## 37.守护线程是什么？
---
1. 守护线程，专门用于服务其他的线程，如果其他的线程（即用户自定义线程）都执行完毕，连main线程也执行完毕，那么jvm就会退出（即停止运行）——此时，连jvm都停止运行了，守护线程当然也就停止执行了。
2. 再换一种说法，如果有用户自定义线程存在的话，jvm就不会退出——此时，守护线程也不能退出，也就是它还要运行，干嘛呢，就是为了执行垃圾回收的任务啊。

## 38.创建线程有哪几种方式？
---
实现Runable接口和实现Thread类。
```
package com.summer;

public class ThreadA implements Runnable {

    public void run() {
        System.out.println("start ThreadA!");
    }

    public static void main(String[] args) {
        Thread thread = new Thread(new ThreadA());
        thread.start();
    }
}
```
```
package com.summer;

public class ThreadB extends Thread {

    @Override
    public void run() {
        System.out.println("start ThreadB!");
    }

    public static void main(String[] args) {
        ThreadB threadB = new ThreadB();
        threadB.start();
    }
}
```
可以实现Callable接口和线程池来创建线程。
```
package com.summer;

import java.util.concurrent.Callable;
import java.util.concurrent.FutureTask;

public class ThreadC implements Callable {

    public Object call() throws Exception {
        System.out.println("start ThreadC!");
        return null;
    }

    public static void main(String[] args) {
        ThreadC threadC = new ThreadC();
        FutureTask futureTask = new FutureTask(threadC);
        Thread thread = new Thread(futureTask);
        thread.start();
    }
```
```
package com.summer;

import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadD implements Runnable {

    public void run() {
        System.out.println("start ThreadD!");
    }

    public static void main(String[] args) {
        ExecutorService executorService = Executors.newSingleThreadExecutor();
        executorService.execute(new ThreadD());
    }
}
```

## 39.说一下 runnable 和 callable 有什么区别？
---
相同点
1. 两者都是接口；（废话）
2. 两者都可用来编写多线程程序；
3. 两者都需要调用Thread.start()启动线程；

不同点
1. 两者最大的不同点是：实现Callable接口的任务线程能返回执行结果；而实现Runnable接口的任务线程不能返回结果；
2. Callable接口的call()方法允许抛出异常；而Runnable接口的run()方法的异常只能在内部消化，不能继续上抛；

## 40.线程有哪些状态？
---
线程状态有 5 种，新建，就绪，运行，阻塞，死亡。

## 41.sleep() 和 wait() 有什么区别？
---
sleep() 和 wait() 的区别就是 调用sleep方法的线程不会释放对象锁，而调用wait() 方法会释放对象锁

## 42.notify()和 notifyAll()有什么区别？
---
+ 如果线程调用了对象的 wait()方法，那么线程便会处于该对象的等待池中，等待池中的线程不会去竞争该对象的锁。
+ 当有线程调用了对象的 notifyAll()方法（唤醒所有 wait 线程）或 notify()方法（只随机唤醒一个 wait 线程），被唤醒的的线程便会进入该对象的锁池中，锁池中的线程会去竞争该对象锁。也就是说，调用了notify后只要一个线程会由等待池进入锁池，而notifyAll会将该对象等待池内的所有线程移动到锁池中，等待锁竞争
+ 优先级高的线程竞争到对象锁的概率大，假若某线程没有竞争到该对象锁，它还会留在锁池中，唯有线程再次调用 wait()方法，它才会重新回到等待池中。而竞争到对象锁的线程则继续往下执行，直到执行完了 synchronized 代码块，它会释放掉该对象锁，这时锁池中的线程会继续竞争该对象锁。

## 43.线程的 run()和 start()有什么区别？
---
+ 调用 start() 方法是用来启动线程的，轮到该线程执行时，会自动调用 run()；直接调用 run() 方法，无法达到启动多线程的目的，相当于主线程线性执行 Thread 对象的 run() 方法。
+ 一个线程对线的 start() 方法只能调用一次，多次调用会抛出 java.lang.IllegalThreadStateException 异常；run() 方法没有限制
测试run() 方法

```
public class TestThreadRunStart {
 
	public static void main(String[] args) {
		Thread t = new Thread(){
			@Override
			public void run() {
				//休眠3秒
				try {
					Thread.sleep(3000);
					System.out.println("休眠3秒");
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				System.out.println("Thread running...");
			}
		};
		
		testRun(t);
//		testStart(t);
	}
	
	private static void testRun(Thread t) {
		t.run();
		//休眠1秒
		try {
			Thread.sleep(1000);
			System.out.println("休眠1秒");
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
	
	private static void testStart(Thread t) {
		t.start();
		//休眠1秒
		try {
			Thread.sleep(1000);
			System.out.println("休眠1秒");
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
```

打印结果

```
休眠3秒
Thread running...
休眠1秒
```

测试 start() 方法

```
public static void main(String[] args) {
		Thread t = new Thread(){
			@Override
			public void run() {
				//休眠3秒
				try {
					Thread.sleep(3000);
					System.out.println("休眠3秒");
				} catch (InterruptedException e) {
					e.printStackTrace();
				}
				System.out.println("Thread running...");
			}
		};
		
//		testRun(t);
		testStart(t);
	}
```

打印结果

```
休眠1秒
休眠3秒
Thread running...
```

## 44.创建线程池有哪几种方式？
---
+ newFixedThreadPool
定长线程池，每当提交一个任务就创建一个线程，直到达到线程池的最大数量，这时线程数量不再变化，当线程发生错误结束时，线程池会补充一个新的线程

```
public class TestThreadPool {
 
	//定长线程池，每当提交一个任务就创建一个线程，直到达到线程池的最大数量，这时线程数量不再变化，当线程发生错误结束时，线程池会补充一个新的线程
	static ExecutorService fixedExecutor = Executors.newFixedThreadPool(3);
	
	
	public static void main(String[] args) {
		testFixedExecutor();
	}
	
	//测试定长线程池，线程池的容量为3，提交6个任务，根据打印结果可以看出先执行前3个任务，3个任务结束后再执行后面的任务
	private static void testFixedExecutor() {
		for (int i = 0; i < 6; i++) {
			final int index = i;
			fixedExecutor.execute(new Runnable() {
				public void run() {
					try {
						Thread.sleep(3000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println(Thread.currentThread().getName() + " index:" + index);
				}
			});
		}
		
		try {
			Thread.sleep(4000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("4秒后...");
		
		fixedExecutor.shutdown();
	}
	
}
```

打印结果

```
pool-1-thread-1 index:0
pool-1-thread-2 index:1
pool-1-thread-3 index:2
4秒后...
pool-1-thread-3 index:5
pool-1-thread-1 index:3
pool-1-thread-2 index:4
```

+ newCachedThreadPool
可缓存的线程池，如果线程池的容量超过了任务数，自动回收空闲线程，任务增加时可以自动添加新线程，线程池的容量不限制

```
public class TestThreadPool {
 
	//可缓存的线程池，如果线程池的容量超过了任务数，自动回收空闲线程，任务增加时可以自动添加新线程，线程池的容量不限制
	static ExecutorService cachedExecutor = Executors.newCachedThreadPool();
	
	
	public static void main(String[] args) {
		testCachedExecutor();
	}
	
	//测试可缓存线程池
	private static void testCachedExecutor() {
		for (int i = 0; i < 6; i++) {
			final int index = i;
			cachedExecutor.execute(new Runnable() {
				public void run() {
					try {
						Thread.sleep(3000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println(Thread.currentThread().getName() + " index:" + index);
				}
			});
		}
		
		try {
			Thread.sleep(4000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("4秒后...");
		
		cachedExecutor.shutdown();
	}
	
}
```
打印结果

```
pool-1-thread-1 index:0
pool-1-thread-6 index:5
pool-1-thread-5 index:4
pool-1-thread-4 index:3
pool-1-thread-3 index:2
pool-1-thread-2 index:1
4秒后...
```

+ newScheduledThreadPool
定长线程池，可执行周期性的任务

```
public class TestThreadPool {
 
	//定长线程池，可执行周期性的任务
	static ScheduledExecutorService scheduledExecutor = Executors.newScheduledThreadPool(3);
	
	
	public static void main(String[] args) {
		testScheduledExecutor();
	}
	
	//测试定长、可周期执行的线程池
	private static void testScheduledExecutor() {
		for (int i = 0; i < 3; i++) {
			final int index = i;
			//scheduleWithFixedDelay 固定的延迟时间执行任务； scheduleAtFixedRate 固定的频率执行任务
			scheduledExecutor.scheduleWithFixedDelay(new Runnable() {
				public void run() {
					System.out.println(Thread.currentThread().getName() + " index:" + index);
				}
			}, 0, 3, TimeUnit.SECONDS);
		}
		
		try {
			Thread.sleep(4000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("4秒后...");
		
		scheduledExecutor.shutdown();
	}
	
}
```

打印结果

```
pool-1-thread-1 index:0
pool-1-thread-2 index:1
pool-1-thread-3 index:2
pool-1-thread-1 index:0
pool-1-thread-3 index:1
pool-1-thread-1 index:2
4秒后...
```

+ newSingleThreadExecutor
单线程的线程池，线程异常结束，会创建一个新的线程，能确保任务按提交顺序执行

```
public class TestThreadPool {
	
	//单线程的线程池，线程异常结束，会创建一个新的线程，能确保任务按提交顺序执行
	static ExecutorService singleExecutor = Executors.newSingleThreadExecutor();
	
	
	public static void main(String[] args) {
		testSingleExecutor();
	}
	
	//测试单线程的线程池
	private static void testSingleExecutor() {
		for (int i = 0; i < 3; i++) {
			final int index = i;
			singleExecutor.execute(new Runnable() {
				public void run() {
					try {
						Thread.sleep(3000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println(Thread.currentThread().getName() + " index:" + index);
				}
			});
		}
		
		try {
			Thread.sleep(4000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("4秒后...");
		
		singleExecutor.shutdown();
	}
	
}
```

打印结果

```
pool-1-thread-1 index:0
4秒后...
pool-1-thread-1 index:1
pool-1-thread-1 index:2
```

+ newSingleThreadScheduledExecutor
单线程可执行周期性任务的线程池

```
public class TestThreadPool {
	
	//单线程可执行周期性任务的线程池
	static ScheduledExecutorService singleScheduledExecutor = Executors.newSingleThreadScheduledExecutor();
	
	
	public static void main(String[] args) {
		testSingleScheduledExecutor();
	}
	
	//测试单线程可周期执行的线程池
	private static void testSingleScheduledExecutor() {
		for (int i = 0; i < 3; i++) {
			final int index = i;
			//scheduleWithFixedDelay 固定的延迟时间执行任务； scheduleAtFixedRate 固定的频率执行任务
			singleScheduledExecutor.scheduleAtFixedRate(new Runnable() {
				public void run() {
					System.out.println(Thread.currentThread().getName() + " index:" + index);
				}
			}, 0, 3, TimeUnit.SECONDS);
		}
		
		try {
			Thread.sleep(4000);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("4秒后...");
		
		singleScheduledExecutor.shutdown();
	}
	
}
```

打印结果

```
pool-1-thread-1 index:0
pool-1-thread-1 index:1
pool-1-thread-1 index:2
pool-1-thread-1 index:0
pool-1-thread-1 index:1
pool-1-thread-1 index:2
4秒后...
```

+ newWorkStealingPool
任务窃取线程池，不保证执行顺序，适合任务耗时差异较大。
线程池中有多个线程队列，有的线程队列中有大量的比较耗时的任务堆积，而有的线程队列却是空的，就存在有的线程处于饥饿状态，当一个线程处于饥饿状态时，它就会去其它的线程队列中窃取任务。解决饥饿导致的效率问题。
默认创建的并行 level 是 CPU 的核数。主线程结束，即使线程池有任务也会立即停止

```
public class TestThreadPool {
 
	//任务窃取线程池
	static ExecutorService workStealingExecutor = Executors.newWorkStealingPool();
	
	public static void main(String[] args) {
		testWorkStealingExecutor();
	}
	
	//测试任务窃取线程池
	private static void testWorkStealingExecutor() {
		for (int i = 0; i < 10; i++) {//本机 CPU 8核，这里创建10个任务进行测试
			final int index = i;
			workStealingExecutor.execute(new Runnable() {
				public void run() {
					try {
						Thread.sleep(3000);
					} catch (InterruptedException e) {
						e.printStackTrace();
					}
					System.out.println(Thread.currentThread().getName() + " index:" + index);
				}
			});
		}
		
		try {
			Thread.sleep(4000);//这里主线程不休眠，不会有打印输出
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
		System.out.println("4秒后...");
		
//		workStealingExecutor.shutdown();
	}
	
}
```

打印结果

```
ForkJoinPool-1-worker-1 index:0
ForkJoinPool-1-worker-7 index:6
ForkJoinPool-1-worker-5 index:4
ForkJoinPool-1-worker-3 index:2
ForkJoinPool-1-worker-4 index:3
ForkJoinPool-1-worker-2 index:1
ForkJoinPool-1-worker-0 index:7
ForkJoinPool-1-worker-6 index:5
4秒后...    
```

## 45.线程池都有哪些状态？
---
线程池的5种状态：RUNNING、SHUTDOWN、STOP、TIDYING、TERMINATED。
1. RUNNING：线程池一旦被创建，就处于 RUNNING 状态，任务数为 0，能够接收新任务，对已排队的任务进行处理。
2. SHUTDOWN：不接收新任务，但能处理已排队的任务。调用线程池的 shutdown() 方法，线程池由 RUNNING 转变为 SHUTDOWN 状态。
3. STOP：不接收新任务，不处理已排队的任务，并且会中断正在处理的任务。调用线程池的 shutdownNow() 方法，线程池由(RUNNING 或 SHUTDOWN ) 转变为 STOP 状态。
4. TIDYING：
SHUTDOWN 状态下，任务数为 0， 其他所有任务已终止，线程池会变为 TIDYING 状态，会执行 terminated() 方法。线程池中的 terminated() 方法是空实现，可以重写该方法进行相应的处理。
线程池在 SHUTDOWN 状态，任务队列为空且执行中任务为空，线程池就会由 SHUTDOWN 转变为 TIDYING 状态。
线程池在 STOP 状态，线程池中执行中任务为空时，就会由 STOP 转变为 TIDYING 状态。
5. TERMINATED：线程池彻底终止。线程池在 TIDYING 状态执行完 terminated() 方法就会由 TIDYING 转变为 TERMINATED 状态。

## 46.线程池中 submit()和 execute()方法有什么区别？
---
+ execute() 参数 Runnable ；submit() 参数 (Runnable) 或 (Runnable 和 结果 T) 或 (Callable)
+ execute() 没有返回值；而 submit() 有返回值
+ submit() 的返回值 Future 调用get方法时，可以捕获处理异常

## 47.在 java 程序中怎么保证多线程的运行安全？
---
线程的安全性问题体现在：

原子性：一个或者多个操作在 CPU 执行的过程中不被中断的特性
可见性：一个线程对共享变量的修改，另外一个线程能够立刻看到
有序性：程序执行的顺序按照代码的先后顺序执行

导致原因：

缓存导致的可见性问题
线程切换带来的原子性问题
编译优化带来的有序性问题

解决办法：

JDK Atomic开头的原子类、synchronized、LOCK，可以解决原子性问题
synchronized、volatile、LOCK，可以解决可见性问题
Happens-Before 规则可以解决有序性问题
 
Happens-Before 规则如下：

程序次序规则：在一个线程内，按照程序控制流顺序，书写在前面的操作先行发生于书写在后面的操作
管程锁定规则：一个unlock操作先行发生于后面对同一个锁的lock操作
volatile变量规则：对一个volatile变量的写操作先行发生于后面对这个变量的读操作
线程启动规则：Thread对象的start()方法先行发生于此线程的每一个动作
线程终止规则：线程中的所有操作都先行发生于对此线程的终止检测
线程中断规则：对线程interrupt()方法的调用先行发生于被中断线程的代码检测到中断事件的发生
对象终结规则：一个对象的初始化完成(构造函数执行结束)先行发生于它的finalize()方法的开始

## 48.多线程锁的升级原理是什么？
---
锁的级别从低到高：

无锁 -> 偏向锁 -> 轻量级锁 -> 重量级锁

锁分级别原因：

没有优化以前，sychronized是重量级锁（悲观锁），使用 wait 和 notify、notifyAll 来切换线程状态非常消耗系统资源；线程的挂起和唤醒间隔很短暂，这样很浪费资源，影响性能。所以 JVM 对 sychronized 关键字进行了优化，把锁分为 无锁、偏向锁、轻量级锁、重量级锁 状态。

## 49.什么是死锁？
---
所谓死锁，是指多个进程在运行过程中因争夺资源而造成的一种僵局，当进程处于这种僵持状态时，若无外力作用，它们都将无法再向前推进。 因此我们举个例子来描述，如果此时有一个线程A，按照先锁a再获得锁b的的顺序获得锁，而在此同时又有另外一个线程B，按照先锁b再锁a的顺序获得锁

## 50.怎么防止死锁？
---
+ 资源一次性分配：一次性分配所有资源，这样就不会再有请求了：（破坏请求条件）
+ 只要有一个资源得不到分配，也不给这个进程分配其他的资源：（破坏请保持条件）
+ 可剥夺资源：即当某进程获得了部分资源，但得不到其它资源，则释放已占有的资源（破坏不可剥夺条件）
+ 资源有序分配法：系统给每类资源赋予一个编号，每一个进程按编号递增的顺序请求资源，释放则相反（破坏环路等待条件）
 
## 51.ThreadLocal 是什么？有哪些使用场景？
---
ThreadLocal 是线程本地存储，在每个线程中都创建了一个 ThreadLocalMap 对象，每个线程可以访问自己内部 ThreadLocalMap 对象内的 value。通过这种方式，避免资源在多线程间共享。
经典的使用场景是为每个线程分配一个 JDBC 连接 Connection。这样就可以保证每个线程的都在各自的 Connection 上进行数据库的操作，不会出现 A 线程关了 B线程正在使用的 Connection； 还有 Session 管理 等问题。
```
public class TestThreadLocal {
    
    //线程本地存储变量
    private static final ThreadLocal<Integer> THREAD_LOCAL_NUM = new ThreadLocal<Integer>() {
        @Override
        protected Integer initialValue() {
            return 0;
        }
    };
 
    public static void main(String[] args) {
        for (int i = 0; i <3; i++) {//启动三个线程
            Thread t = new Thread() {
                @Override
                public void run() {
                    add10ByThreadLocal();
                }
            };
            t.start();
        }
    }
    
    /**
     * 线程本地存储变量加 5
     */
    private static void add10ByThreadLocal() {
        for (int i = 0; i <5; i++) {
            Integer n = THREAD_LOCAL_NUM.get();
            n += 1;
            THREAD_LOCAL_NUM.set(n);
            System.out.println(Thread.currentThread().getName() + " : ThreadLocal num=" + n);
        }
    }
    
}
```
打印结果:启动了 3 个线程，每个线程最后都打印到 "ThreadLocal num=5"，而不是 num 一直在累加直到值等于 15
```
Thread-0 : ThreadLocal num=1
Thread-1 : ThreadLocal num=1
Thread-0 : ThreadLocal num=2
Thread-0 : ThreadLocal num=3
Thread-1 : ThreadLocal num=2
Thread-2 : ThreadLocal num=1
Thread-0 : ThreadLocal num=4
Thread-2 : ThreadLocal num=2
Thread-1 : ThreadLocal num=3
Thread-1 : ThreadLocal num=4
Thread-2 : ThreadLocal num=3
Thread-0 : ThreadLocal num=5
Thread-2 : ThreadLocal num=4
Thread-2 : ThreadLocal num=5
Thread-1 : ThreadLocal num=5
```

## 52.说一下 synchronized 底层实现原理？
---
Synchronized是Java中解决并发问题的一种最常用的方法，也是最简单的一种方法。Synchronized的作用主要有三个：（1）确保线程互斥的访问同步代码（2）保证共享变量的修改能够及时可见（3）有效解决重排序问题。从语法上讲，Synchronized总共有三种用法：
1. 修饰普通方法
2. 修饰静态方法
3. 修饰代码块

## 53.synchronized 和 volatile 的区别是什么？
---
+ volatile本质是在告诉jvm当前变量在寄存器（工作内存）中的值是不确定的，需要从主存中读取； synchronized则是锁定当前变量，只有当前线程可以访问该变量，其他线程被阻塞住。
+ volatile仅能使用在变量级别；synchronized则可以使用在变量、方法、和类级别的
+ volatile仅能实现变量的修改可见性，不能保证原子性；而synchronized则可以保证变量的修改可见性和原子性
+ volatile不会造成线程的阻塞；synchronized可能会造成线程的阻塞。
+ volatile标记的变量不会被编译器优化；synchronized标记的变量可以被编译器优化

## 54.synchronized 和 Lock 有什么区别？
---
1. 原始构成：

synchronized是关键字，属于JVM层面，底层是由一对monitorenter和monitorexit指令实现的。

ReentrantLock是一个具体类，是API层面的锁。

2. 使用方法：

synchronized不需要用户手动释放锁，当synchronized代码块执行完成后，系统会自动让线程释放对锁的占用

ReentrantLock需要用户手动释放锁，若没有手动释放可能导致死锁现象。

3. 等待是否可中断：

synchronized不可中断，除非抛出异常或者正常运行完成

ReentrantLock可中断

4. 加锁是否公平：

synchronized非公平锁

ReentrantLock两者都可以，默认是非公平锁。

5. 锁绑定多个条件Condition：

synchronized没有。

ReentrantLock可用来分组唤醒需要唤醒的线程。而不是像synchronized要么随机唤醒一个线程，要么唤醒所有线程。

题目：多线程之间按顺序调用，实现A->B->C三个线程启动，要求如下：

* AA打印5次，BB打印10次，CC打印15次
* 紧接着
* AA打印5次，BB打印10次，CC打印15次
* 来10轮

```
class ShareResource {
    private int number = 1;
    private Lock lock = new ReentrantLock();
    private Condition condition1 = lock.newCondition();
    private Condition condition2 = lock.newCondition();
    private Condition condition3 = lock.newCondition();
 
    public void print5() {
        lock.lock();
        try {
            while (number != 1) {
                condition1.await();
            }
            for (int i = 1; i <= 5; i++) {
                System.out.println(Thread.currentThread().getName() + " " + i);
            }
            number = 2;
            condition2.signal();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
 
 
    public void print10() {
        lock.lock();
        try {
            while (number != 2) {
                condition2.await();
            }
            for (int i = 1; i <= 10; i++) {
                System.out.println(Thread.currentThread().getName() + " " + i);
            }
            number = 3;
            condition3.signal();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
 
 
    public void print15() {
        lock.lock();
        try {
            while (number != 3) {
                condition3.await();
            }
            for (int i = 1; i <= 15; i++) {
                System.out.println(Thread.currentThread().getName() + " " + i);
            }
            number = 1;
            condition1.signal();
        } catch (InterruptedException e) {
            e.printStackTrace();
        } finally {
            lock.unlock();
        }
    }
}
 
public class SyncAndReentrantLockDemo {
 
    public static void main(String[] args) {
        ShareResource shareResource = new ShareResource();
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                shareResource.print5();
            }
        }, "AAA").start();
 
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                shareResource.print10();
            }
        }, "BBB").start();
 
        new Thread(() -> {
            for (int i = 0; i < 10; i++) {
                shareResource.print15();
            }
        }, "CCC").start();
    }
}
```

## 55.synchronized 和 ReentrantLock 区别是什么？
---
+ synchronized 竞争锁时会一直等待；ReentrantLock 可以尝试获取锁，并得到获取结果
+ synchronized 获取锁无法设置超时；ReentrantLock 可以设置获取锁的超时时间
+ synchronized 无法实现公平锁；ReentrantLock 可以满足公平锁，即先等待先获取到锁
+ synchronized 控制等待和唤醒需要结合加锁对象的 wait() 和 notify()、notifyAll()；ReentrantLock 控制等待和唤醒需要结合 Condition 的 await() 和 signal()、signalAll() 方法
+ synchronized 是 JVM 层面实现的；ReentrantLock 是 JDK 代码层面实现
+ synchronized 在加锁代码块执行完或者出现异常，自动释放锁；ReentrantLock 不会自动释放锁，需要在 finally{} 代码块显示释放

## 56.说一下 atomic 的原理？
---
在多线程的场景中，我们需要保证数据安全，就会考虑同步的方案，通常会使用synchronized或者lock来处理，使用了synchronized意味着内核态的一次切换。这是一个很重的操作。
有没有一种方式，可以比较便利的实现一些简单的数据同步，比如计数器等等。concurrent包下的atomic提供我们这么一种轻量级的数据同步的选择。

```
class MyThread implements Runnable {
 
    static AtomicInteger ai=new AtomicInteger(0);
 
    public void run() {
        for (int m = 0; m < 1000000; m++) {
            ai.getAndIncrement();
        }
    }
};
 
public class TestAtomicInteger {
    public static void main(String[] args) throws InterruptedException {
        MyThread mt = new MyThread();
 
        Thread t1 = new Thread(mt);
        Thread t2 = new Thread(mt);
        t1.start();
        t2.start();
        Thread.sleep(500);
        System.out.println(MyThread.ai.get());
    }
}
```

在以上代码中，使用AtomicInteger声明了一个全局变量，并且在多线程中进行自增，代码中并没有进行显示的加锁。以上代码的输出结果，永远都是2000000。如果将AtomicInteger换成Integer，打印结果基本都是小于2000000。 也就说明AtomicInteger声明的变量，在多线程场景中的自增操作是可以保证线程安全的。接下来我们分析下其原理。
原理 这里，我们来看看AtomicInteger是如何使用非阻塞算法来实现并发控制的。 AtomicInteger的关键域只有一下3个：

```
public class AtomicInteger extends Number implements java.io.Serializable {
    private static final long serialVersionUID = 6214790243416807050L;
 
    // setup to use Unsafe.compareAndSwapInt for updates
    private static final Unsafe unsafe = Unsafe.getUnsafe();
    private static final long valueOffset;
 
    static {
        try {
            valueOffset = unsafe.objectFieldOffset
                (AtomicInteger.class.getDeclaredField("value"));
        } catch (Exception ex) { throw new Error(ex); }
    }
 
    private volatile int value;
 
    /**
     * Creates a new AtomicInteger with the given initial value.
     *
     * @param initialValue the initial value
     */
    public AtomicInteger(int initialValue) {
        value = initialValue;
    }
 
    /**
     * Creates a new AtomicInteger with initial value {@code 0}.
     */
    public AtomicInteger() {
    }
 
    ......
}
```

这里， unsafe是java提供的获得对对象内存地址访问的类，注释已经清楚的写出了，它的作用就是在更新操作时提供“比较并替换”的作用。实际上就是AtomicInteger中的一个工具。
valueOffset是用来记录value本身在内存的编译地址的，这个记录，也主要是为了在更新操作在内存中找到value的位置，方便比较。
value是用来存储整数的时间变量，这里被声明为volatile。volatile只能保证这个变量的可见性。不能保证他的原子性。
可以看看getAndIncrement这个类似i++的函数，可以发现，是调用了UnSafe中的getAndAddInt.

```
    /**
     * Atomically increments by one the current value.
     *
     * @return the previous value
     */
    public final int getAndIncrement() {
        return unsafe.getAndAddInt(this, valueOffset, 1);
    }
 
    /**
     * Atomically sets to the given value and returns the old value.
     *
     * @param newValue the new value
     * @return the previous value
     */
    public final int getAndSet(int newValue) {
        return unsafe.getAndSetInt(this, valueOffset, newValue);
    }
 
    public final int getAndSetInt(Object var1, long var2, int var4) {
        int var5;
        do {
            var5 = this.getIntVolatile(var1, var2);
            //使用unsafe的native方法，实现高效的硬件级别CAS
        } while(!this.compareAndSwapInt(var1, var2, var5, var4));
 
        return var5;
    }
```

如何保证原子性：自旋 + CAS（乐观锁）。在这个过程中，通过compareAndSwapInt比较更新value值，如果更新失败，重新获取旧值，然后更新。

优缺点
    CAS相对于其他锁，不会进行内核态操作，有着一些性能的提升。但同时引入自旋，当锁竞争较大的时候，自旋次数会增多。cpu资源会消耗很高。
    换句话说，CAS+自旋适合使用在低并发有同步数据的应用场景。

Java 8做出的改进和努力
    在Java 8中引入了4个新的计数器类型，LongAdder、LongAccumulator、DoubleAdder、DoubleAccumulator。他们都是继承于Striped64。
在LongAdder 与AtomicLong有什么区别？
    Atomic*遇到的问题是，只能运用于低并发场景。因此LongAddr在这基础上引入了分段锁的概念。可以参考《JDK8系列之LongAdder解析》一起看看做了什么。
    大概就是当竞争不激烈的时候，所有线程都是通过CAS对同一个变量（Base）进行修改，当竞争激烈的时候，会将根据当前线程哈希到对于Cell上进行修改（多段锁）。

```
/**
     * Table of cells. When non-null, size is a power of 2.
     */
    transient volatile Cell[] cells;
 
    /**
     * Base value, used mainly when there is no contention, but also as
     * a fallback during table initialization races. Updated via CAS.
     */
    transient volatile long base;
```
```
/**
     * Padded variant of AtomicLong supporting only raw accesses plus CAS.
     *
     * JVM intrinsics note: It would be possible to use a release-only
     * form of CAS here, if it were provided.
     */
    @sun.misc.Contended static final class Cell {
        volatile long value;
        Cell(long x) { value = x; }
        final boolean cas(long cmp, long val) {
            return UNSAFE.compareAndSwapLong(this, valueOffset, cmp, val);
        }
 
        // Unsafe mechanics
        private static final sun.misc.Unsafe UNSAFE;
        private static final long valueOffset;
        static {
            try {
                UNSAFE = sun.misc.Unsafe.getUnsafe();
                Class<?> ak = Cell.class;
                valueOffset = UNSAFE.objectFieldOffset
                    (ak.getDeclaredField("value"));
            } catch (Exception e) {
                throw new Error(e);
            }
        }
    }
```

可以看到大概实现原理是：通过CAS乐观锁保证原子性，通过自旋保证当次修改的最终修改成功，通过降低锁粒度（多段锁）增加并发性能。