---

layout: default
title: Java 堆异常 和 栈异常
abstract: 
comments: false

---

```java

package chang.hu.t2;

import java.io.IOException;
import java.util.ArrayList;
import java.util.Hashtable;
import java.util.List;

public class Q01 {

	static int count = 1;
	
	//循环创建较大的对象直到引起堆异常
	public static void testHeap() {
		List list = new ArrayList();
		for (;;) {
			System.out.println(count++);
			list.add(new Hashtable(20000000));
		}
	}
	
	//递归调用方法，直到栈内存耗尽引起栈异常
	public static void testStack1(){
		System.out.println(count++);
		testStack1();
	}
	
	//在Windows环境下运行该方法，可能会导致系统假死，
	//由于windows平台下Java的线程是映射到操作系统内核线程上的
	public static void testStack2(){
		while(true){
			Thread t = new Thread(new Runnable(){
				public void run() {
					//一个不会停止的旅行
					while(true){}
				}
			});
			t.start();
		}
	}
	
	public static void main(String[] args) throws IOException {
		
		testHeap();		
		//testStack1();
		//testStack2();
	}

}



```

## 1. 堆内存溢出

- Java 堆用于存储对象实例，只要不断滴创建对象，并保证GC Roots到对象之间有可达路径来避免垃圾回收机制清除这些对象，那么在对象数量到达最大堆限制时则会产生堆溢出异常

**运行结果:**


```java

1
2
3
4
5
6
7
8
9
Exception in thread "main" java.lang.OutOfMemoryError: Java heap space
	at java.util.Hashtable.<init>(Hashtable.java:162)
	at java.util.Hashtable.<init>(Hashtable.java:175)
	at chang.hu.t2.Q01.testHeap(Q01.java:16)
	at chang.hu.t2.Q01.main(Q01.java:27)

```


## 2. 虚拟机栈和本地方法栈溢出

关于虚拟机栈和本地方法栈，Java虚拟机规范中描述了两种异常：

- 如果线程请求的栈深度大于虚拟机所允许的最大深度，将抛出_StackOverflowError_异常
- 如果虚拟机在扩展栈时无法申请到足够的内存空间，则抛出_OutOfMemoryError_异常


testStack1 方法用来模拟 StackOverflowError异常

testStack2 方法用来模拟OutOfMemoryError异常

**注：** testStack2 方法Windows平台上执行是有风险，可能导致操作系统假死. 