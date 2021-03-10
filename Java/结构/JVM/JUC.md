## 6 集合类不安全

### List

```java
public class ListTest {
    public static void main(String[] args) {

        // List<String> list = new ArrayList<>();
        // List<String> list = new CopyOnWriteArrayList<>();

        List<String> list = new CopyOnWriteArrayList<>();

        for (int i = 0; i <= 99; i++) {
            new Thread(()->{
                list.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(list);
            },String.valueOf(i)).start();
        }
    }
}
```



### Set

```java
public class SetTest {

    public static void main(String[] args) {
//        Set<String> set = new HashSet<>();
//        Set<String> set = Collections.synchronizedSet(new HashSet<>());
        Set<String> set = new CopyOnWriteArraySet();
        for (int i = 0; i < 30; i++) {
            new Thread(()->{
                set.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(set);
            },String.valueOf(i)).start();
        }
    }
}
```



```java
public class HashSet<E>
    extends AbstractSet<E>
    implements Set<E>, Cloneable, java.io.Serializable
{
    static final long serialVersionUID = -5024744406713321676L;

    private transient HashMap<E,Object> map;

    // Dummy value to associate with an Object in the backing Map
    private static final Object PRESENT = new Object();

    public HashSet() {
        map = new HashMap<>();
    }

    public int size() {
        return map.size();
    }

    public boolean isEmpty() {
        return map.isEmpty();
    }

    public boolean contains(Object o) {
        return map.containsKey(o);
    }

    public boolean add(E e) {
        return map.put(e, PRESENT)==null;
    }

    public boolean remove(Object o) {
        return map.remove(o)==PRESENT;
    }

    public void clear() {
        map.clear();
    }

    @SuppressWarnings("unchecked")
    public Object clone() {
        try {
            HashSet<E> newSet = (HashSet<E>) super.clone();
            newSet.map = (HashMap<E, Object>) map.clone();
            return newSet;
        } catch (CloneNotSupportedException e) {
            throw new InternalError(e);
        }
    }
}

```

### Map

```java
public class SetTest {

    public static void main(String[] args) {
//        Set<String> set = new HashSet<>();
//        Set<String> set = Collections.synchronizedSet(new HashSet<>());
        Set<String> set = new CopyOnWriteArraySet();
        for (int i = 0; i < 30; i++) {
            new Thread(()->{
                set.add(UUID.randomUUID().toString().substring(0,5));
                System.out.println(set);
            },String.valueOf(i)).start();
        }
    }
}
```



## 7 Callable

```java
public class CallableTest {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
//        new Thread(new MyThread()).start();
//        new Thread(new FutureTask<V>()).start();
        MyThread thread = new MyThread();
        FutureTask futureTask = new FutureTask(thread);//适配类
        
        new Thread(futureTask,"A").start();
        new Thread(futureTask,"B").start(); // 结果会被缓存提高效率
        
        Integer o = (Integer) futureTask.get(); //获取Callable的返回结果
        System.out.println(o);
    }
}

class MyThread implements Callable<Integer> {

    @Override
    public Integer call() throws Exception {
        System.out.println("call()");
        return 1024;
    }
}
```

细节：

1. 有缓存
2. 结果可能有延迟



## 8 常用的辅助类

### CountDownLatch

减法器

````java
public class CountDownLatchDemo {
    public static void main(String[] args) throws InterruptedException {
        CountDownLatch countDownLatch = new CountDownLatch(6);

        for (int i = 1; i <= 6; i++) {
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+" Go out");
                countDownLatch.countDown();// 数量-1
            },String.valueOf(i)).start();
        }

        countDownLatch.await();// 等待计数器归0
        System.out.println("Close Door" );
    }
}
````

`countDownLatch.countDown();`数量-1

### CyclicBarrier

加法器

````java
public class CyclicBarrierDemo {

    public static void main(String[] args) {
        CyclicBarrier cyclicBarrier = new CyclicBarrier(7,()->{
            // 数量达到后才能启动
            System.out.println("召唤神龙成功！ ");
        });
        for (int i = 1; i <= 7; i++) {
            final int temp = i;
            // lambda不能操作i
            new Thread(()->{
                System.out.println(Thread.currentThread().getName()+"收集龙珠"+temp+"个龙珠");
                try {
                    cyclicBarrier.await(); //等待
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } catch (BrokenBarrierException e) {
                    e.printStackTrace();
                }
            }).start();
        }
    }
}
````

