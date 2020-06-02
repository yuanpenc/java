# java

## 1. stringbuffer,stringbuilder

## 2. override equals and hashCode
```package equal;
import java.util.Objects;
public class prove {
	int age;
	String name;
	public prove(int a, String n) {
		this.age =a;
		this .name=n;
	}
	public boolean equals(Object b){
		if (b==null) {
			return false;
		}
		if (this==b) {
			return true;
		}
		if (!(b instanceof prove) ) {
			System.out.print("yes");
			return false;
		}
		prove person=(prove) b;
		
		return (age==person.age)&&(name==person.name);
	}
	@Override
	public int hashCode() {
		return Objects.hash(age,name);
	}	
	public static void main(String[] args) {
		prove person1=new prove(15,"jack");
		prove person2=new prove(15,"jack");
		System.out.println(person1.equals(person1));
		System.out.println(person1.hashCode());
		System.out.println(person2.hashCode());
	}
}
```
## 3. ways to get input from keyboard
method1: Scanner

```java
Scanner input = new Scanner(System.in);
String s  = input.nextLine();
input.close();
```

mathod2: BufferedReader 

```java
BufferedReader input = new BufferedReader(new InputStreamReader(System.in)); 
String s = input.readLine(); 
```

## 4. 异常
### 引入与分类
#### try - catch - finally
* try-catch
  ```
  try{
  codes that probably causes crash;
  }catch(Exception ex){ // Exception ex => polymorphism.
  code for modification or reminding;
  
  ex.printStackTrace();//show all the process to trace the crash.
  throw ex; //throw exception to caller. If caller is main function, then this command will throw ex to JVM and trigle two  effects: (1) print infomation of exception. (2) stop the program.
  }
  ```
 
* try-catch-finally
1. once using finally, the code in finally part would run eventully(except for System.exit(status:1)). No matter what are in try or catch part, no matter the program is crashed or not.
2. In finally, we normally deal with shuting down database resource, IO flow and socket. 
3. return commmand will not conflict with finally. Finally runs before return. 
```
try{
  codes that probably causes crash;
  }catch(ArithmaticException ex){ 
  code for modification or reminding.
  ex.printStackTrace();//show all the process to trace the crash.
  }catch(InputMismatchException ex){
  //some code.
  }catch(Exception ex){
  //put in the last place.
  }finally{
  //some code would definitely run by program normally.
  }
```
#### Exception Category
<p align="center">
  <img src="./pic/Screen Shot 2020-02-23 at 9.31.13 PM.png" width="400">
</p>


* Runtime Exception 

ArrayIndexOutOfBounds... NullPointer... Arithmatic... etc.

* compile Exception

```
Class.forname("Text").newInstance();
//Where has two risks: no Text class or instance exception.
```


WAY TO HANDLE THIS SITUATION
1. Utilize try-catch to capture the Exception
2. Throws Exception

```
Public Static void main(String[] args) throws Exception{

....
throw new Exception("oops");
....
}
or 

Public Static void main(String[] args){

....
throw new RuntimeException("oops");
....


}

```

#### The difference  between throw and throws

1. 方法内部 vs 方法声明处
2. throw 异常对象 vs throws 异常类型
3. throw 异常出现的源头 vs throws 向caller抛出异常，让caller去处理。

## 5. IO Stream
### 字节流
read one byte once， 不能很好的表示汉字，会变成乱码。
```
public class Test {
	public void main (String arg[]) throws IOException{
		File file= new File(pathname: "/user/....");
		FileInputStream fis= new FileInputStream(file);
		int n = fis.read();
		while(n!=-1){
			System.out.println(n);
			n=fis,read();
		}
		fis.close();
	}
}
```
提高效率的方法，改变每次读入对字节，同时返回每次读入对字节数。利用缓存数组。
```
byte[] b=new byte[];
int len=fis.read(b);
while(len!=-1){
len=fis.read(b);
}
```
以上是读取文件，接下来是写入文件。
```
...
File file= new File(pathname: "/user/d.txt");
FileOutputstream fos=new FileOutputStream(file); 
String str="dafhkahdfkhakdfhak";
byte[] b= str.getBytes();
fos.write();
fos.close();


```
## 6. 并发
### 线程初始化状态


### 如果一次性插入100条数据怎么办？不用for循环这种形式
减少连接资源，拼接一条sql
```
$sql    = substr($sql ,0 ,-1);
//拼接之后大概就是 INSERT INTO tablename ('username','password') values
('xxx','xxx'),('xxx','xxx'),('xxx','xxx'),('xxx','xxx'),('xxx','xxx'),('xxx','xxx')
.......



```

### 进程和线程
1.进程与线程

进程：具有独立功能的程序关于某个数据集合上的一次运行活动。
线程：进程的一个实体。
比喻：一列火车是一个进程，火车的每一节车厢是线程。

2.进程与线程的联系

①一个线程只能属于一个进程，一个进程可以有多个线程；
②系统资源分配给进程，同一进程的所有线程共享该进程的所有资源；
③真正在处理机上运行的是线程；
④不同进程的线程间利用消息通信的方式实现同步。

3.进程与线程的区别

