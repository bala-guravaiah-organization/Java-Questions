**Synchronized Method(Non-Static - Single Instance - two Threads)** :
```java
package com.seleniumexpress.java8.multithreading;

public class Counter {
	
	private int count = 0;
	
	public synchronized void increment()
	{
		count++;
		try {
			Thread.sleep(10000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println(Thread.currentThread().getName() + " incremented count to: " + count);
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
		
	}

}

```
 - The problem can be modeled as finding all possible ways to arrange 5 increments by Thread-0 and 5 increments by Thread-1 in a sequence of 10 steps. This is a combinatorial problem where we choose 5 positions out of 10 for Thread-0 (the remaining 5 are for Thread-1), and the count increases sequentially. The number of possible sequences is given by the binomial coefficient:
(105)=10!5!5!=252\binom{10}{5} = \frac{10!}{5!5!} = 252\binom{10}{5} = \frac{10!}{5!5!} = 252
.

This means there are 252 possible distinct output sequences. Listing all of them explicitly would be impractical here, but I can explain the range of possibilities and provide representative examples.

```java
Thread-0 incremented count to: 1
Thread-0 incremented count to: 2
Thread-0 incremented count to: 3
Thread-0 incremented count to: 4
Thread-0 incremented count to: 5
Thread-1 incremented count to: 6
Thread-1 incremented count to: 7
Thread-1 incremented count to: 8
Thread-1 incremented count to: 9
Thread-1 incremented count to: 10
```
```java
Thread-0 incremented count to: 1
Thread-1 incremented count to: 2
Thread-0 incremented count to: 3
Thread-1 incremented count to: 4
Thread-0 incremented count to: 5
Thread-1 incremented count to: 6
Thread-0 incremented count to: 7
Thread-1 incremented count to: 8
Thread-0 incremented count to: 9
Thread-1 incremented count to: 10
```
```java
Thread-0 incremented count to: 1
Thread-0 incremented count to: 2
Thread-1 incremented count to: 3
Thread-1 incremented count to: 4
Thread-0 incremented count to: 5
Thread-1 incremented count to: 6
Thread-0 incremented count to: 7
Thread-0 incremented count to: 8
Thread-1 incremented count to: 9
Thread-1 incremented count to: 10
```
**Why So Many Possibilities?**
- The **synchronized method ensures that each increment is atomic**, but it doesn’t dictate which thread gets the lock next after it’s released. Between each increment() call, the lock is released, and either thread can acquire it.

- The Thread.sleep(1000) delays execution within each call but doesn’t affect lock release. After the method ends, the lock is free, and the scheduler decides which thread runs next.

- With 10 increments and 2 threads, any sequence where Thread-0 gets 5 and Thread-1 gets 5 is valid, leading to 252 combinations.
---

**Key Points to Address Your Doubt**


**1. Lock Release After Method Execution**:
You’re absolutely correct: a `synchronized` method releases its lock when the method completes execution. In your code, the lock on the `Counter` object (`c1`) is released every time increment() finishes one call. So, after `Thread-0` increments `count` to 1 and prints the message, the lock is indeed released.

**2. Why Doesn’t Thread-1 Jump In?**:
If the lock is released after each `increment()` call, why doesn’t `Thread-1` get a chance to acquire it between `Thread-0’`s iterations? The answer lies in thread scheduling and the timing of execution, influenced by the `Thread.sleep(1000)` inside the `synchronized` method.


**3. What Happens Step-by-Step**:
Let’s walk through the execution:
1. `Thread-0 `starts and calls `c1.increment()`.

2. It acquires the lock on `c1`, increments `count` to 1, sleeps for 1 second (still holding the lock), prints "`Thread-0` incremented `count` to: 1", and then exits `increment()`.

3. At this point, the lock on `c1` is released because the method has completed.

4. `Thread-0` immediately proceeds to the next iteration of its for loop (since it’s still running and the loop is tight) and calls `c1.increment()` again, re-acquiring the lock.

5. Meanwhile, `Thread-1` is waiting to acquire the lock but doesn’t get a chance because Thread-0 is quick to re-acquire it after each release.


- The critical factor here is that Thread-0 doesn’t pause long enough between iterations for the JVM scheduler to give Thread-1 a chance to run. After releasing the lock, Thread-0 is still the active thread and jumps right back into the next iteration.

**4. Effect of Thread.sleep(1000)**:
The `sleep(1000)` inside `increment()` makes the thread pause for 1 second while holding the lock. However, once the method completes and the lock is released, there’s no delay before `Thread-0` moves to the next loop iteration. The transition from releasing the lock to re-acquiring it happens almost instantly (in CPU terms), leaving little opportunity for `Thread-1` to intervene.

During the 1-second sleep, `Thread-1` can’t do anything because `Thread-0` still holds the lock. After the sleep, when the lock is released, `Thread-0` is already poised to grab it again.

**5. Thread Scheduling and Fairness**:
Java’s thread scheduler doesn’t guarantee fairness by default. Once `Thread-0` starts running, it can keep executing its loop iterations back-to-back, re-acquiring the lock each time before `Thread-1` gets scheduled. The scheduler might not preempt `Thread-0` to let `Thread-1` run unless forced (e.g., by a higher-priority thread or explicit yielding).

In practice, `Thread-0` finishing all 5 iterations before `Thread-1` starts is a common outcome in this scenario, but it’s not strictly guaranteed—it’s a race condition influenced by scheduling.

**6. Why the Output Is Sequential**
Because `Thread-0` re-acquires the lock immediately after each `increment()` call, it completes all 5 iterations (count from 1 to 5) before `Thread-1` gets its turn. Only after `Thread-0` finishes its entire for loop and stops calling `increment()` does `Thread-1` acquire the lock and start its 5 iterations (count from 6 to 10).

The synchronized keyword ensures that each individual `increment()` call is atomic (no interleaving of increments), but it doesn’t enforce that the two threads take turns—it only ensures mutual exclusion.

**7. Could It Interleave?**
Yes, it’s theoretically possible for `Thread-1` to acquire the lock between two of `Thread-0`’s iterations if the scheduler decides to switch threads at just the right moment (e.g., after `Thread-0` releases the lock and before it re-acquires it). For example, you might see:

```java
Thread-0 incremented count to: 1
Thread-1 incremented count to: 2
Thread-0 incremented count to: 3
```

However, this requires the scheduler to preempt `Thread-0` and give `Thread-1` a chance, which doesn’t happen reliably in this code due to the tight loop and the timing. To force interleaving, you could:
Add `Thread.yield()` after `increment()` to hint the scheduler to switch threads.

Use a mechanism like a Lock with fairness policies (e.g., ReentrantLock with fair=true).


### **Conclusion**:

The lock is released after each `increment()` call completes, as per the rules of synchronized. However, `Thread-0` quickly re-acquires it for the next iteration because it’s still running and the loop is tight. The 1-second sleep delays execution within each call but doesn’t help Thread-1 get in between iterations since the lock is held during the sleep.

The sequential output (all `Thread-0` then all `Thread-1`) is a result of `Thread-0` dominating the lock acquisition due to scheduling behavior, not because the lock isn’t released—it is released, just not in a way that gives `Thread-1` a fair shot until `Thread-0`is done.

---
































