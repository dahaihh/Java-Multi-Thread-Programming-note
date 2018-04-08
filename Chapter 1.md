# 第一章 Java多线程技能
## 1.1 创建线程
### 1.1.1 继承Thread类
```
/**
 * @className: Method1
 * @package: com.jy.modules.MultiThread.createthread
 * @describe: 方式1：继承Thread类
 * @auther: huanghai
 * @date: 2018/4/8 0008
 * @time: 下午 2:18
 **/
public class Method1 extends Thread{

    @Override
    public void run() {
        super.run();
        System.out.println("Method1");
    }
}

/**
 * @className: Test
 * @package: com.jy.modules.MultiThread.createthread
 * @describe: 创建线程的测试类
 * @auther: huanghai
 * @date: 2018/4/8 0008
 * @time: 下午 2:20
 **/
public class Test {

    public static void main(String[] args) {
        Thread t1 = new Method1();
        t1.start();
    }

}
```
### 1.1.2 实现Runnable接口
```
/**
 * @className: Test
 * @package: com.jy.modules.MultiThread.createthread
 * @describe: 创建线程的测试类
 * @auther: huanghai
 * @date: 2018/4/8 0008
 * @time: 下午 2:20
 **/
public class Test {

    public static void main(String[] args) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("运行中！");
            }
        }).start();
    }

}
```
### 1.1.3 实例变量和线程安全
```
/**
 * @className: MyThread
 * @package: com.jy.modules.MultiThread.createthread
 * @describe:共享变量与非共享变量
 * @auther: huanghai
 * @date: 2018/4/8 0008
 * @time: 下午 2:55
 **/
public class MyThread extends Thread{

    private int count = 5;

    public MyThread (String name) {
        super();
        this.setName(name);// 设置线程名称
    }

    @Override
    public void run() {
        super.run();
        while (count > 0) {
            count --;
            System.out.println("由" + this.currentThread().getName() + "计算，count=" + count);
        }
    }

}
/**
 * @className: Test
 * @package: com.jy.modules.MultiThread.createthread
 * @describe: 共享变量与非共享变量的测试类
 * @auther: huanghai
 * @date: 2018/4/8 0008
 * @time: 下午 2:20
 **/
public class Test {

    public static void main(String[] args) {
        Thread a = new MyThread("A");
        Thread b = new MyThread("B");
        Thread c = new MyThread("C");
        a.start();
        b.start();
        c.start();
    }

}
```
>变量不共享的情况，分别创建了三个线程，每个线程分别减少count值。
```
/**
 * @className: MyThread
 * @package: com.jy.modules.MultiThread.createthread
 * @describe:共享变量与非共享变量
 * @auther: huanghai
 * @date: 2018/4/8 0008
 * @time: 下午 2:55
 **/
public class MyThread extends Thread{

    private int count = 5;

    @Override
    public void run() {
        super.run();
        count --;
        System.out.println("由" + this.currentThread().getName() + "计算，count=" + count);
    }

}
/**
 * @className: Test
 * @package: com.jy.modules.MultiThread.createthread
 * @describe: 共享变量与非共享变量的测试类
 * @auther: huanghai
 * @date: 2018/4/8 0008
 * @time: 下午 2:20
 **/
public class Test {

    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        Thread a = new Thread(myThread,"A");
        Thread b = new Thread(myThread,"B");
        Thread c = new Thread(myThread,"C");
        Thread d = new Thread(myThread,"D");
        Thread e = new Thread(myThread,"E");
        a.start();
        b.start();
        c.start();
        d.start();
        e.start();
    }

}
```
>变量共享的情况，5个线程都可以访问同一个变量count，因此可能产生非线程安全性的问题

synchronized关键字可以使多个线程在执行run方法时，进行排队操作；
**实现原理**：当一个线程在调用run方法前，先判断run方法有没有上锁，如果上锁证明有线程在调用run方法，则必须等其他线程调用结束后才可以调用run方法；
synchronized关键字可以在*任意对象或方法上加锁*，加锁的这段代码称为“互斥区”或“临界区”；

### 1.1.4 留意i--和System.out.println()的异常
>虽然println()方法内部是同步的，但i--是在进入println()方法内部之前发生的，所以有发生非线程安全问题的概率。

## 1.2 currentThread()方法
>返回当前代码段正在被哪个线程调用的信息

## 1.3 isAlive()方法
>判断当前线程是否处于**活动状态**，活动状态是指线程已经启动且尚未终止。线程处于正在运行或准备开始运行的状态就认为线程是存活的

## 1.4 sleep()方法
>在指定毫秒内让当前“正在执行的线程”休眠（暂停执行），“正在执行的线程”指的是this.currentThread()返回的线程

## 1.5 getId()方法
>取得线程的唯一标识

## 1.6 停止线程
### 1.6.1 三种方法
1.使用退出标志，使线程正常退出，也就是当run方法完成后线程终止。
2.使用stop方法强行终止线程（已过期，不安全）
3.使用interrupt方法中断线程
### 1.6.2 