### Semphore

限流

```java
public class SemphoreDemo {
    public static void main(String[] args) {
        Semaphore semaphore = new Semaphore(3);

        for (int i = 1; i <= 6; i++) {
            new Thread(()->{
                // acquire() 得到
                try {
                    semaphore.acquire();
                    System.out.println(Thread.currentThread().getName()+"抢到车位");
                    TimeUnit.SECONDS.sleep(2);
                    System.out.println(Thread.currentThread().getName()+"离开车位");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                } finally {
                    semaphore.release();
                }
                // release() 释放
            },String.valueOf(i)).start();
        }
    }
}

```

原理：

`semphore.acquire();`  获得,假设如果已经满了,等待被释放为止

`Semphore.release();`

作用:多个共享资源互斥使用

## 9 读写锁



## 10 堵塞队列



### 四组API

| 方式           | 抛出异常 | 有返回值 | 堵塞 等待 | 超时等候  |
| -------------- | -------- | -------- | --------- | --------- |
| 添加           | add      | offer    | pull      | offer(,,) |
| 移除           | remove   | poll     | take      | poll(,,)  |
| 判断队列首元素 | element  | peek     |           |           |

```java
public class Test {
    public static void main(String[] args) throws InterruptedException {
        test4();
    }

    /**
     *  抛出异常
     */
    public static void test1() {
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);

        System.out.println(blockingQueue.add("a"));
        System.out.println(blockingQueue.add("b"));
        System.out.println(blockingQueue.add("c"));

        //Exception in thread "main" java.lang.IllegalStateException: Queue full
//        System.out.println(blockingQueue.add("d"));
        System.out.println("======");
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
        System.out.println(blockingQueue.remove());
    }

    /**
     * 返回boolean
     */
    public static void test2(){
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue(3);

        System.out.println(blockingQueue.offer("a"));
        System.out.println(blockingQueue.offer("b"));
        System.out.println(blockingQueue.offer("c"));
        System.out.println(blockingQueue.offer("d")); // false 不抛出异常

        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());

    }

    /**
     * 堵塞队列
     * @throws InterruptedException
     */
    public static void test3() throws InterruptedException {
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        blockingQueue.put("a");
        blockingQueue.put("b");
        blockingQueue.put("c");
//        blockingQueue.put("d"); // 堵塞一直等待
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
        System.out.println(blockingQueue.take());
//        System.out.println(blockingQueue.take());// 堵塞
    }

    /**
     * 超时异常
     * @throws InterruptedException
     */
    public  static void test4() throws InterruptedException {
        ArrayBlockingQueue blockingQueue = new ArrayBlockingQueue<>(3);
        blockingQueue.offer("a");
        blockingQueue.offer("b");
        blockingQueue.offer("c");
        blockingQueue.offer("d",2,TimeUnit.SECONDS);// 2秒 超时退出

        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll());
        System.out.println(blockingQueue.poll(2,TimeUnit.SECONDS));// 2秒 超时退出
    }
}
```



###  同步队列

```java
/**
 * 同步队列
 * SynchronousQueue 不存储元素
 * put了一个元素,必须先take才能 put
 */
public class SynchronousQueueDemo {
    public static void main(String[] args) {
        BlockingQueue<String> blockingQueue = new SynchronousQueue<>();//同步队列

         new Thread(()->{

             try {
                 System.out.println(Thread.currentThread().getName()+"put 1");
                 blockingQueue.put("1");
                 System.out.println(Thread.currentThread().getName()+"put ok");
                 System.out.println(Thread.currentThread().getName()+"put 2");
                 blockingQueue.put("2");
                 System.out.println(Thread.currentThread().getName()+"put ok");
                 System.out.println(Thread.currentThread().getName()+"put 3");
                 blockingQueue.put("3");
                 System.out.println(Thread.currentThread().getName()+"put ok");
             } catch (InterruptedException e) {
                 e.printStackTrace();
             }

         },"T1").start();
        new Thread(()->{

            try {
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"+>"+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"+>"+blockingQueue.take());
                TimeUnit.SECONDS.sleep(3);
                System.out.println(Thread.currentThread().getName()+"+>"+blockingQueue.take());
//                System.out.println(Thread.currentThread().getName()+"+>"+blockingQueue.take());
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

        },"T2").start();
    }
}
```



