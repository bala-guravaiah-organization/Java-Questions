### Multithreading Terminology - Fast Track Guide

#### CPU & Core
- **CPU (Central Processing Unit):** Executes program instructions.
- **Core:** An independent processing unit within a CPU. More cores enable true parallel execution.
  - Example: A **quad-core processor** can run four tasks simultaneously (e.g., web browser, music player, downloads, system updates).

#### Program vs. Process vs. Thread
- **Program:** A set of instructions written in a programming language (e.g., Microsoft Word).
- **Process:** An instance of a running program. The OS manages its execution.
  - Example: Opening **Microsoft Word** creates a new process.
- **Thread:** The smallest unit of execution within a process. Threads share resources but execute independently.
  - Example: A **web browser** runs multiple threads—one for rendering, one for JavaScript, one for user input.

#### Multitasking vs. Multithreading
- **Multitasking:** Running multiple processes simultaneously.
  - **Single-core CPU:** Uses time-sharing (rapid switching).
  - **Multi-core CPU:** Runs tasks in true parallel.
  - Example: Browsing the internet while listening to music and downloading a file.
- **Multithreading:** Running multiple threads **within the same process**.
  - Example: A web browser using separate threads for rendering, scripting, and user interaction.

#### Key Mechanism: Context Switching
- **Definition:** The OS saves the state of a running process/thread and loads the next one.
- **Purpose:** Allows multiple processes/threads to share the CPU efficiently.
  - **Single-core CPU:** Creates an **illusion** of parallel execution.
  - **Multi-core CPU:** Enables **true** parallel execution by distributing tasks across cores.

#### Multitasking vs. Multithreading – Key Difference
- **Multitasking:** Manages **multiple applications** (processes).
- **Multithreading:** Manages **multiple threads** within a **single** application/process.

---

### Concurrency vs. Parallelism

#### 🚀 Concurrency
#### **Definition**
> Concurrency means executing multiple tasks **in overlapping time periods**, but **not necessarily at the same time**.

#### **Example**
Imagine you are **singing** and **eating** at the same time. Since both require your mouth, you cannot do them **simultaneously**. Instead, you will **switch between them**, i.e., eat for some time, then sing, then eat again. 

🔹 **Concurrency = Task Switching** (one task at a time, but switching between tasks quickly)

#### **How It Works in Computers?**
- In a **single-core processor**, concurrency is achieved using **context switching**, where the CPU rapidly switches between tasks.

```plaintext
Task 1 -> Task 2 -> Task 1 -> Task 2 (Switching back and forth)
```

---

#### ⚡ Parallelism
#### **Definition**
> Parallelism means executing multiple tasks **simultaneously** at the same time.

#### **Example**
Imagine you are **cooking** while **talking on the phone**. Both actions can happen at the same time because they use different resources (hands for cooking, mouth for talking).

🔹 **Parallelism = Tasks running at the same time on multiple resources**

#### **How It Works in Computers?**
- In a **multi-core processor**, tasks can run truly **in parallel**, meaning different tasks run on different cores **without switching**.

```plaintext
Task 1 | Task 2 (Executing at the same time on different cores)
```

---

#### 🔄 Concurrency vs. Parallelism
| Feature       | Concurrency | Parallelism |
|--------------|------------|------------|
| Execution | Tasks start, run, and complete in overlapping time but **not simultaneously** | Tasks run **at the same time** |
| Example | Singing & Eating (Switching) | Cooking & Talking (Simultaneous) |
| Processor | Works on **single-core** | Requires **multi-core** |
| Technique | **Context Switching** | **True Parallel Execution** |

---

#### 🔗 How They Are Related?
✅ **Concurrency enables Parallelism** when multiple cores are available.  
✅ In a **single-core CPU**, tasks execute **one after another** using **context switching**.  
✅ In a **multi-core CPU**, tasks can be executed **truly in parallel** without switching.  

---

#### 🏁 Conclusion
- **Use Concurrency** when you have a **single-core CPU** or want to efficiently switch between multiple tasks.
- **Use Parallelism** when you have a **multi-core CPU** and want to perform tasks **truly simultaneously**.

---

#### What are the ways to Create Threads in Java ?
Java provides two main ways to create threads:

#### 1. Extending `Thread` Class
```java
class MyThread extends Thread {
    public void run() {
        System.out.println("Thread running...");
    }
    public static void main(String[] args) {
        MyThread t = new MyThread();
        t.start(); // Starts a new thread
    }
}
```

#### 2. Implementing `Runnable` Interface
```java
class MyRunnable implements Runnable {
    public void run() {
        System.out.println("Thread running...");
    }
    public static void main(String[] args) {
        Thread t = new Thread(new MyRunnable());
        t.start(); // Starts a new thread
    }
}
```
---
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










