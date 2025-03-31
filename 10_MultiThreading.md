#### What are the Benefits of Multithreading?
- **Efficient CPU utilization**
- **Faster execution for parallel tasks**
- **Better responsiveness in applications**
- **Resource sharing among threads**

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

#### **When to Use What?**
- Use `Thread` **only if** you need to modify thread behavior.
- Use `Runnable` **when** you need better design flexibility and code reusability.

---
#### What is the Purpose of `start()` method in Thread class?

#### Purpose
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
#### Process and Thread Understanding
![Process and Thread Execution Example](MultiThreading_Process_Vs_Thread.jpg)

##### 1. Code Segment (Text Segment)
- Stores the compiled program’s machine code (instructions).
- It is **read-only** to prevent accidental modifications.
- Shared among multiple instances of the same program.

##### 2. Data Segment
- Stores **global and static variables**.
- Further divided into:
  - **Initialized Data Segment**: Contains global/static variables with assigned values.
  - **Uninitialized Data Segment (BSS - Block Started by Symbol)**: Stores global/static variables without assigned values (defaulted to zero).

##### 3. Register
- A small, fast memory inside the **CPU** used for temporary data storage and processing.
- Types of registers:
  - **General-purpose registers** (e.g., AX, BX in x86)
  - **Special-purpose registers** (e.g., Stack Pointer, Instruction Pointer)

##### 4. Counter Register (Program Counter - PC)
- Holds the address of the **next instruction** to be executed.
- Automatically increments after fetching an instruction.
- Used for **program flow control** (jumps, loops, function calls).

---

#### 📌 Summary
| Segment/Register | Description |
|-----------------|-------------|
| **Code Segment** | Stores program instructions |
| **Data Segment** | Holds global/static variables |
| **Register** | Fast storage inside CPU for temporary data |
| **Program Counter (PC)** | Holds address of the next instruction |
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

#### How Asynchronous and Synchronous Programming Related to Concurrency & Parallelism ? 

#### **Concurrency**
- Multiple tasks are executed *in an overlapping manner* (not necessarily at the same time).  
- **Synchronous concurrency**: Uses multi-threading but blocks when waiting.  
- **Asynchronous concurrency**: Uses event-driven programming (e.g., Java's `CompletableFuture`, Spring WebFlux).  

#### **Parallelism**
- Multiple tasks execute *exactly at the same time* using multiple CPU cores.  
- **Synchronous parallelism**: Uses multi-threading but with blocking operations.  
- **Asynchronous parallelism**: Uses non-blocking calls with multi-threading (e.g., Java's `ForkJoinPool`).  

### Example: Java 8 Asynchronous Execution using `CompletableFuture`
```java
import java.util.concurrent.CompletableFuture;

public class AsyncExample {
    public static void main(String[] args) {
        CompletableFuture<Void> future = CompletableFuture.runAsync(() -> {
            System.out.println("Executing asynchronously in thread: " + Thread.currentThread().getName());
        });
        future.join(); // Wait for completion
    }
}
```

#### Conclusion
- **Synchronous programming** is simple but may cause blocking and performance issues.
- **Asynchronous programming** improves efficiency by non-blocking execution.
- Choosing between them depends on the use case, resource availability, and application requirements.
---










