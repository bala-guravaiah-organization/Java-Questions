package com.seleniumexpress.java8.multithreading;

public class Counter {
	
	private int count = 0;
	
	public synchronized void increment()
	{
		System.out.println("Lock Acquired by " + Thread.currentThread().getName());
		count++;
		try {
			Thread.sleep(100);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println(Thread.currentThread().getName() + " incremented count to: " + count);
		System.out.println("Lock Released by " + Thread.currentThread().getName());
	}
	public int getCount()
	{
		return count;
	}
	
	public static void main(String[] args) {
		
		// first instance
		Counter c1 = new Counter();
		
		Thread t1 = new Thread(() -> {
			for(int i = 0; i<5; i++)
			{
				c1.increment();
			}
		});
		t1.start();
		
		Thread t2 = new Thread(() -> {
			for(int i = 0; i<5; i++)
			{
				c1.increment();
			}
		});
		t2.start();
		
		// first instance
		Counter c2 = new Counter();
		Thread t3 = new Thread(() -> 
		{
			for (int i = 0; i < 5; i++) {
				c2.increment();
			}	
		});
		t3.start();
		
	}

}

output :

```java
Lock Acquired by Thread-0
Lock Acquired by Thread-2
Thread-2 incremented count to: 1
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 1
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 2
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 2
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 3
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 3
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 4
Lock Released by Thread-2
Lock Acquired by Thread-2
Thread-0 incremented count to: 4
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-2 incremented count to: 5
Lock Released by Thread-2
Thread-0 incremented count to: 5
Lock Released by Thread-0
Lock Acquired by Thread-1
Thread-1 incremented count to: 6
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 7
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 8
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 9
Lock Released by Thread-1
Lock Acquired by Thread-1
Thread-1 incremented count to: 10
Lock Released by Thread-1
```

**Output Explanation**

`Threads 1 and 2`: Both operate on `counter1`. Because `increment()` is `synchronized`, only one `thread` can execute it on `counter1` at a time. The output will show count incrementing from 0 to 10 in a thread-safe order (e.g., "`Thread-1` incremented count to 1", "`Thread-2` incremented count to 2", etc.).

`Thread 3`: Operates on `counter2`, a different instance. It runs concurrently with Threads 1 and 2 because `counter2` has its own lock. Its count goes from 0 to 5 independently.

Key Point
The `lock` is on the instance (counter1 or counter2), not the class. Different objects = different locks = no blocking between them.



**Different objects = different locks = no blocking between them**

