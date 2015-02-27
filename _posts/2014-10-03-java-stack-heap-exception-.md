---

layout: default
title: Java 堆异常 和 栈异常
abstract: 
comments: false

---

```java

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
	public static void testStack(){
		System.out.println(count++);
		testStack();
	}
	
	public static void main(String[] args) throws IOException {
		
		testHeap();
		//testStack();
	}

}


```

- 我们知道new出来的对象存放在堆内存中，同时方法栈中存放对象的引用关系。

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
