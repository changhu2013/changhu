---

layout: default
title: Java 堆异常 和 栈异常
abstract: 
comments: false

---

- 我们知道new出来的对象存放在堆内存中，同时方法栈中存放对象的引用关系。

```java

   public class Q01 {

    //循环创建较大的对象直到引起堆异常
	public static void testHeap() {
		for (;;) {
			Hashtable ht = new Hashtable(200000000);
		}
	}

	static int count = 1;
	//递归调用方法，直到栈内存耗尽引起栈异常
	public static void testStack(){
		count++;
		testStack();
	}
	
	public static void main(String[] args) throws IOException {
		
		//testHeap();		
		testStack();
	}

}


```


  

