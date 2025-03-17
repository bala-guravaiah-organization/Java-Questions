#### Multithreading vs Multiprocessing

#### What is Multiprocessing?
Multiprocessing is when multiple **processes** run at the same time. Each process has its own memory space, making it more powerful but also more resource-intensive compared to multithreading.

#### Example: Restaurant Analogy 🍽️
- Imagine a restaurant with **multiple kitchens**, each with its own chef.
- Each chef works independently, making the process faster but requiring more space and staff.

#### Example in Java:
```java
import java.io.IOException;

public class MultiprocessingExample {
    public static void main(String[] args) {
        try {
            ProcessBuilder pb = new ProcessBuilder("notepad.exe"); // Opens a new process
            pb.start();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
#### What is Multithreading?
Multithreading is a technique where multiple **threads** run within a single **process**. These threads share the same memory space but execute independently, allowing programs to perform multiple operations simultaneously.

#### Example: Restaurant Analogy 🍽️
- Imagine a restaurant with **one kitchen** and **multiple waiters**.
- Each waiter handles a different order (task), but they all use the same kitchen (shared memory).
- If the kitchen is busy, a waiter might have to wait.

#### Example in Java:
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread " + Thread.currentThread().getId() + " is running");
    }
}

public class MultithreadingExample {
    public static void main(String[] args) {
        for (int i = 0; i < 3; i++) {
            MyThread thread = new MyThread();
            thread.start(); // Starts a new thread
        }
    }
}
```



#### Key Differences
| Feature            | Multithreading 🧵 | Multiprocessing 🖥️ |
|--------------------|------------------|------------------|
| Execution Units   | Multiple **threads** in one **process** | Multiple **processes** |
| Memory Usage     | **Shared** memory | **Separate** memory |
| Performance     | Faster for **light** tasks | Better for **heavy** tasks |
| Overhead        | Lower (less memory used) | Higher (more memory needed) |
| Example         | Web browser, Chat apps | Video rendering, AI training |
---

#### ✅ **How many Ways to Create a Thread in Java?**

#### 🔹 **1. Extending the `Thread` class**  
👉 Suitable when you **don’t need to extend another class**.

#### **Example:**
```java
class MyThread extends Thread { // Extending Thread
    public void run() { // Override run() method
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyThread t1 = new MyThread(); // Create thread
        t1.start(); // Start thread execution
    }
}
```

🟢 **Key Points:**
- `MyThread` **inherits** from `Thread`.
- `run()` contains the **task** for the thread.
- `start()` **calls `run()` internally in a separate thread**.

---

#### 🔹 **2. Implementing the `Runnable` interface**  
👉 Best when you **want to extend another class**.

#### **Example:**
```java
class MyRunnable implements Runnable { // Implementing Runnable
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        MyRunnable myRunnable = new MyRunnable();
        Thread t1 = new Thread(myRunnable); // Pass to Thread constructor
        t1.start(); // Start thread
    }
}
```

🟢 **Key Points:**
- `MyRunnable` **implements** `Runnable` instead of `Thread`.
- We pass an **instance of `MyRunnable` to `Thread`**.
- `start()` runs the `run()` method **inside a new thread**.

---

#### 🔥 **Key Differences**
| Feature | Extending `Thread` | Implementing `Runnable` |
|---------|-----------------|-----------------|
| **Inheritance** | Can’t extend another class | Can extend another class |
| **Reusability** | Less reusable | More reusable |
| **Flexibility** | Less flexible | More flexible (recommended) |

👉 **Best Practice:** Prefer `Runnable` for better design and reusability.

---

#### 🔹 **When to Use Which?**
- **Use `Thread`** → When your class is not extending anything else.
- **Use `Runnable`** → When your class already extends another class (**Recommended for most cases**).

---

#### Why do we Prefer `Runnable` Over `Thread`?

#### 1. **Better Design (Separation of Concerns)**
   - `Runnable` follows **composition**, whereas `Thread` follows **inheritance**.
   - Java allows **only single inheritance**, so extending `Thread` prevents extending another class.

