#### What are the Common Issues in Multithreading?
- **Race conditions:** Multiple threads modifying shared data without synchronization.
- **Deadlocks:** Two or more threads waiting indefinitely for each other.
- **Thread starvation:** Some threads not getting CPU time due to high-priority threads.

---
#### What are the Differences Between `Runnable` and `Thread`?

| Feature          | `Thread` Class | `Runnable` Interface |
|-----------------|---------------|----------------------|
| **Inheritance** | Extends `Thread` class (single inheritance) | Implements `Runnable` interface (allows multiple inheritance) |
| **Code Reusability** | Less reusable (as extending a class restricts further inheritance) | More reusable (can be implemented along with other interfaces) |
| **Object Creation** | Creates a thread object directly | Requires `Thread` object to run |
| **Best Practice** | Not recommended unless overriding `Thread` methods | Preferred for better design flexibility |

---
#### What is the Purpose of `start()` method in Thread class?

The `start()` method in the **Thread** class is used to **begin execution** of a new **thread**. It performs the following functions:

1. **Creates a new thread** in the **JVM**.
2. Calls the **`run()` method** of the `Thread` class (or **`Runnable`** implementation) **internally**.
3. Runs **concurrently** in a **separate thread**, enabling **parallel execution**.

#### Example
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread is running...");
    }
}

public class ThreadExample {
    public static void main(String[] args) {
        MyThread t1 = new MyThread();
        t1.start();  // Starts the thread and calls run() internally
    }
}
```

#### Important Notes
- Calling **`run()` directly** does **not** start a **new thread**; it executes in the **main thread**.
- Calling **`start()` twice** on the **same thread instance** throws **`IllegalThreadStateException`**.

#### Key Takeaways
- `start()` is responsible for **creating a new thread**.
- It enables **parallel execution** by running **`run()`** in a separate **thread**.
- Directly calling **`run()`** does **not** create a new **thread**.
- A **thread cannot be restarted** once it has been started and completed execution.

---

#### Thread Life Cycle:

![Thread Life Cycle](Useful_Important_Concept_Images/MultiThreading_Thread_Life_Cycle.jpg)

#### Synchronous vs Asynchronous Programming in Concurrency & Parallelism

| Feature         | **Synchronous** | **Asynchronous** |
|---------------|----------------|------------------|
| **Execution** | Tasks run one after another, blocking the thread until completion. | Tasks can run independently, without blocking the thread. |
| **Concurrency** | Limited concurrency; each task must finish before the next one starts. | Supports high concurrency by scheduling tasks to run when resources are available. |
| **Parallelism** | Typically single-threaded unless used with multi-threading. | Can achieve parallelism when combined with multi-threading. |
| **Thread Usage** | Usually single-threaded, but can use multiple threads explicitly. | Can use multiple threads, event loops, or callbacks to improve efficiency. |
| **Example** | Reading files one by one in sequence. | Reading multiple files simultaneously using non-blocking I/O. |

---