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