#### Example:
```java
// Implementing Runnable instead of extending Thread
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Running via Runnable");
    }
}

// Another class that can be extended since we use Runnable instead of Thread
class MyExtendedClass {} 

public class RunnableDesignExample {
    public static void main(String[] args) {
        // Create a Runnable instance
        MyTask task = new MyTask();
        
        // Pass it to a Thread instance
        Thread t = new Thread(task);
        
        // Start the thread
        t.start();
    }
}
```

#### 2. **Code Reusability**
   - If a class implements `Runnable`, it can be reused and passed to multiple threads.
   - A `Thread` object, once started, **cannot be restarted**, but a `Runnable` object can be reused.

#### Example:
```java
// Implementing Runnable so it can be used by multiple threads
class SharedTask implements Runnable {
    @Override
    public void run() {
        System.out.println(Thread.currentThread().getName() + " is executing the task");
    }
}

public class RunnableReusabilityExample {
    public static void main(String[] args) {
        // Creating a single instance of SharedTask
        SharedTask task = new SharedTask();
        
        // Creating multiple threads using the same Runnable instance
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        
        // Start both threads
        t1.start();
        t2.start();
    }
}
```

#### 3. **Resource Sharing**
   - Multiple threads can share the same `Runnable` instance, improving performance.
   - If extending `Thread`, a new instance is required for each new thread.

#### Example:
```java
class CounterTask implements Runnable {
    private int counter = 0; // Shared resource

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            counter++; // Incrementing shared counter
            System.out.println(Thread.currentThread().getName() + " Counter: " + counter);
        }
    }
}

public class RunnableSharingExample {
    public static void main(String[] args) {
        // Create a single instance of CounterTask
        CounterTask task = new CounterTask();
        
        // Multiple threads sharing the same task instance
        Thread t1 = new Thread(task);
        Thread t2 = new Thread(task);
        
        // Start both threads
        t1.start();
        t2.start();
    }
}
```

#### 4. **Flexibility**
   - `Runnable` can be executed by `ThreadPoolExecutor`, `ScheduledExecutorService`, etc.
   - `Thread` is tightly coupled to the thread lifecycle, limiting flexibility.

#### Example:
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

// A task that can be executed by multiple threads from a thread pool
class PooledTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Task executed by " + Thread.currentThread().getName());
    }
}

public class RunnableFlexibilityExample {
    public static void main(String[] args) {
        // Creating a thread pool with 3 threads
        ExecutorService executor = Executors.newFixedThreadPool(3);
        
        // Submitting multiple tasks to the thread pool
        for (int i = 0; i < 5; i++) {
            executor.execute(new PooledTask());
        }
        
        // Shutdown executor after task completion
        executor.shutdown();
    }
}
```

---

#### Example Code

#### Using `Runnable` (Preferred Approach)
```java
// Define a task by implementing Runnable interface
class MyTask implements Runnable {
    @Override
    public void run() {
        System.out.println("Task is running...");
    }
}