## 11 线程池

> 池化技术

程序的运行,本质:占用系统的资源!优化资源的使用

池化技术: 事先准备好一些资源,有人要用,就来我这里拿,用完之后还给我



**线程池的好处**

1. 降低资源的消耗
2. 提高响应技术
3. 方便管理

==**线程复用,可以控制最大并发数,管理线程**==

### 三大方法

```java
public class Demo01 {
    public static void main(String[] args) {
//        ExecutorService threadPool = Executors.newSingleThreadExecutor();// 单个线程
        ExecutorService threadPool = Executors.newFixedThreadPool(5); // 创建一个固定线程池的大小
//        ExecutorService threadPool = Executors.newCachedThreadPool();// 伸缩的,遇强则强

        try {
            for (int i = 0; i < 100; i++) {
                threadPool.execute(()->{
                    System.out.println(Thread.currentThread().getName()+" ok");
                });
            }
        }catch (Exception e) {
            e.printStackTrace();
        }finally {
            //
            threadPool.shutdown();
        }

    }
}
```

### 七大参数

```java
    public ThreadPoolExecutor(int corePoolSize, // 核心线程池大小
                              int maximumPoolSize, // 最大线程池大小
                              long keepAliveTime, // 超时了没有人调用就会释放
                              TimeUnit unit, // 超时单位
                              BlockingQueue<Runnable> workQueue) { // 堵塞队列
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue,
             Executors.defaultThreadFactory(), defaultHandler);
    }
    public ThreadPoolExecutor(int corePoolSize, // 核心线程池大小
                              int maximumPoolSize, // 最大线程池大小
                              long keepAliveTime, // 超时了没有人调用就会释放
                              TimeUnit unit, // 超时单位
                              BlockingQueue<Runnable> workQueue, // 堵塞队列
                              ThreadFactory threadFactory, // 线程工厂,创建线程的,一般不动
                              RejectedExecutionHandler handler) {//拒绝策略
        if (corePoolSize < 0 ||
            maximumPoolSize <= 0 ||
            maximumPoolSize < corePoolSize ||
            keepAliveTime < 0)
            throw new IllegalArgumentException();
        if (workQueue == null || threadFactory == null || handler == null)
            throw new NullPointerException();
        this.acc = System.getSecurityManager() == null ?
                null :
                AccessController.getContext();
        this.corePoolSize = corePoolSize;
        this.maximumPoolSize = maximumPoolSize;
        this.workQueue = workQueue;
        this.keepAliveTime = unit.toNanos(keepAliveTime);
        this.threadFactory = threadFactory;
        this.handler = handler;
    }
```



> 4种拒绝策略

```java
        // ThreadPoolExecutor.CallerRunsPolicy//哪来的回哪
        //ThreadPoolExecutor.AbortPolicy()); // 银行满了,还有人进来,不处理这个人的,抛出异常
//        new ThreadPoolExecutor.DiscardPolicy()); // 队列满了丢了
//        ThreadPoolExecutor.DiscardOldestPolicy()); // 队列满了,尝试去和最早的竞争,也不会抛出异常
```

### 最大线程如何定义

- CPU 密集型, 几核就是几可以保持cpu的效率最高
- IO 密集型判断你程序中十分耗IO的线程

## 12四大函数式接口

lambda表达式,链式编程,函数式接口,stream流式计算

> 函数式接口:只有一个方法的接口

>  function 一个参数 一个返回

```java
    /**
 * function 一个参数 一个返回
 * 只要是函数型的接口 可以 用lambda表达式简化
 */
public class Demo01 {

    public static void main(String[] args) {
//        Function<T,R> // 传入参数T返回R
/*        Function function = new Function<String, String>(){

            @Override
            public String apply(String s) {
                return s;
            }
        };*/

        Function function = (str)->{return str;};
        System.out.println(function.apply("asd"));

    }
}
```

> 断定型接口:有一个输入参数,返回之只能为boolean

