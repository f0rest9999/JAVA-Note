#Java多线程（Thread）

## 线程简介

- 程序：是指令和数据的有序集合，本身没有运行的含义，是一个静态的概念

- 进程：运行的程序，执行程序的一次执行过程，它是一个动态过程，是系统分配资源的单位

  ​			进程里面有一个或者多个线程（比如视频中同时有声音、图像、字幕）

- 线程：是CPU调度和执行的单位

- 多线程：类比于生活中，扩宽了道路，通过开辟多条道路，解决了之前只有一条道路时的阻塞问题（多人游戏等等）

![image-20201210102143830](https://gitee.com/f0rest9999/images/raw/master/20201210102150.png)

![image-20201210102519461](https://gitee.com/f0rest9999/images/raw/master/20201210102519.png)

###核心概念

![image-20201210102833896](https://gitee.com/f0rest9999/images/raw/master/20201210102834.png)

## 线程实现（重点）

###线程的创建

创建的三种方式

- 继承Thread类（重点）

- 实现Runnable接口（重点）

- 实现Callable接口（了解即可）

### Thread类

![image-20201210104658626](https://gitee.com/f0rest9999/images/raw/master/20201210104658.png)

```java

//创建线程方式一:继承Thread类，重写Run()方法，调用start开启线程

//注意：线程开启b不一定立即执行，由CPU调度执行
public class TestThread extends Thread{

    @Override
    public void run() {
        for(int i = 0;i < 200;i++){
            System.out.println("线程 " + i);
        }
    }

    //主线程
    public static void main(String[] args) {
        //创建一个线程对象，调用start方法
        TestThread thread = new TestThread();
        thread.start();

        for(int i = 0;i < 2000;i++){
            System.out.println("主线程 " + i);
        }
    }
}

```

![image-20201210105653525](https://gitee.com/f0rest9999/images/raw/master/20201210105653.png)

==注意用start（）方法开启线程之后是交替执行的，用run（）方法则不是==

### 实现Runnable接口

![image-20201210135932620](https://gitee.com/f0rest9999/images/raw/master/20201210135932.png)

```java
//创建线程方式2：实现Runnable接口，重写Run方法，执行线程需要丢入Runnable接口实现类，调用start方法
public class TestThread2 implements Runnable{

    @Override
    public void run() {
        for(int i = 0;i < 200;i++){
            System.out.println("线程 " + i);
        }
    }

    //主线程
    public static void main(String[] args) {

        //创建Runnable接口的实现类对象
        TestThread2 thread = new TestThread2();

        //创建线程对象，通过线程对象来开启我们的线程，代理
//        Thread thread1 = new Thread(thread);
//        thread1.start();

        //总结为这句话
        new Thread(thread).start();

        for(int i = 0;i < 2000;i++){
            System.out.println("主线程 " + i);
        }
    }

```

### 小结

![image-20201210141136752](https://gitee.com/f0rest9999/images/raw/master/20201210141136.png)

### 实现Runnable接口的好处

- **避免了单继承性，灵活方便，方便同一个对象被多个线程使用**

- 几个线程操作同一个资源所带来的问题以及Runnable接口的练习

  ```java
  //多个线程操作同一个对象，买火车票的例子
  //多个线程操作同一个资源的情况下，线程不安全，数据紊乱
  public class TestThread4 implements Runnable{
      private int tickets = 10;
  
      @Override
      public void run() {
          while(true){
              if(tickets <= 0){
                  break;
              }
              //模拟延时
              try {
                  Thread.sleep(200);
              } catch (InterruptedException e) {
                  e.printStackTrace();
              }
              System.out.println(Thread.currentThread().getName() + "拿到了第" + tickets-- + "张票");
          }
      }
  
      public static void main(String[] args) {
          TestThread4 ticket = new TestThread4();
          new Thread(ticket, "小明").start();
          new Thread(ticket, "老师").start();
          new Thread(ticket, "黄牛").start();
      }
  }
  ```

  结果：可以发现明显的错误，引发了我们对线程同步的探索

  ```
  小明拿到了第10张票
  黄牛拿到了第8张票
  老师拿到了第9张票
  老师拿到了第7张票
  黄牛拿到了第7张票
  小明拿到了第7张票
  黄牛拿到了第5张票
  老师拿到了第6张票
  小明拿到了第5张票
  老师拿到了第4张票
  黄牛拿到了第4张票
  小明拿到了第4张票
  黄牛拿到了第3张票
  小明拿到了第3张票
  老师拿到了第2张票
  老师拿到了第1张票
  小明拿到了第1张票
  黄牛拿到了第1张票
  ```

### 案例：龟兔赛跑

```java
import java.sql.SQLOutput;
import java.util.RandomAccess;

public class TestThread5 implements Runnable{

    private static String winner;

    @Override
    public void run() {
        for (int i = 0; i <= 100; i++) {

            if(Thread.currentThread().getName().equals("兔子") && i % 10 == 0){
                try {
                    Thread.sleep(5);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }

            boolean flag = gameOver(i);
            if(flag)
                break;
            System.out.println(Thread.currentThread().getName() + "跑了" + i + "步");

        }
    }

    public boolean gameOver(int steps){
        if(winner != null)
            return true;
        if(steps >= 100){
            winner = Thread.currentThread().getName();
            System.out.println("winner is " + winner);
            return true;
        }
        return false;
    }

    public static void main(String[] args) {
        TestThread5 race = new TestThread5();

        new Thread(race, "兔子").start();
        new Thread(race, "乌龟").start();

    }
}
```

### 实现Callable接口（扩充）

![image-20201210144339614](https://gitee.com/f0rest9999/images/raw/master/20201210144339.png)

- 优点：可以定义返回值、可以抛出异常
- 缺点：稍微繁琐一点

### 静态代理模式

- 案例：结婚、婚庆公司

```java

//静态代理模式总结
//真实对象和代理对象都实现同一个接口
//代理对象要代理真实对象

public class StaticProxy {
    public static void main(String[] args) {
        //正常的流程 你要结婚
        You you = new You();

        //好处：代理对象可以做很多真实对象做不了的事情
        //    真实对象专注做自己的事情
        WeddingCompany weddingCompany = new WeddingCompany(you);

        weddingCompany.happyMarry();
    }
}

interface  Marry{
    void happyMarry();
}

//真实角色，你去结婚
class You implements Marry{

    @Override
    public void happyMarry() {
        System.out.println("结婚了");
    }
}
//代理角色，帮助你结婚
class WeddingCompany implements Marry{

    //代理谁，即真实目标角色
    private Marry target;

    public WeddingCompany(Marry target) {
        this.target = target;
    }

    @Override
    public void happyMarry() {
        before();
        this.target.happyMarry();
        after();
    }
    private void before(){
        System.out.println("布置场景");
    }
    private void after(){
        System.out.println("收尾款");
    }
}
```

- 静态代理和线程的相似之处

  ```java
  	//WeddingCompany weddingCompany = new WeddingCompany(you);
  	//weddingCompany.happyMarry();
  	new WeddingCompany(new You()).happyMarry();
  	new Thread(() -> System.out.println("我")).start();
  ```

## 线程状态

###五大状态

![image-20201210163551857](https://gitee.com/f0rest9999/images/raw/master/20201210163551.png)

- ![image-20201210163622086](https://gitee.com/f0rest9999/images/raw/master/20201210163622.png)

###线程停止

- 线程自己停下来是最安全的
- 建议线程正常停止——>利用次数，防止死循环
- 建议使用标志位——>设置一个标志位
- 不要使用stop或者destory等过时的方法

![image-20201210164001712](https://gitee.com/f0rest9999/images/raw/master/20201210164001.png)

```java
public class TestStop implements Runnable{

    private boolean flag = true;

    @Override
    public void run() {
        int i = 0;
        while(flag) {
            System.out.println("Running.." + i--);
        }
    }
    public void mystop(){
        this.flag = false;
    }

    public static void main(String[] args) {
        TestStop testStop = new TestStop();
        new Thread(testStop).start();

        for (int i = 0; i < 1000; i++) {
            System.out.println("main" + i);
            if(i == 900){
                testStop.mystop();
                System.out.println("我说停停");
            }

        }
    }
}
```



![image-20201210165404525](https://gitee.com/f0rest9999/images/raw/master/20201210165404.png)

###线程休眠

- ==每个对象都有一个锁，sleep不会释放锁==
- 模拟网络延时：放大问题的发生性
- 模拟倒计时

![image-20201210181018219](https://gitee.com/f0rest9999/images/raw/master/20201210181018.png)

###线程礼让

- **礼让不一定成功，还要看CPU的具体调度**

![](https://gitee.com/f0rest9999/images/raw/master/20201210181906.png)

###线程Join（强制执行）

```java
//即主线程在100时，thread线程开始插队，且该插队进程执行完成之后别的进程才能执行
public class TestJoin implements Runnable{

    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.println("我是VIP" + i);
        }
    }

    public static void main(String[] args) throws InterruptedException {
        TestJoin testJoin = new TestJoin();
        Thread thread = new Thread(testJoin);
        thread.start();

        for (int i = 0; i < 500; i++) {
            if(i == 100) {
                thread.join();
            }
            System.out.println("我是主线程" + i);
        }

    }
}

```

![image-20201210183448549](https://gitee.com/f0rest9999/images/raw/master/20201210183448.png)

### 观测线程状态

- 线程已经退出之后就不能在start了

![image-20201210185347370](https://gitee.com/f0rest9999/images/raw/master/20201210185347.png)

```java
public class TestState {
    public static void main(String[] args) throws InterruptedException {
        Thread thread = new Thread(()-> {
            for (int i = 0; i < 5; i++) {
                try {
                    Thread.sleep(1000);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("OVER");
        });

        //观察目前状态
        Thread.State state = thread.getState();
        System.out.println(state);//NEW

        //启动后
        thread.start();
        state = thread.getState();
        System.out.println(state);//RUN
        //每0.5s捕捉一次上thread线程的状态
        while(state != Thread.State.TERMINATED){
            Thread.sleep(500);
            state = thread.getState();
            System.out.println(state);
        }
    }
}
```

![image-20201210191139745](https://gitee.com/f0rest9999/images/raw/master/20201210191139.png)

### 线程优先级

- 先设置优先级，再启动
- 不一定优先级高的就先跑，但是大多数情况下，是这样的
- 优先级低只是意味着获得调度的概率低，并不是优先级低就不会调用了，这都看CPU的调度
- 如果觉得这个线程重要，那么就给它设置高一些的优先级

![image-20201211082635076](https://gitee.com/f0rest9999/images/raw/master/20201211082642.png)

### 守护线程（daemon）

![image-20201211083443622](https://gitee.com/f0rest9999/images/raw/master/20201211083443.png)

- 测试守护线程

  ```java
  public class TestD {
      public static void main(String[] args) {
          God god = new God();
          Person person = new Person();
          Thread thread = new Thread(god);
          thread.setDaemon(true);//默认是false，用户线程
          thread.start();//启动守护线程
  
          new Thread(person).start();//用户线程启动
      }
  
  }
  class  God implements Runnable{
  
      @Override
      public void run() {
          while(true){
              System.out.println("God bless you");
          }
      }
  }
  class Person implements Runnable{
  
      @Override
      public void run() {
          for(int i = 0;i < 100;i++){
              System.out.println("一年都开心");
          }
          System.out.println("GoodeBye, World");
      }
  }
  ```

  程序正常结束了：即jvm不用等待守护线程执行完毕再结束

###线程常用方法

![image-20201210163829623](https://gitee.com/f0rest9999/images/raw/master/20201210163829.png)

## 线程同步（重点）（synchronized）

- **并发：同一个对象被多个线程同时操作**
- **线程同步其实就是一种等待机制，多个需要同时访问此对象的线程进入这个对象的等待池形成队列，等待前面的线程使用完毕，下一个线程再使用**
- **光有队列是不够的，还要有锁，每个对象都有一把锁**
- **队列 + 锁才能解决安全性，但是要是保证安全，就会损失一定的性能**

### 三个不安全的案例

- 买票，线程不安全
- **每个线程在自己的工作内存进行交互，内存控制不当会造成数据不一致**

```java
public class TestTicket {
    public static void main(String[] args) {
        BuyTicket buyTicket = new BuyTicket();

        new Thread(buyTicket, "我").start();
        new Thread(buyTicket, "你").start();
        new Thread(buyTicket, "他").start();
    }
}

class BuyTicket implements Runnable{

    private int ticketsNum = 10;
    private boolean flag = true;

    @Override
    public void run() {
        while(flag){
            try {
                buy();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }
    }

    private void buy() throws InterruptedException {
        if(ticketsNum <= 0){
            flag = false;
            return ;
        }
        Thread.sleep(100);
        System.out.println(Thread.currentThread().getName() + " 拿到了第" +ticketsNum-- + "票");
    }
}
```

![image-20201211124857044](https://gitee.com/f0rest9999/images/raw/master/20201211124857.png)

- 不安全的取钱，两个人同时去银行取同一张卡的钱

```java
import sun.awt.windows.ThemeReader;

public class TestBank {
    public static void main(String[] args) throws InterruptedException {
        Account account = new Account(100, "卡");

        Drawing me = new Drawing(account, 50, "我取");
        Drawing you = new Drawing(account, 80, "你取");

        me.start();
        you.start();
        Thread.sleep(4000);
        System.out.println("最后账户剩" + account.money);
    }
}

class Account{
    int money;//余额
    String name;//卡名

    public Account(int money, String name) {
        this.money = money;
        this.name = name;
    }
}

//银行：模拟取款
class Drawing extends Thread{
    Account account;

    private int drawingMoney;//取钱

    private int nowMoney;//现在手里有多少钱

    public Drawing(Account account, int drawingMoney, String name) {
        super(name);
        this.drawingMoney = drawingMoney;
        this.account = account;
    }

    @Override
    public void run() {
        if(account.money - drawingMoney < 0){
            System.out.println(Thread.currentThread().getName() + " 钱不够，不给取");
            return;
        }
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }

        account.money = account.money - drawingMoney;
        nowMoney = nowMoney + drawingMoney;

        System.out.println(this.getName() + " 手里的钱" + nowMoney);
        try {
            Thread.sleep(500);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(account.name + " 余额为" + account.money);
    }
}
```

![image-20201211095940041](https://gitee.com/f0rest9999/images/raw/master/20201211095940.png)

- 线程不安全的集合

  在同一个瞬间，添加时发生了覆盖

```java
import java.util.ArrayList;
import java.util.List;

public class UnsafeList {

    public static void main(String[] args) {
        List<String> list = new ArrayList<>();

        for (int i = 0; i < 10000; i++) {
            new Thread(() -> {
               list.add(Thread.currentThread().getName());
            }).start();
        }
        System.out.println(list.size());
    }
}
```

![image-20201211100313436](https://gitee.com/f0rest9999/images/raw/master/20201211100313.png)



### synchronized同步

- **对于某一个特定的对象来说，它的所有的synchronized方法共享同一个锁，我们把临界资源标记为synchronized之后，如果某一个任务处于一个对标记为synchronized的方法的调用中，那么在这个线程从该方法返回之前，其他所有的调用类中任何标记为synchronized的方法的线程都会被阻塞**
- synchronized也可以来修饰class
- ==**Brian同步规则：如果你正在写一个变量，它接下来可能被另一个线程读取，或者正在读取一个上一次已经被另一个线程写过的变量，那么你必须使用同步，并且，读写线程都必须用相同的监视器锁同步**==

- **同步方法，说白了，synchronized也是个修饰符**
- **一种是同步方法，一种是同步块**
- **方法里面需要修改的内容才需要锁，锁的太多了，浪费资源**

![image-20201211103053415](https://gitee.com/f0rest9999/images/raw/master/20201211103053.png)

![image-20201211124141903](https://gitee.com/f0rest9999/images/raw/master/20201211124149.png)

- ==锁的对象就是互斥的那个变化的量==

```java
//同步块演示 
@Override
    public void run() {
        
        //锁的对象就是互斥的那个变化的量
        synchronized (account){
            if(account.money - drawingMoney < 0){
                System.out.println(Thread.currentThread().getName() + " 钱不够，不给取");
                return;
            }
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }

            account.money = account.money - drawingMoney;
            nowMoney = nowMoney + drawingMoney;

            System.out.println(this.getName() + " 手里的钱" + nowMoney);
            try {
                Thread.sleep(500);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println(account.name + " 余额为" + account.money);
        }
    }
```

```
for (int i = 0; i < 10000; i++) {
        new Thread(() -> {
        synchronized(list){
        	list.add(Thread.currentThread().getName());
        }
	}).start();
}
```

- JUC包下的一个使用例子（了解即可）
- ![image-20201211190330318](https://gitee.com/f0rest9999/images/raw/master/20201211190330.png)

### 死锁

- 概念：某个任务在等待另一个任务，而后者又等待别的任务，这样一直下去，直到这个链条上的任务又在等待第一个任务释放锁，**这得到了一个任务之间相互等待的连续循环**，没有哪个线程能继续，这被称之为死锁。
- 一个死锁的例子

```java
public class DeadLock {

    public static void main(String[] args) {
        GetOne p1 = new GetOne(0, "小明");
        GetOne p2 = new GetOne(1, "小红");
        p1.start();
        p2.start();
    }
}

class Water{

}
class Food{

}

class GetOne extends Thread{

    //保证只有一份
    static Water water = new Water();
    static Food food = new Food();

    int choice;
    String name;

    GetOne(int choice, String name){
        this.choice = choice;
        this.name = name;
    }

    @Override
    public void run() {
        try {
            getOne();
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }

    private void getOne() throws InterruptedException {
        if(choice == 0){
            synchronized (water){
                System.out.println(this.name + "获得了水");
                Thread.sleep(1000);
                synchronized (food){
                    System.out.println(this.name + "获得了食物");
                }
            }
        } else{
            synchronized (food){
                System.out.println(this.name + "获得了食物");
                Thread.sleep(2000);
                synchronized (water){
                    System.out.println(this.name + "获得了水");
                }
            }
        }
    }
}
```

![image-20201211192508471](https://gitee.com/f0rest9999/images/raw/master/20201211192508.png)



### Lock锁同步

- **通过显式地定义同步锁来实现同步**

- **ReentrantLock类实现了Lock，且比较常用，显示地加锁和释放锁**（name：可重入锁）

- 使用方法

  ![image-20201211194611729](https://gitee.com/f0rest9999/images/raw/master/20201211194611.png)

- 两者的区别

  ![image-20201211194920733](https://gitee.com/f0rest9999/images/raw/master/20201211194920.png)

## 线程协作与通信

### 生产者消费者问题

- 问题概述

  ![image-20201211195331021](https://gitee.com/f0rest9999/images/raw/master/20201211195331.png)

- 问题分析

  ![image-20201211195620669](https://gitee.com/f0rest9999/images/raw/master/20201211195620.png)

- **Java提供了几个方法来进行线程通信**

- 生产者生产，消费者消费，两者之间有通信

- wait 和 sleep的区别就是wait会释放锁

  ![image-20201211201424603](https://gitee.com/f0rest9999/images/raw/master/20201211201424.png)

### 解决生产者消费者--管程法

![image-20201211202150726](https://gitee.com/f0rest9999/images/raw/master/20201211202150.png)

==！！！！！这个程序写的不咋好！！！！！==

```java
//测试生产者消费者模型---利用管程法----缓冲区

//生产者、消费者、缓冲区、产品
public class TestPC {
    public static void main(String[] args) {
        Container container = new Container();

        new Productor(container).start();
        new Consumer(container).start();
    }
}

//生产者需要一个容器
class Productor extends Thread{
    Container container;

    public Productor(Container container){
        this.container = container;
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            container.push(new Chicken(i));
            System.out.println("生产了第" + i + " 只鸡");
        }
    }
}
//消费者也需要一个容器
class Consumer extends Thread{
    Container container;

    public Consumer(Container container){
        this.container = container;
    }

    @Override
    public void run() {
        for (int i = 0; i < 100; i++) {
            System.out.println("消费了第" + container.pop().id + "只鸡");
        }
    }
}

class Chicken{
    int id;

    public Chicken(int id){
        this.id = id;
    }
}

class Container{

    //容器大小
    Chicken[] chickens = new Chicken[10];
    int count = 0;
    //生产者放入产品
    public synchronized void push(Chicken chicken){
        //如果容器满了就等待消费者消费
        if(count >= chickens.length){
            //通知消费者消费，生产者进行等待
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //如果没有满，我们就丢入产品
        chickens[count] = chicken;
        count ++;

        //可以通知消费者消费了
        this.notifyAll();
        return ;
    }

    //消费者消费产品
    public synchronized Chicken pop(){
        if(count == 0){
            //等待生产者生产
            try {
                this.wait();
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        }

        //如果可以消费
        count --;
        Chicken chicken = chickens[count];

        //吃完了，通知生产者生产
        this.notifyAll();
        return chicken;
    }


}
```

### 解决生产者消费者--信号灯法

- 设立一个flag进行通信

- 实质上就是cache长度为1的管程法（我是这么认为的==）
- 一个只管生产，一个只管消费，但是需要wait和notify进行等待和通知唤醒

## 线程池

![image-20201211210418668](https://gitee.com/f0rest9999/images/raw/master/20201211210418.png)

![image-20201211210628901](https://gitee.com/f0rest9999/images/raw/master/20201211210628.png)

 ```java
import java.util.concurrent.Executor;
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class TestPool {

    public static void main(String[] args) {
        //1.创建线程池
        //参数是池子大小
        ExecutorService executorService = Executors.newFixedThreadPool(10);

        executorService.execute(new MyThread());
        executorService.execute(new MyThread());
        executorService.execute(new MyThread());
        executorService.execute(new MyThread());
        executorService.execute(new MyThread());

        //2.关闭连接
        executorService.shutdown();
        
    }

}

class MyThread implements Runnable{
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName());
    }

}
 ```

![image-20201211211135894](https://gitee.com/f0rest9999/images/raw/master/20201211211135.png)



## Lamda表达式（补充知识）

- 其实质属于**函数式编程**的概念

![image-20201210154445605](https://gitee.com/f0rest9999/images/raw/master/20201210154445.png)

![image-20201210154517605](https://gitee.com/f0rest9999/images/raw/master/20201210154517.png)

- **看习惯就好了**

- **函数式接口的定义【注意 是唯一 一个 抽象  方法】**

  ![image-20201210154713539](https://gitee.com/f0rest9999/images/raw/master/20201210154713.png)

- 12 - > 3 - > 4 - > 5 - > 6

  ```java
  //推导以下Lambda表达式的变化过程
  public class TestLambda {
  
      //3.静态内部类
      static class Like2 implements ILike{
  
          @Override
          public void f() {
              System.out.println("I like 2");
          }
      }
  
      public static void main(String[] args) {
          ILike like = new Like();
          like.f();
  
          like = new Like2();
          like.f();
  
          //4.局部内部类
          class Like3 implements ILike{
  
              @Override
              public void f() {
                  System.out.println("I like 3");
              }
          }
  
          like = new Like3();
          like.f();
  
          //5.匿名内部类  没有类的名字,必须借助接口或者是父类
          like = new ILike() {
              @Override
              public void f() {
                  System.out.println("I like 4");
              }
          };
          like.f();
  
          //6.用lambda简化
          like = ()->{
              System.out.println("I like 5");
          };
          like.f();
      }
  
  }
  
  //1.定义一个函数式接口
  interface ILike{
      void f();
  }
  
  interface ILove{
      void ff(int k);
  }
  
  //2.实现类
  class Like implements ILike{
  
      @Override
      public void f() {
          System.out.println("I like 1");
      }
  }
  ```

- 带参数类型的【函数式接口】以及几种可能的简化方式

  ```java
  public class TestLambda {
  	ILove iLove = (int a)->{
              System.out.println("I love " + a);
          };
  	//再简化，多个参数也可以去掉参数类型，但是要去掉都去掉
     	iLove = (a)->{
          System.out.println("I love " + a);
      };
  	//再简化，因为只有一个参数，如果有多个则不能用
      iLove = a->{
          System.out.println("I love " + a);
      };
      //再简化，去掉花括号，因为方法只有一行，如果有多行，则不能用
      iLove = a-> System.out.println("I love " + a);
      
      iLove.ff(2);
  }
  
  interface ILove{
      void ff(int k);
  }
  ```

  ![image-20201210161242139](https://gitee.com/f0rest9999/images/raw/master/20201210161242.png)

- **Runnable接口就是一个函数式接口** ---> 因此可以用lambda表达式简化