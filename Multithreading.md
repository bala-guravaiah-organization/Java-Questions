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
