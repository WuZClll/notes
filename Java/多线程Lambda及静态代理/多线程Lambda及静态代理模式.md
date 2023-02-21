

[多线程01：概述_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1V4411p7EF?p=1&vd_source=796ed40051b301bfa3a84ba357f4828c)的自学笔记

>1. [多线程](##一. 多线程)
>2. [Lambda表达式](##二. Lambda表达式)
>3. [静态代理模式](##三.  静态代理模式)
>
>

## 一. 多线程

多线程是[静态代理](##三. 静态代理模式)模式

- 线程就是独立的执行路径

- 在程序运行时，即使没有自己创建线程，后台也会有多个线程，如主线程，gc线程

- main()称之为主线程，为系统的入口，用于执行整个程序

- 在一个进程中，如果开辟了多个线程，线程的运行由**调度器(cpu)安排调度**，调度器是与操作系统紧密相关的，先后顺序是不能人为的干预的

- 对同一份资源操作时，会存在资源抢夺的问题，需要加入**并发控制**

- 线程会带来额外的开销，如cpu调度时间，并发控制开销

- 每个线程在自己的工作内存交互，内存控制不当会造成数据不一致

  > 当发生并发时，如买票，多个人同时买，他们都发现售票处还有一张票，就把一张票拷贝的自己的内存区域，然后进行购买，结果是每人买了一张票，购买时售票处票数`--`，售票处还有负数张票）

### 1. 线程的三种实现方式

> 1. [继承Thread类(重点)](####1.1 继承Thread类(重点))
>    - 不建议使用，OOP单继承局限性
>      - 单继承：java只能继承一个，不能继承多个，只能有一个父类
> 2. [实现Runnable接口(重点)](####1.2 实现Runnable接口(重点))
>    - 推荐使用，避免单继承局限性，灵活方便，方便同一个对象被多个线程使用
> 3. [实现Callable接口(了解)](####1.3 实现Callable接口(了解))

注：

>1和2两种方式本质时一样的：
>
>1. 源码中Thread类实现了Runnable接口
>   - 继承Thread类便等同于实现了Runnable接口，它就是一个线程类了
>2. Thread类中有start()sleep()等方法
>   - 继承Thread类就有了start()等方法
>   - 实现Runnable接口这个方式没有这些方法，需要用Thread代理使用这些方法
>     - Thread代理就是用Thread带参构造方法将Runnable接口实现类对象作为参数创建Thread对象，调用start()等方法

#### 1.1 继承Thread类(重点)

1. 自定义线程类继承`extends Thread`

   ```java
   public class TestThread1 extends Thread{}
   ```

2. 重写`run()`方法，编写线程执行体
   ```java
   @Override
       public void run(){}
   ```
   
3. *创建自定义线程类对象，调用start()方法启动线程*(与实现[Runnable接口](###2. 实现Runnable接口)有区别)
   
   ```java
   TestThread1 testThread1 = new TestThread1();
   testThread1.start();
   ```

##### 1.1.1 演示

```java
// 创建线程方式1：继承Thread类
public class TestThread1 extends Thread{
    @Override
    public void run() {
        //run方法线程体
        for (int i = 0; i < 2000; i++) {
            System.out.println("我在看代码--" + i);
        }
    }

    public static void main(String[] args) {
        //main线程，主线程
        //创建一个线程对象
        TestThread1 testThread1 = new TestThread1();
        //调用start()方法开启线程
        testThread1.start();//线程不一定立即执行CPU安排调度

        for (int i = 0; i < 2000; i++) {
            System.out.println("我在学习多线程--" + i);
        }
    }
}
```

##### 1.1.2 案例 

###### 1.1.2.1 Thread实现多线程同步下载图片

> 首先需要jar包([Commons IO – Download Apache Commons IO](https://commons.apache.org/proper/commons-io/download_io.cgi))用于下载网络图片

```java
import org.apache.commons.io.FileUtils;

import java.io.File;
import java.io.IOException;
import java.net.URL;
// 练习
//  Thread实现多线程同步下载图片
//  首先需要引入jar包
public class TestThread2 extends Thread{
    private String url;//网络图片地址
    private String name;//保存的文件名

    public TestThread2(String url, String name) {
        this.url = url;
        this.name = name;
    }
    @Override
    public void run() {
        WebDownloader webDownloader = new WebDownloader();
        webDownloader.downloader(url, name);
        System.out.println("现在的文件名为：" + name);
    }

    public static void main(String[] args) {
        TestThread2 t1 = new TestThread2("https://img-home.csdnimg.cn/images/20230213094345.jpg", "1.jpg");
        TestThread2 t2 = new TestThread2("https://img-home.csdnimg.cn/images/20230213094345.jpg", "2.jpg");
        TestThread2 t3 = new TestThread2("https://img-home.csdnimg.cn/images/20230213094345.jpg", "3.jpg");
        t1.start();
        t2.start();
        t3.start();
    }
}

class WebDownloader {
    //下载方法
    public void downloader(String url, String name) {
        try {
            FileUtils.copyURLToFile(new URL(url), new File(name));//把一个网络url变为图片下载为name
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常,download方法出现问题");
        }
    }
}
```

#### 1.2 实现Runnable接口(重点)

1. 定义类，实现Runnable接口
   ```java
   public class TestThread3 implements Runnable{}
   ```
   
2. 实现run()方法，编写线程执行体
   ```java
   @Override
       public void run() {}
   ```
   
3. 创建Runnable接口的实现类对象
   ```java
   TestThread3 testThread3 = new TestThread3();
   ```
   
4. *创建线程对象，调用线程对象调用start()方法启动线程*, <u>代理</u>   (与[继承Thread类](###1. 继承Thread类)有区别)

   ```java
   Thread thread = new Thread(testThread3);//代理
   thread.start();
   ```
   
   

##### 1.2.1 演示

```java
public class TestThread3 implements Runnable{
    @Override
    public void run() {
        //run方法线程体
        for (int i = 0; i < 2000; i++) {
            System.out.println("我在看代码--" + i);
        }
    }

    public static void main(String[] args) {
        //main线程，主线程
        //创建一个Runnable接口的实现类(一份资源)
        TestThread3 testThread3 = new TestThread3();
        
/*
        //创建线程对象，通过线程对象来开启线程，(多次代理)
        Thread thread1 = new Thread(testThread3);//参数: runnable接口的实现类
        Thread thread2 = new Thread(testThread3);
        //调用start()方法开启线程
        thread.start();
        thread.start();
*/
        
        //创建线程对象，通过线程对象来开启线程，(多次代理)
        new Thread(testThread3).start();//同上两行代码
        new Thread(testThread3).start();

        for (int i = 0; i < 2000; i++) {
            System.out.println("我在学习多线程--" + i);
        }
    }
}
```

##### 1.2.2 案例(多个线程同时操作一同个对象)

###### 1.2.2.1 买火车票

```java
//多个线程同时操作一个对象
//买火车票

//发现问题 多个资源操作同一个资源的情况下，线程不安全，数据紊乱
//	有时抢到同一张票，又有时抢到0或-1张票
public class TestThread4 implements Runnable{

    //票数
    int tickNumbers = 10;

    @Override
    public void run() {
        while (true) {
            if (tickNumbers <= 0) {
                break;
            }
            //模拟延时
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
            System.out.println(Thread.currentThread().getName() + "拿到了第" + tickNumbers-- + "张票");
        }
    }

    public static void main(String[] args) {
        TestThread4 ticket = new TestThread4();

        new Thread(ticket, "小明").start();
        new Thread(ticket, "老师").start();
        new Thread(ticket, "黄牛党").start();
    }
}
```

###### 2.2.2 龟兔赛跑

```java
//模拟龟兔赛跑
public class Race implements Runnable {
    //胜利者
    private static String winner;

    @Override
    public void run() {
        for (int i = 0; i <= 1000; i++) {

            //模拟兔子休息 且每50步休息一次
            if (Thread.currentThread().getName().equals("兔子") && i%50 == 0) {
                try {
                    Thread.sleep(10);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            //判断比赛是否结束
            boolean flag = gameOver(i);
            if (flag) {
                break;
            }
            System.out.println(Thread.currentThread().getName() + "跑了" + i + "步");
        }
    }


    /**
     * 判断比赛是否结束
     * @param steps 步数
     * @return
     */
    private boolean gameOver(int steps) {
        if (winner != null) {
            //已经存在胜利者了
            return true;
        }
        {   //这是一个代码块
            if (steps >= 1000) {
                winner = Thread.currentThread().getName();
                System.out.println("winner is " + winner);
                return true;
            }
        }
        return false;
    }

    public static void main(String[] args) {
        Race race = new Race();//一条赛道

        new Thread(race, "兔子").start();
        new Thread(race, "乌龟").start();
    }
}
```

#### 1.3 实现Callable接口(了解)

1. 实现Callable接口，需要返回值类型
   ```java 
   implements Callable<返回值类型> 
   ```

2. `重写call方法，需要抛出异常

   ```java
   @Override
   public 返回值类型 call() {}
   ```

3. 创建目标对象(Callable接口实现类)

   ```java
   目标类 目标对象 = new 目标类();
   ```

4. 创建执行服务(线程池服务为固定几个线程的线程池) Executor: 线程池 fixed：固定的 Pool：池
   ```java
   ExecutorService ser = Executors.newFixedThreadPool(线程个数);
   ```

5. 提交执行(返回一个封装返回值的Future类)
   ```java
   Future<返回值类型> result1 = ser.submit(目标对象);
   Future<返回值类型> result2 = ser.submit(目标对象);
   //...  这里可以写线程个数个执行
   ```

6. 获取结果
   ```java
   返回值类型 r1 = result1.get();
   返回值类型 r2 = result1.get();
   //...
   ```

7. 关闭服务
   ```java
   ser.shutdownNow();
   ```

好处

>Callable的好处
>
>1. 可以定义返回值
>2. 可以抛出异常

##### 1.3.1 演示

```java
public class TestCallable implements Callable<Boolean> {//Boolean是返回值类型
    @Override
    public Boolean call() {//这里的call()方法类似上两种方式的run()方法
        //cal方法线程体
        for (int i = 0; i < 2000; i++) {
            System.out.println("我在看代码--" + i);
        }
        return true;
    }
    public static void main(String[] args) throws ExecutionException, InterruptedException {
        TestCallable t1 = new TestCallable();//创建Callable接口实现类
        TestCallable t2 = new TestCallable();
        TestCallable t3 = new TestCallable();
        
        //创建执行服务(线程池服务 创建固定线程池) Executor: 线程池 fixed：固定的 Pool：池
        ExecutorService ser = Executors.newFixedThreadPool(3);//3是固定线程池的个数
        
        //提交执行 (返回一个封装返回值类型的Future类)
        Future<Boolean> r1 = ser.submit(t1);
        Future<Boolean> r2 = ser.submit(t2);
        Future<Boolean> r3 = ser.submit(t3);
        
        //获取结果
        boolean rs1 = r1.get();
        boolean rs2 = r2.get();
        boolean rs3 = r3.get();

        System.out.println(rs1);
        System.out.println(rs2);
        System.out.println(rs3);
        //关闭线程池服务
        ser.shutdownNow();
    }
}
```

##### 1.3.2 案例

###### 1.3.2.1 利用Callable改造下载图片案例

[点击查看原下载图片案例](#####1.1.2.1 Thread实现多线程同步下载图片)

```java
public class TestCallable implements Callable<Boolean> {
    private String url;//网络图片地址
    private String name;//保存的文件名

    public TestCallable(String url, String name) {
        this.url = url;
        this.name = name;
    }
    @Override
    public Boolean call() {
        WebDownloader webDownloader = new WebDownloader();
        webDownloader.downloader(url, name);
        System.out.println("现在的文件名为：" + name);
        return true;
    }

    public static void main(String[] args) throws ExecutionException, InterruptedException {
        TestCallable t1 = new TestCallable("https://img-home.csdnimg.cn/images/20230213094345.jpg", "1.jpg");
        TestCallable t2 = new TestCallable("https://img-home.csdnimg.cn/images/20230213094345.jpg", "2.jpg");
        TestCallable t3 = new TestCallable("https://img-home.csdnimg.cn/images/20230213094345.jpg", "3.jpg");

        //创建执行服务 Executor: 线程池 fixed：固定的 Pool：池
        ExecutorService ser = Executors.newFixedThreadPool(3);

        //提交执行
        Future<Boolean> r1 = ser.submit(t1);
        Future<Boolean> r2 = ser.submit(t2);
        Future<Boolean> r3 = ser.submit(t3);

        //获取结果
        boolean rs1 = r1.get();
        boolean rs2 = r2.get();
        boolean rs3 = r3.get();

        System.out.println(rs1);
        System.out.println(rs2);
        System.out.println(rs3);
        //关闭服务
        ser.shutdownNow();
    }
}

class WebDownloader {
    //下载方法
    public void downloader(String url, String name) {
        try {
            FileUtils.copyURLToFile(new URL(url), new File(name));//把一个网络url变为图片下载为name
        } catch (IOException e) {
            e.printStackTrace();
            System.out.println("IO异常,download方法出现问题");
        }
    }
```

### 2. 线程的五大状态流程及方法

#### 2.1 五大状态流程图

```mermaid
graph TB
%% 创建状态：new Thread();
%% 启动状态：thread.start();
%% 运行状态：启动之后CPU调度完了
aa["new Thread"]-->a[创建状态]--"启动线程(start)"-->b[就绪状态];
b--获得CPU资源-->c[运行状态]
c--"dead()"-->d[死亡状态]
c--"sleep()、wait()、同步锁定等"-->e[阻塞状态]
c--"yield()线程礼让"-->b
e--阻塞解除-->b
```

#### 2.2 线程方法

| 方法                           | 说明                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| setPriority(int newPriority)   | 更改线程的优先级                                             |
| static void sleep(long millis) | 让当前正在执行的线程休眠millis毫秒                           |
| void join()                    | 等待该线程终止                                               |
| static void yield()            | 线程礼让，从运行状态转为就绪状态，重新开始cpu调度，从运行状态转为就绪状态 |
| void interrupt()               | 中断线程(不建议用这种方式)                                   |
| boolean isAlive()              | 测试线程是否处于活动状态                                     |

#### 2.3【stop】停止

- 这里的stop是自己写的，需要自己控制

##### 2.3.1 详解

- 不推荐使用JDK提供的stop()、destriy方法【已废弃】

- 推荐线程自己停下来

  - 建议使用一个标志位进行终止变量，当flag=false，则终止线程
    
    ```java
    public class Thread1 implements Runnable{	
    	@Override
    	public void run() {
    	    //线程体使用该标志
    	    while (flag) {
    	        //...
    	    }
    	}
    	//在线程中写一个stop方法，用于修改标志位
    	public void stop() {
    	    this.flag = false;
    	}
    }
    ```
    

##### 2.3.2 代码演示

```java
public class TestStop implements Runnable{

//    1.设置一个标志位
    private boolean flag = true;
    @Override
    public void run() {
        int i = 0;//线程执行的次数
        while (flag) {
            System.out.println("线程执行run...Thread" + i++ + "次");
        }
    }

//    2.设置一个公开的方法停止线程，转换标志位
    public  void stop() {
        this.flag = false;
    }

    public static void main(String[] args) {
        TestStop testStop = new TestStop();
        new Thread(testStop).start();
        System.out.println("线程开始了");

        for (int i = 0; i < 2000; i++) {//i main方法执行的次数
            System.out.println("main执行" + i + "次");
            if (i == 900) {
//                调用stop方法切换标志位，让线程停止
                testStop.stop();
                System.out.println("线程停止了");
            }
        }
    }
}
```

#### 2.4 sleep休眠

##### 2.4.1 详解

- sleep(毫秒值)：指定当前线程阻塞的毫秒值
- sleep存在InterruptedException
- sleep时间达到后线程进入就绪状态
- sleep可以模拟网络延时，倒计时等
- 每个对象都有一个锁🔒，sleep不会释放锁

##### 2.4.2 代码演示

```java
////打印10次当前系统时间，每秒打印一次
public static void main(String[] args) {
        Date startTime = new Date(System.currentTimeMillis());//获取系统当前时间
        int i = 0;
        while (i < 10) {
            try {
                Thread.sleep(1000);
                System.out.println(new SimpleDateFormat("HH:mm:ss").format(startTime));
                startTime = new Date(System.currentTimeMillis());
                i++;
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
}
```

#### 2.5 yield礼让

- 礼让就是重新开始cpu调度，从运行状态转为就绪状态

##### 2.5.1 详解

- 将线程从运行状态转为就绪状态，但不阻塞，与其他线程重新竞争cpu
- 让cpu重新调度，礼让不一定会比其他线程慢，看cpu心情

##### 2.5.2 代码演示

```java
public class TestYield {
    public static void main(String[] args) {
        MyYield myYield = new MyYield();

        new Thread(myYield, "a").start();
        new Thread(myYield, "b").start();
    }
}

class MyYield implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "线程开始执行");
        //礼让后，甲从运行状态转为暂停(就绪状态)，乙本就是就绪状态，cpu重新调度，可能出现甲继续运行
        Thread.yield();
        System.out.println(Thread.currentThread().getName() + "线程停止执行");
    }
}
```

#### 2.6 join插队

##### 2.6.1 详解

- 强制执行且很霸道
- 类似vip，没有插队(没有vip)时公平执行，插队(有vip)后强制执行插队线程
- 直到插队(vip)线程执行完毕才可执行其他线程

##### 2.6.2 代码演示

```java
//插队,且很霸道
public class TestJoin implements Runnable{
    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("线程vip来啦" + i);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        //启动我们的线程
        TestJoin testJoin = new TestJoin();
        Thread thread = new Thread(testJoin);
        thread.start();

        //主线程
        for (int i = 0; i < 1000; i++) {
            if (i == 200) {
                thread.join();//插main线程的队，当main线程执行到199时thread执行且必须thread执行完后main才能继续执行
            }
            System.out.println("main" + i);
        }
    }
}
```

#### 2.7 观测线程状态

##### 2.7.1 语法

state是Thread中一个枚举类型的变量

```java
Thread.State state = thread.getState();
```

##### 2.7.2 含义

| 状态          | 简述 | 含义                                                   |
| ------------- | ---- | ------------------------------------------------------ |
| NEW           | 新生 | 尚未启动的线程                                         |
| RUNNABLE      | 运行 | Java虚拟机中执行的线程                                 |
| BLOCKED       | 阻塞 | 被阻塞等待监视器锁定的线程                             |
| WAITING       | 阻塞 | 正在等待另一个线程执行特定动作的线程                   |
| TIMED WAITING | 阻塞 | 正在等待另一个线程执行动作达到指定时间的线程处于此状态 |
| TERMINATED    | 死亡 | 已退出的线程                                           |

### 3.  线程优先级

#### 3.1 详情

线程的优先级用数字表示，范围从0~10(数字越大优先级越高，cpu越容易调度)

```java
//Thread中源码的一些常量
public static final int MAX_PRIORITY = 10;
...
```

> Thread.MIN_PRIORITY = 1
> Thread.MAX_PRIORITY = 10
> Thread.NORE_PRIORITY = 5

使用`getPriority()`和`setPriority(int xxx)`来获取和改变优先级

#### 3.2 代码演示

```java
//打印不同优先级的各线程，观察执行顺序
public class TestPriority{
    public static void main(String[] args) {
        //main主线程是默认优先级5，无法改变
        System.out.println(Thread.currentThread().getName() + "-->" + Thread.currentThread().getPriority());

        MyPriority myPriority = new MyPriority();
        Thread t1 = new Thread(myPriority);
        Thread t2 = new Thread(myPriority);
        Thread t3 = new Thread(myPriority);
        Thread t4 = new Thread(myPriority);
        Thread t5 = new Thread(myPriority);
        Thread t6 = new Thread(myPriority);

        //设置优先级（优先级再1-10之间）
        t1.setPriority(Thread.MIN_PRIORITY);//1
        t2.setPriority(2);
        t3.setPriority(3);
        t4.setPriority(4);
        t5.setPriority(5);
        t6.setPriority(Thread.MAX_PRIORITY);//10

        //启动
        t1.start();
        t2.start();
        t3.start();
        t4.start();
        t5.start();
        t6.start();
    }
}

class MyPriority implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + "-->" + Thread.currentThread().getPriority());
    }
}
```

### 4. 守护线程(daemon)

#### 4.1 详解

- 线程分为用户现程(默认就是用户线程)和守护线程
  ```java
  //将thread线程设为守护线程 Daemon：守护线程
  thread.setDaemon(true);//默认是false表示是用户线程，不是守护线程
  ```
- 虚拟机必须确保「用户线程」执行完毕
- 虚拟机不用等待守护线程执行完毕
- 如，后台记录操作日志，监控内存，垃圾回收等待

> 注：
>
> 1. 虚拟机不会管守护线程，其他线程跑完后不管守护线程是否跑完，虚拟机都会关闭
> 2. 虚拟机关闭需要时间，因此守护线程在用户线程跑完后仍会运行一段时间

#### 4.2 代码演示

```java
//上帝守护你直到你死去
public class TestDaemon {
    public static void main(String[] args) {
        God god = new God();
        You you = new You();

        Thread thread = new Thread(god);
        thread.setDaemon(true);//默认是false表示是用户线程，true为守护线程

        thread.start();//上帝 守护线程  虚拟机不会管守护线程，其他线程跑完后不管守护线程是否跑完，虚拟机仍会结束，虚拟机关闭需要时间，因此守护线程在用户线程跑完后仍会运行一段时间

        new Thread(you).start();//你 用户线程启动
    }
}

//上帝 上帝一直在保佑着你，直到你死去
class God implements Runnable {
    @Override
    public void run() {
        while (true) {
            System.out.println("上帝保佑着你");
        }
    }
}

//你 活了16500天
class You implements Runnable {
    @Override
    public void run() {
        for (int i = 0; i < 36500; i++) {
            System.out.println("今天你成功生存了" + i + "天");
        }
        System.out.println("====goodbye！world====");
    }
}
```

### 5.  线程同步

#### 5.1 发现问题：

- 多个线程操作同一块存库空间，带来方便的同时，也带来了「并发」问题

- 并发：同一个对象被多个线程同时操作

- 多个线程访问同一对象，且某些线程还想修改这个对象，我们就需要「线程同步」来解决这个问题

  > 比如银行：
  > 同一张银行卡存了300w RMB
  > 你和你媳妇同时取钱出现并发问题，
  > 你在手机上取100RMB，
  > 你媳妇在银行取300w RMB，
  > 这样就会获得300.01w RMB,出现问题，
  > 银行血亏100RMB
  
- 当发生并发时，如买票，多个人同时买，他们都发现售票处还有一张票，就把一张票拷贝的自己的内存区域，然后进行购买，结果是每人买了一张票，购买时售票处票数`--`，售票处还有负数张票）

#### 5.2 解决问题「队列 + 锁」

- 线程同步其实就是一种**等待机制**

- 解决方式：「队列 + 锁」

  - **保证线程的安全性**

    > eg:上厕所
    >
    > 多个用户同时上厕所，需要排队
    > 上厕所的人会把坑上的门反锁保证其余人不会进入
    >
    > 当上完厕所后打开锁
    > 队列中下一人进入

    - 多个需要同时访问此对象的线程进入这个对象的「对象等待池」形成**队列**，等待前面线程执行完毕，下一个线程再使用

    - 为保证队列，在访问时加入了**锁机制(synchronized)**，当一个线程获得对象的排他锁后，独占资源，其他线程必须等待，使用后释放锁即可

    - 每个线程**在自己的工作内存交互**，内存控制不当会造成数据不一致

      > eg:买票，多个人同时买，他们都发现售票处只有一张票时，就把售票处还有一张票拷贝的自己的内存区域，然后进行购买，结果是每人买了一张票，购买时售票处票数`--`，结果是售票处还有负数张票）

  - **队列 + 锁会出现以下问题**

    - 一个线程持有锁会导致其他所有需要此锁的线程**挂起**(性能变慢)
    - 在多线程竞争下，加锁，释放锁会导致比较多的「上下文切换」和「调度延时」，引起性能问题(性能变慢)
    - 如果一个优先级高的线程等待一个优先级低的线程释放锁会导致**优先级倒置**，引起性能问题

#### 5.3 synchronized锁的两种用法

synchronized()方法 和 synchronized块

- synchronized()方法只能锁this
- 方法里面需要修改的内容才需要锁，锁的太多会浪费资源，由此产生synchronized块
- synchronized(Obj){}代码块可以锁任意对象，锁住的对象就是变化的对象，需要增删改的对象

##### 5.3.1 synchronized()方法

```java
 public synchronized void method(int args){}
```

- 同步方法的Obj「同步监视器」就是this
- synchronized方法控制对「对象」的访问，每个对象对应一把锁，每个synchronized方法必须调用该方法的对象的锁才能执行synchronized方法，否则线程会阻塞
- 方法一旦执行，就独占该锁，直到该方法返回才释放锁，后面被阻塞的线程才能获得这个锁继续执行
- 缺陷：若将一个大的方法申明为synchronized将会影响效率（不高效）

##### 5.3.2 synchronized同步块

```java 
synchronized(Obj){}
```

- 产生：方法里面需要修改的内容才需要锁，锁的太多会浪费资源，由此产生synchronized块
- Obj 就是锁住的对象，称之为「同步监视器」
  - Obj 可以是**任何对象**，但是推荐使用共享资源作为同步监视器
  - 锁住的对象就是变化的对象，需要增删改的对象
  - 同步方法中无需指定同步监视器，因为同步方法的同步监视器就是this，就是这换个对象本身，或者是 class【反射中】
- Obj「同步监视器」的执行过程
  1. 第一个线程访问，锁定同步监视器，执行其中代码
  2. 第二个线程访问，发现同步监视器被锁定，无法访问
  3. 第一个线程访问完毕，解锁同步监视器
  4. 第二个线程访问，发现同步监视器没有锁，然后锁定并访问



#### 5.4 案例

##### 5.4.1 买票（synchronized方法）

```Java
//不安全的买票(会出现多个线程同时发现还有1张票，并将有一张票的信息拷贝到自己内存空间，多个线程同时买这一张票，售票处票数多次--，出现负数)
//解决方式：synchronized方法（锁的是this）
public class UnsafeBuyTicket {
    public static void main(String[] args) {
        BuyTicket station = new BuyTicket();

        new Thread(station,"苦逼的我").start();
        new Thread(station,"牛逼的你们").start();
        new Thread(station,"可恶的黄牛党").start();
    }
}

class BuyTicket implements Runnable {
    private int ticketNums = 10;
    boolean flag = true;//外部停止方式

    @Override
    public void run() {
        //买票
        while (flag) {
            try {
                buy();
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }

    //买票
    private void buy() throws InterruptedException {
        //判断是否有票
        if (ticketNums <= 0) {
            flag = false;
            return;
        }
        Thread.sleep(100);
        //买票
        System.out.println(Thread.currentThread().getName() + "拿到" + ticketNums--);
    }
}
```

##### 5.4.2 集合添加数据(同步代码块)

```java
//线程不安全的集合(两个线程在同一瞬间可能会在同一个位置添加数据，数据会被覆盖掉，也就是添加的位置不安全)
//解决方式：synchronized同步块（锁的是变化的对象）
public class UnsafeList {
    public static void main(String[] args) throws InterruptedException {
        List<String> list = new ArrayList<>();
        for (int i = 0; i < 10000; i++) {
            new Thread(()-> {
                //synchronized块（锁的是变化的对象list）
                synchronized (list) {
                    list.add(Thread.currentThread().getName());//添加时会在最后一个位置添加
                }
            }).start();
        }
        Thread.sleep(3000);
        System.out.println(list.size());//打印集合的元素个数，可能少于10000
    }
}
```



## 二. Lambda表达式

### 1.简介及语法

lambda表达式是函数式编程

**前提**

> 必须是函数式接口

理解函数式接口Functional interface(函数式接口) 是学习Java8 lambda的关键所在

**语法**

- 任何接口，如果只包含一个抽象方法，那么它就是一个**函数式接口**
  ```java
  public interface Runnable {
      public abstract void run();
  }
  ```

- 对于函数式接口，我们可以通过**lambda表达式**来创建该接口的对象
  ```java
  //当抽象方法需要传参时,()内可以传参数
  //如ISum i = (int a) -> {};
  Runnable r1 = () -> {
      System.out.println("在此写Runnable接口内的唯一抽象方法run()的方法体");
  }//相当于创建了一个类实现了Runnable接口
  ```

- 简化

  1. 可以去掉参数类型(多个参数时也可省略)
  2. 可以去掉括号(多个参数时不可省略括号)
  3. 可以去掉花括号(仅限只有一行方法体，如果有多行仍需花括号包裹)

### 2.Lambda表达式的诞生

#### 2.1 简化过程：

```mermaid
graph LR
a([1.外部类])-.实现.->b[[接口]]
c([2.静态内部类])-.实现.->b
d([3.局部内部类])-.实现.->b
e([4.匿名内部类])-.借助.->b
f([5.Lambda表达式])-.利用.->b
a--简化-->c
c--简化-->d
d--简化-->e
e--简化-->f
```

#### 2.2 代码演示

##### 2.2.1 Lambda的演变

```java
/*
推导Lambda表达式
注释2->5逐渐简化
 */
public class TestLambda1 {
    //3.静态内部类
    static class Like2 implements ILike {
        @Override
        public void lambda() {
            System.out.println("I like Lambda2");
        }
    }

    public static void main(String[] args) {
        ILike like = new Like();//实现类
        like.lambda();

        like = new Like2();//静态内部类
        like.lambda();

        //4.局部内部类
        class Like3 implements ILike {
            @Override
            public void lambda() {
                System.out.println("I like Lambda3");
            }
        }
        like = new Like3();
        like.lambda();

        //5.匿名内部类,没有类名称，必须借助接口或者父类
        like = new ILike() {
            @Override
            public void lambda() {
                System.out.println("I like Lambda4");
            }
        };
        like.lambda();

        //6.用Lambda简化
        like = () -> {
            System.out.println("I like Lambda5");
        };
        like.lambda();
    }
}
//1. 定义一个函数式接口
interface ILike {
    void lambda();
}
//2.实现类
class Like implements ILike {

    @Override
    public void lambda() {
        System.out.println("I like Lambda1");
    }
}
```

##### 简化Lambda表达式

```java
public class TestLambda2 {
    public static void main(String[] args) {
        ILove love = null;

        //4.0 lambda
        love = (String a)->{
            System.out.println("I love you-->" + a);
        };
        love.love("小青1");

        //4.1 简化1：去掉参数类型(多个参数时也可省略)
        love = (a)->{
            System.out.println("I love you-->" + a);
        };
        love.love("小青2");

        //4.2 简化2：去掉括号(多个参数时不可省略括号)
        love = a->{
            System.out.println("I love you-->" + a);
        };
        love.love("小青3");

        //4.3：去掉花括号(仅限只有一行方法体，如果有多行仍需花括号包裹)
        love = a-> System.out.println("I love you-->" + a);
        love.love("小青4");

    }
}
interface ILove {
    void love(String a);
}
```

## 三. 静态代理模式

### 3.1 介绍

**静态代理模式：**
    真实对象(个人)和代理对象(婚介公司)都要实现同一个接口(干一件事)
    代理对象代理真实角色(创建代理对象时要传入目标对象)
**好处：**
    代理对象可以做很多真实对象做不了的事(布置现场)
    真实对象专注于自己的事情(结婚)

**Thread是代理模式**
		`new Thread(() -> System.out.println("我爱你")).start();`
		Thread相当于代理对象
		()相当于真实对象，也就是Runnable实现类，重写run方法调用run

### 3.2 代码演示

```java
public class StaticProxy {
    public static void main(String[] args) {
        You you = new You();//你要结婚(真实对象)

        WeddingCompany weddingCompany = new WeddingCompany(you);//婚介公司帮助你结婚(代理对象传入真实对象)
        weddingCompany.HappyMarry();//两对象共同完成结婚

        new WeddingCompany(new You()).HappyMarry();
    }
}

//结婚
interface Marry {
    //开心的结婚
    void HappyMarry();
}


//真实角色，你 去结婚 真实对象和代理对象都要实现同一个接口
//要去结婚的你类
class You implements Marry {
    @Override
    public void HappyMarry() {
        System.out.println("你：结婚中，敲开森！！");
    }
}

//代理角色， 帮助你结婚 真实对象和代理对象都要实现同一个接口
//婚介公司类
class WeddingCompany implements Marry {
    private Marry target;
	
    //代理对象要有参数为真实对象的构造方法
    public WeddingCompany(Marry target) {
        this.target = target;
    }

    //公司业务
    @Override
    public void HappyMarry() {
        //结婚前
        before();
        //你结婚
        this.target.HappyMarry();
        //结婚后
        after();

    }

    private void before() {
        System.out.println("婚介公司：结婚之前，布置现场");
    }
    private void after() {
        System.out.println("婚介公司：结婚之后，收尾款");
    }

}
```

