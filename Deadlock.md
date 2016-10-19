#Deadlock#
##产生死锁的实验过程##
1. 在window中安装java（version:1.8.0_111)，并按照[此处](http://jingyan.baidu.com/article/d169e186afd233436611d828.html "此处")配置环境变量。

	**需要注意的是系统变量path和classpath都需要在前面加上“.;”**

	![figure1](http://i.imgur.com/RwpckR2.png)

2. 运行cmd，进入Deadlock.java目录下，运行指令`javac Deadlock.java`

3. 在同目录下运行Deadlock.bat批处理文件，观察到的结果如下图

	![figure2](http://i.imgur.com/HPUNZ8i.png)

	可以看到程序在跑了23次就停下来了。

##死锁##
**死锁（Deadlock）**指的是进程死锁，是个计算机技术名词。它是操作系统或软件运行的一种状态：在多任务系统下，当一个或多个进程等待系统资源，而资源又被进程本身或其他进程占用时，就形成了死锁。由于资源占用是互斥的，当某个进程提出资源申请后，使得有关进程在无外力协助下，永远分配不到必需的资源而无法继续运行。

死锁产生的4个必要条件如下：
1. 互斥条件：一个资源每次只能被一个进程使用。
2. 请求与保持条件：一个进程因请求资源而阻塞时，对已获得的资源保持不放。
3. 不剥夺条件：进程已获得的资源，在末使用完之前，不能强行剥夺。
4. 循环等待条件：若干进程之间形成一种头尾相接的循环等待资源关系。
只要上述条件一个不满足即不会产生死锁。

##对上述程序产生死锁的解释##
Deadlock.java代码如下

	class A {
		synchronized void methodA(B b) {
			b.last();
		}
	
		synchronized void last() {
			System.out.println("Inside A.last()");
		}
	}
	class B {
		synchronized void methodB(A a) {
			a.last();
		}
		synchronized void last() {
			System.out.println("Inside B.last()");
		}
	}

	class Deadlock implements Runnable{
		A a=new A();
		B b=new B();

		Deadlock(){
			Thread t = new Thread(this);
			int count = 10000;	
			t.start();
			while(count-->0);
			a.methodA(b);
		}
		public void run(){
			b.methodB(a);
		}
		public static void main(String args[]){
			new Deadlock();
		}
	}

1. 首先对于Class A以及B内：关键字synchronized——当它用来修饰一个方法或者一个代码块的时候，能够保证在同一时刻最多只有一个线程执行该段代码。也就是说每个资源只能由一个进程使用，是**互斥**的。
2. 当一个线程访问object的一个synchronized同步代码块或同步方法时，其他线程对object中所有其它synchronized同步代码块或同步方法的访问将被阻塞。也就是说这些资源是**不可抢占**的。
3. 另外，Class A在methodA中申请占有一个B类实例，同时，Class B也在methodB中申请占有一个A类实例。
4. Runnable指明了是多线程模式。
4. 进程运行class Deadlock时，执行main函数，定义一个新的Deadlock类，执行构造函数Deadlock()，先是定义一个类A对象和一个类B对象，并分配空间，随后定义一个子线程，让主线程持续空运行一段时间（因为子线程只是被扔进队列中），要注意的是t.start()时，会运行run()函数，占有b，申请a；同时间，主进程继续执行，占有a并申请b。其时间轴如下图：

	![figure3](http://i.imgur.com/u8VvzqY.png)

5. 不断进行这样的过程，可能会有这样情况：a.methodA(b) 和 b.methodB(a) 被两个进程同时执行，导致阻塞形成死锁。原因是，一个线程占有了a，而想申请b；而另一个线程占有了b，向申请a。**它们占有了各自的资源，而被对方占有的资源阻塞**。**从而形成相互等待对方资源释放的局面**。这满足了上述的产生死锁的四个必要条件，产生了死锁。
