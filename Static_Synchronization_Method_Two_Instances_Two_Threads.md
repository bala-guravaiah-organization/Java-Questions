#### **Simple Rule**: Only one thread can run deposit() or withdraw() at a time, no matter how many Bank objects exist (or even if none exist).

**What is a Static Synchronized Method?**
A **static synchronized method** is a method marked with the `synchronized` keyword that belongs to the class itself (not an instance). When a thread enters this method, it acquires a lock on the `Class` object associated with the class (e.g., `MyClass.class`). This ensures that only one thread can execute any static synchronized method of that class at a time, regardless of how many instances exist or even if no instances exist.

---
**Key Characteristics**
**Lock Object:** The lock is the Class object, not an instance. Every class loaded by the JVM has a single Class object representing it.

**Scope:** The synchronization applies across the entire class, meaning it affects all threads trying to access any static synchronized method of that class.

**Purpose:** It’s used to protect shared static data (class-level data) that multiple threads might access.

**How It Works**
- When a thread calls a static synchronized method, it must first acquire the lock on the `Class` object.

- Other threads trying to call the same method—or any other static synchronized method in the same class—will block until the lock is released.

- The lock is automatically released when the thread exits the method.

---

**Example 1: Basic Static Synchronized Method**

```java
package com.seleniumexpress.java8.multithreading;

public class StaticCounter {
	
	private static int count = 0;
	
	public static synchronized void increment()
	{
		System.out.println("Lock Acquired by " + Thread.currentThread().getName());
		count++;
		try {
			Thread.sleep(10000);
		} catch (InterruptedException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println(Thread.currentThread().getName() + " incremented count to: " + count);
		System.out.println("Lock Released by " + Thread.currentThread().getName());
	}
	public static int getCount()
	{
		return count;
	}
	
	public static void main(String[] args) {
		
		// first instance
		StaticCounter c1 = new StaticCounter();
		
		Thread t1 = new Thread(() -> {
			for(int i = 0; i<5; i++)
			{
				c1.increment();
			}
		});
		t1.start();
		
		// first instance
		StaticCounter c2 = new StaticCounter();
		Thread t3 = new Thread(() -> 
		{
			for (int i = 0; i < 5; i++) {
				c2.increment();
			}	
		});
		t3.start();
		
	}

}
```
**Output** :
```
Lock Acquired by Thread-0
Thread-0 incremented count to: 1
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-0 incremented count to: 2
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-0 incremented count to: 3
Lock Released by Thread-0
Lock Acquired by Thread-0
Thread-0 incremented count to: 4
Lock Released by Thread-0
Lock Acquired by Thread-0
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

**Explanation**
**Lock:** Both threads are trying to execute increment(), which is static and synchronized. The lock is on StaticCounter.class.

**Behavior:** Only one thread can hold the StaticCounter.class lock at a time. Thread-1 completes all its increments (1, 2, 3), then Thread-2 takes over (4, 5, 6). The sleep exaggerates this to show the blocking.

**Result:** The count increments safely from 0 to 6, with no overlap or race conditions.


**Example 2: Multiple Static Synchronized Methods**

```java
package com.seleniumexpress.java8.multithreading;

public class Bank {
	
    private static int balance = 0; // Shared class-level balance

    // Static synchronized method to deposit money
    public static synchronized void deposit(int amount) {
    	System.out.println("Thread Acquired by : " + Thread.currentThread().getName());
        balance += amount;
        System.out.println(Thread.currentThread().getName() + " deposited " + amount + ", balance = " + balance);
        try {
            Thread.sleep(500); // Simulate some processing time
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        
        System.out.println("Thread Released by : " + Thread.currentThread().getName());
    }

    // Static synchronized method to withdraw money
    public static synchronized void withdraw(int amount) {
    	System.out.println("Thread Acquired by : " + Thread.currentThread().getName());
    	
        balance -= amount;
        System.out.println(Thread.currentThread().getName() + " withdrew " + amount + ", balance = " + balance);
        try {
            Thread.sleep(500); // Simulate some processing time
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println("Thread Acquired by : " + Thread.currentThread().getName());
    }

    public static void main(String[] args) {
        // Thread 1: Deposits and withdraws
        Thread t1 = new Thread(() -> {
            Bank.deposit(100);  // Add 100
            Bank.withdraw(50);  // Take 50
        }, "Thread-1");

        // Thread 2: Deposits and withdraws
        Thread t2 = new Thread(() -> {
            Bank.deposit(200);  // Add 200
            Bank.withdraw(150); // Take 150
        }, "Thread-2");

        t1.start();
        t2.start();
    }
}

```
**Output :**

```
Thread Acquired by : Thread-1
Thread-1 deposited 100, balance = 100
Thread Released by : Thread-1
Thread Acquired by : Thread-1
Thread-1 withdrew 50, balance = 50
Thread Acquired by : Thread-1
Thread Acquired by : Thread-2
Thread-2 deposited 200, balance = 250
Thread Released by : Thread-2
Thread Acquired by : Thread-2
Thread-2 withdrew 150, balance = 100
Thread Acquired by : Thread-2
```
**What Happens When You Run This?**
Both deposit() and withdraw() are static synchronized methods, so they use the same lock: Bank.class. Only one thread can execute either method at a time. Let’s break it down:

**Step-by-Step Explanation**
1. **Thread-1 Starts**:
- Thread-1 calls Bank.deposit(100).

- It acquires the lock on Bank.class.

- It adds 100 to balance (0 → 100) and prints the result.

- It sleeps for 500ms (simulating work), holding the lock.

2. **Thread-2 Waits**:
- Thread-2 tries to call Bank.deposit(200) while Thread-1 has the lock.

- Since Bank.class is locked, Thread-2 blocks (waits).

3. **Thread-1 Continues**:
 - Thread-1 finishes deposit(100) and releases the lock after exiting the method.

- Thread-1 then calls Bank.withdraw(50), reacquires the Bank.class lock, subtracts 50 (100 → 50), prints, and sleeps for 500ms.

- Thread-2 is still waiting because Thread-1 has the lock again.

4. **Thread-1 Finishes, Thread-2 Takes Over**:
- Thread-1 exits `withdraw(50)` and releases the lock.

- Thread-2 acquires the lock, runs `Bank.deposit(200)`, adds 200 (50 → 250), prints, and sleeps.

- After finishing `deposit(200)`, Thread-2 calls `Bank.withdraw(150)`, subtracts 150 (250 → 100), prints, and finishes.


**Why This Works**
**Single Lock**: Both `deposit()` and `withdraw()` are static synchronized, so they share the Bank.class lock. No matter which method is called, only one thread can run at a time.

**Thread Safety**: The balance is updated safely—no race conditions where two threads could overwrite each other’s changes.

**Sequential Execution**: The `sleep(500)` makes it obvious that `Thread-2` waits for `Thread-1` to finish all its operations before starting.


**What If They Weren’t Synchronized?**

Without `synchronized`, two threads could access `balance` simultaneously, leading to unpredictable results. For example: 
- Thread-1 reads `balance = 0`, adds 100.

- Thread-2 reads `balance = 0`, adds 200.

- Both write back, and `balance` might end up as 100 or 200 instead of 300.

- With `synchronized`, this can’t happen—the updates are atomic and sequential.

**Key Takeaways**
**Lock Scope**: The Bank.class lock ensures that all static synchronized methods in the Bank class are mutually exclusive.

**Simple Rule**: Only one thread can run deposit() or withdraw() at a time, no matter how many Bank objects exist (or even if none exist).

**Use Case**: Perfect for protecting static variables like balance that belong to the class, not individual instances.
























