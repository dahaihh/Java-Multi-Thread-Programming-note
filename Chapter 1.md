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