public class RunnableExample {
    public static void main(String[] args) {
        // Create a Runnable instance
        MyTask task = new MyTask();
        
        // Create a Thread and pass Runnable instance to it
        Thread t1 = new Thread(task);
        
        // Start the thread
        t1.start();
    }
}
```

#### Using `Thread` (Less Preferred)
```java
// Define a task by extending Thread class
class MyThread extends Thread {
    @Override
    public void run() {
        System.out.println("Thread is running...");
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        // Create an instance of MyThread
        MyThread t1 = new MyThread();
        
        // Start the thread
        t1.start();
    }
}
```

#### Conclusion:
✔ **Use `Runnable`** when the task is independent of the thread lifecycle.
❌ **Use `Thread`** only if you need to override its methods like `start()`, `join()`, etc.

---
#### Explain Thread Life Cycle in Java

A thread in Java goes through the following states during its execution:

1. **NEW**  
2. **RUNNABLE**  
3. **BLOCKED**  
4. **WAITING**  
5. **TIMED_WAITING**  
6. **TERMINATED**  

#### **1. NEW State**
- When a thread is created but **not yet started**, it is in the **NEW** state.  
- It remains in this state until `start()` is called.

#### **Example:**
```java
class NewStateExample extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        // Creating a thread but not starting it
        NewStateExample thread = new NewStateExample();
        System.out.println("Thread state: " + thread.getState()); // Output: NEW
    }
}
```

---

#### **2. RUNNABLE State**
- When `start()` is called, the thread moves from **NEW → RUNNABLE** state.
- It is now ready to run but waiting for CPU time.

#### **Example:**
```java
class RunnableStateExample extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) {
        RunnableStateExample thread = new RunnableStateExample();
        thread.start(); // Now thread is in RUNNABLE state
        System.out.println("Thread state: " + thread.getState()); // Output: RUNNABLE or TERMINATED
    }
}
```

---

#### **3. BLOCKED State**
- A thread enters the **BLOCKED** state if it tries to access a **synchronized method** locked by another thread.
- It stays in this state until the lock is released.

#### **Example:**
```java
class BlockedStateExample {
    // Shared resource
    synchronized void sharedMethod() {
        System.out.println(Thread.currentThread().getName() + " is inside sharedMethod.");
        try {
            Thread.sleep(3000); // Simulating work
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}

class ThreadA extends Thread {
    BlockedStateExample obj;
    ThreadA(BlockedStateExample obj) { this.obj = obj; }

    public void run() {
        obj.sharedMethod();
    }
}

class ThreadB extends Thread {
    BlockedStateExample obj;
    ThreadB(BlockedStateExample obj) { this.obj = obj; }

    public void run() {
        obj.sharedMethod(); // Will be BLOCKED if ThreadA is inside this method
    }
}
```

---

#### **4. WAITING State**
- A thread goes into **WAITING** state when it calls `wait()`.
- It waits **indefinitely** until another thread calls `notify()`.

#### **Example:**
```java
class WaitingStateExample {
    public static void main(String[] args) throws InterruptedException {
        final Object lock = new Object();

        Thread t1 = new Thread(() -> {
            synchronized (lock) {
                try {
                    System.out.println("Thread going into WAITING state...");
                    lock.wait(); // Thread enters WAITING state
                    System.out.println("Thread resumed after notify...");
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
        });

        t1.start();
        Thread.sleep(1000);
        System.out.println("Thread state: " + t1.getState()); // Output: WAITING

        synchronized (lock) {
            lock.notify(); // Notify t1 to resume
        }
    }
}
```

---

#### **5. TIMED_WAITING State**
- A thread enters **TIMED_WAITING** state when it waits for a **fixed time** using:
  - `Thread.sleep(time)`
  - `wait(time)`
  - `join(time)`

#### **Example:**
```java
class TimedWaitingStateExample {
    public static void main(String[] args) throws InterruptedException {
        Thread t1 = new Thread(() -> {
            try {
                System.out.println("Thread going into TIMED_WAITING state...");
                Thread.sleep(5000); // Thread enters TIMED_WAITING state
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
        });

        t1.start();
        Thread.sleep(1000);
        System.out.println("Thread state: " + t1.getState()); // Output: TIMED_WAITING
    }
}
```

---

#### **6. TERMINATED State**
- A thread moves to **TERMINATED** state after completing execution.

#### **Example:**
```java
class TerminatedStateExample extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }

    public static void main(String[] args) throws InterruptedException {
        TerminatedStateExample thread = new TerminatedStateExample();
        thread.start();
        Thread.sleep(1000); // Ensure thread execution is completed
        System.out.println("Thread state: " + thread.getState()); // Output: TERMINATED
    }
}
```

---

#### **Summary of Thread States**
| **State**           | **Description** |
|--------------------|----------------|
| **NEW**           | Thread created but not started. |
| **RUNNABLE**      | Thread started and waiting for CPU. |
| **BLOCKED**       | Thread waiting for a locked resource. |
| **WAITING**       | Thread waiting indefinitely for another thread’s signal. |
| **TIMED_WAITING** | Thread waiting for a fixed time (`sleep()`, `wait(time)`). |
| **TERMINATED**    | Thread finished execution. |

---

#### **Conclusion**
1. **Threads start in the NEW state.**  
2. **Once started, they go to RUNNABLE.**  
3. **If waiting for a lock, they go to BLOCKED.**  
4. **If waiting indefinitely, they go to WAITING.**  
5. **If waiting for a fixed time, they go to TIMED_WAITING.**  
6. **When execution completes, they go to TERMINATED.**  
---