```java
public class Demo02 {
    public static void main(String[] args) {
        // 判断字符串是否为空
/*        Predicate predicate = new Predicate<String>(){

            @Override
            public boolean test(String s) {
                return s.isEmpty();
            }
        };*/

        Predicate<String> predicate = (str)->{return str.isEmpty();};
        System.out.println(predicate.test(""));
    }
}
```

> Consumer: 消费型接口:只有输入没有返回值

```
public class Demo03 {
    public static void main(String[] args) {
        /*Consumer<String> consumer = new Consumer<String>() {

            @Override
            public void accept(String s) {
                System.out.println("s");
            }
        };*/

        Consumer<String> consumer = (str)->{
            System.out.println(str);
        };
        consumer.accept("yes");
    }
}
```

> 供给型接口:没有参数,只有返回值

```java
public class Demo04 {
    public static void main(String[] args) {
        /*Supplier supplier = new Supplier<Integer>() {
            @Override
            public Integer get() {
                return 1024;
            }
        };*/

        Supplier supplier = ()->{return 1024;};
        System.out.println(supplier.get());
    }
}
```

## 13 Stream流式计算

```java
public class Test {
    public static void main(String[] args) {

        User u1 = new User(1,"a",21);
        User u2 = new User(2,"b",22);
        User u3 = new User(3,"c",23);
        User u4 = new User(4,"d",24);
        User u5 = new User(6,"e",25);

        List<User> list = Arrays.asList(u1,u2,u3,u4,u5);

        //  链式编程
        list.stream()
                .filter(u->{return u.getId()%2==0;})
                .filter(u->{return u.getAge()>23;})
                .map(u->{return u.getName().toUpperCase();})
                .sorted((uu1,uu2)->{return uu1.compareTo(uu2);})
                .limit(1)
                .forEach(System.out::println);
    }
}
```



## 14 ForkJoin

> 什么是forkjoin

大任务分成小任务

```java
/**
 * 如何使用ForkJoin
 * 1.forkjoinPool 通过它来执行
 * 2.ForkJoinTask
 *
 */
public class ForkJoinDemo extends RecursiveTask<Long> {

    private Long start;
    private Long end;

    private Long temp = 10000L;

    public ForkJoinDemo(Long start, Long end) {
        this.start = start;
        this.end = end;
    }


    @Override
    protected Long compute() {
        if((end-start)<temp){
            // 分支合并计算
            Long sum = 0L;
            for (Long i = start; i <= end; i++) {
                sum+=i;
            }
            return sum;

        }else {
            long middle = (start+end)/2;
            ForkJoinDemo task1 = new ForkJoinDemo(start, middle);
            task1.fork();
            ForkJoinDemo task2 = new ForkJoinDemo(middle+1, end);
            task2.fork();
            long sum = task1.join() + task2.join();
            return sum;
        }

    }
}
```



```java
/**
 * 同一任务效率高
 */
public class Test {
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        test(); // 7050
        test2(); // 5251
        test3(); // 358
    }

    public static void test(){
        long start = System.currentTimeMillis();
        Long sum = 0L;
        for (Long i = 1L; i <= 10_0000_0000L; i++) {
            sum+=i;
        }
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+"时间: "+(end-start));
    }

    // ForkJoin
    public static void test2() throws ExecutionException, InterruptedException {
//        System.out.println("sdfa");
        long start = System.currentTimeMillis();

        ForkJoinPool forkJoinPool = new ForkJoinPool();
        ForkJoinTask<Long> task = new ForkJoinDemo(0L, 10_0000_0000L);
//        forkJoinPool.execute(task);
        ForkJoinTask<Long> submit = forkJoinPool.submit(task);

        Long sum = submit.get();
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+"时间: "+(end-start));
    }

    // 使用stream方法
    public static void test3(){
        long start = System.currentTimeMillis();
        long sum = LongStream.rangeClosed(0L, 10_0000_0000L).parallel().reduce(0, Long::sum);
        long end = System.currentTimeMillis();
        System.out.println("sum="+sum+"时间: "+(end-start));
    }
}
```

## 15 异步回调

