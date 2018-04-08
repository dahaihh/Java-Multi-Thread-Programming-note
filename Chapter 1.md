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
## 1.1.3 实例变量和线程安全
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
>synchronized关键字可以使多个线程在执行run方法时，进行排队操作
>实现原理：当一个线程在调用run方法前，先判断run方法有没有上锁，如果上锁证明有线程在调用run方法，则必须等其他线程调用结束后才可以调用run方法
>synchronized关键字可以在任意对象或方法上加锁，加锁的这段代码称为“互斥区”或“临界区”