①调度：线程是系统调度和分配的基本单位，进程是作为拥有系统资源的基本单位；
②并发性：进程之间可以并发执行，同一进程的多个线程时间亦可以并发执行；
③拥有资源：进程是拥有资源的独立单位，线程不拥有资源，但可以访问隶属于进程的资源；
④系统开销：创建和撤销进程的开销更大；进程拥有独立的地址空间，一个进程的崩溃不会影响其他进程；线程拥有自己的堆栈和局部变量，没有独立的地址空间，因此进程里的一个线程崩溃会导致其他线程均崩溃。

4.进程间通信方式

进程间通信方式有很多，网上一说有十几种。面试的时候说上以下几种差不多：
①管道：半双工的通信方式，数据只能单向流动，且只能在有亲缘关系（父子进程或兄弟进程）的进程间使用；
②命名管道：FIFO，半双工的通信方式，但允许在无亲缘关系的进程间通信；
③消息队列：消息的链表，存放在内核中，并由消息队列标识符标识。消息队列克服了信号传递信息少、管道只能承载无格式字节流以及缓冲区大小受限等缺点；
④信号量：是一个计数器，用于控制多个进程间对共享资源的访问；
⑤共享内存：映射一段能被其他进程访问的内存，这段内存由一个进程创建，但多个进程都可以访问；
⑥套接字
Linux网络编程——Unix本地套接字：https://blog.csdn.net/lu_embedded/article/details/78352230


5.线程间通信方式

多个线程在处理同一个资源，并且任务不同时，需要线程通信来帮助解决线程之间对同一个变量的使用或操作。就是多个线程在操作同一份数据时， 避免对同一共享变量的争夺。
①锁机制：包括互斥锁、条件变量、读写锁

互斥锁提供了以排他方式防止数据结构被并发修改的方法。
读写锁允许多个线程同时读共享数据，而对写操作是互斥的。
条件变量可以以原子的方式阻塞进程，直到某个特定条件为真为止。对条件的测试是在互斥锁的保护下进行的。条件变量始终与互斥锁一起使用。
②信号量机制(Semaphore)：包括无名线程信号量和命名线程信号量
③信号机制(Signal)：类似进程间的信号处理
线程间的通信目的主要是用于线程同步，所以线程没有像进程通信中的用于数据交换的通信机制。

原文链接：https://blog.csdn.net/sinat_21107433/article/details/82809946

### 网络协议模型
作者：Z_萧晓
链接：https://www.jianshu.com/p/cfac5c131a9b

应用层：负责处理特定的应用程序细节（表示、会话）
HTTP、FTP、DNS

传输层：为两台主机提供端到端的基础通信
TCP、UDP

网络层：控制分组传输、路由选择等
IP

链路层：操作系统设备驱动程序、网卡相关接口

物理层：比特流传输

协议具体：https://blog.csdn.net/m0_37925202/article/details/80293925

### TCP 和 UDP 区别

TCP 连接；可靠；有序；面向字节流；速度慢；较重量；全双工；适用于文件传输、浏览器等


1、TCP面向连接（如打电话要先拨号建立连接）;UDP是无连接的，即发送数据之前不需要建立连接

2、TCP提供可靠的服务。也就是说，通过TCP连接传送的数据，无差错，不丢失，不重复，且按序到达;UDP尽最大努力交付，即不保证可靠交付

Tcp通过校验和，重传控制，序号标识，滑动窗口、确认应答实现可靠传输。如丢包时的重发控制，还可以对次序乱掉的分包进行顺序控制。

3、UDP具有较好的实时性，工作效率比TCP高，适用于对高速传输和实时性有较高的通信或广播通信。

4.每一条TCP连接只能是点到点的;UDP支持一对一，一对多，多对一和多对多的交互通信

5、TCP对系统资源要求较多，UDP对系统资源要求较少。

具体区别：https://blog.csdn.net/qq_34624951/article/details/82669444


### TCP 三次握手

A：你能听到吗？
B：我能听到，你能听到吗？
A：我能听到，开始吧

A 和 B 两方都要能确保：我说的话，你能听到；你说的话，我能听到。所以需要三次握手

### TCP 四次挥手

A：我说完了
B：我知道了，等一下，我可能还没说完
B：我也说完了
A：我知道了，结束吧

B 收到 A 结束的消息后 B 可能还没说完，没法立即回复结束标示，只能等说完后再告诉 A ：我说完了。

### POST 和 GET 区别

Get 参数放在 url 中；Post 参数放在 request Body 中
Get 可能不安全，因为参数放在 url 中

### HTTPS

HTTP 是超文本传输协议，明文传输；HTTPS 使用 SSL 协议对 HTTP 传输数据进行了加密

HTTP 默认 80 端口；HTTPS 默认 443 端口

优点：安全
缺点：费时、SSL 证书收费，加密能力还是有限的，但是比 HTTP 强多了


### StringBuilder、StringBuffer、+、String.concat 链接字符串：

StringBuffer 线程安全，StringBuilder 线程不安全
+实际上是用 StringBuilder 来实现的，所以非循环体可以直接用 +，循环体不行，因为会频繁创建 StringBuilder
String.concat 实质是 new String ，效率也低，耗时排序：StringBuilder < StringBuffer < concat < +

### Java 泛型擦除

修饰成员变量等类结构相关的泛型不会被擦除
容器类泛型会被擦除

### ArrayList、LinkedList

ArrayList

基于数组实现，查找快：o(1)，增删慢：o(n)
初始容量为10，扩容通过 System.arrayCopy 方法

LinkedList

基于双向链表实现，查找慢：o(n)，增删快：o(1)
封装了队列和栈的调用