```java
public class Demo01 {

    public static void main(String[] args) throws ExecutionException, InterruptedException {
//        Future future;
        // 没有返回值的异步回调
/*        CompletableFuture<Void> completableFuture = CompletableFuture.runAsync(()->{
            try {
                TimeUnit.SECONDS.sleep(2);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(Thread.currentThread().getName()+"runAsync=>void");
        });
        System.out.println("111");
        completableFuture.get();// 执行结果*/

        CompletableFuture<Integer> completableFuture = CompletableFuture.supplyAsync(()->{
            System.out.println(Thread.currentThread().getName()+"suppluAsunc=>inteher");
            int i = 10/0;
            return 1024;
        });
        System.out.println(completableFuture.whenComplete((t, u) -> {
            System.out.println("t=>" + t); // 正确的返回值
            System.out.println("u=>" + u); // 错误信息
        }).exceptionally((e) -> {
            e.printStackTrace();
            e.getMessage();
            return 233;
        }).get());
        //
    }
}
```



## 16 JMM

> Volatile

Volatile是java虚拟机提供**轻量级的同步机制**

1. 保证可见性
2. 不保证原子性
3. 禁止指令重拍



> JMM

JMM: java内存模型,不存在的东西



**关于JMM的一些同步的约定**

1. 线程解锁前,必把共享变量立刻刷回主存
2. 线程加锁前，必须读取主存中的最新值到工作内存中
3. 加锁和解锁是同一把锁



线程 **工作内存** **主内存**

> JMM主内存和本地内存交互操作

**计算机硬件内存模型有缓存和主内存的交互协议MESI，同样JMM也规范了主内存和线程工作内存进行数据交换操作。一共包括如上图所示的8中操作，并且每个操作都是原子性的。**

1. **lock(锁定)：**作用于主内存的变量，一个变量在同一时间只能一个线程锁定。该操作表示该线程独占锁定的变量。
2. **unlock(解锁)：**作用于主内存的变量，表示这个变量的状态由处于锁定状态被释放，这样其他线程才能对该变量进行锁定。
3. **read(读取)：**作用于主内存变量，表示把一个主内存变量的值传输到线程的工作内存，以便随后的load操作使用。
4. **load(载入)：**作用于线程的工作内存的变量，表示把read操作从主内存中读取的变量的值放到工作内存的变量副本中(副本是相对于主内存的变量而言的)。
5. **use(使用)：**作用于线程的工作内存中的变量，表示把工作内存中的一个变量的值传递给执行引擎，每当虚拟机遇到一个需要使用变量的值的字节码指令时就会执行该操作。
6. **assign(赋值)：**作用于线程的工作内存的变量，表示把执行引擎返回的结果赋值给工作内存中的变量，每当虚拟机遇到一个给变量赋值的字节码指令时就会执行该操作。
7. **store(存储)：**作用于线程的工作内存中的变量，把工作内存中的一个变量的值传递给主内存，以便随后的write操作使用。
8. **write(写入)：**作用于主内存的变量，把store操作从工作内存中得到的变量的值放入主内存的变量中。

JMM规定了以上8中操作需要按照如下规则进行

- 不允许read和load、store和write操作之一单独出现，即不允许一个变量从主内存读取了但工作内存不接受，或者从工作内存发起回写了但主内存不接受的情况出现。
- 不允许一个线程丢弃它的最近的assign操作，即变量在工作内存中改变了之后必须把该变化同步回主内存。
- 不允许一个线程无原因地（没有发生过任何assign操作）把数据从线程的工作内存同步回主内存中。
- 一个新的变量只能在主内存中“诞生”，不允许在工作内存中直接使用一个未被初始化（load或assign）的变量，换句话说就是对一个变量实施use和store操作之前，必须先执行过了assign和load操作。
- 一个变量在同一个时刻只允许一条线程对其进行lock操作，但lock操作可以被同一条线程重复执行多次，多次执行lock后，只有执行相同次数的unlock操作，变量才会被解锁。
- 如果对一个变量执行lock操作，将会清空工作内存中此变量的值，在执行引擎使用这个变量前，需要重新执行load或assign操作初始化变量的值。
- 如果一个变量事先没有被lock操作锁定，则不允许对它执行unlock操作，也不允许去unlock一个被其他线程锁定住的变量。

以上8中规则看着也是比较生涩的，其实如果你没看明白也没关系，其实这些规则就是保障数据同步的一些规则。不是很重要，重要的在后面的happens-before原则。

## 17 volatile

