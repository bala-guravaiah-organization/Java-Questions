#### What is Multithreading in Java

Multithreading in Java allows multiple threads to execute concurrently, enabling efficient CPU utilization and faster program execution. It is useful for tasks like parallel processing, background computations, and responsive UI applications.

#### Key Concepts
- **Thread:** A lightweight subprocess that executes independently.
- **Concurrency:** Multiple threads execute in an interleaved manner.
- **Parallelism:** True parallel execution when multiple CPUs are available.
- **Synchronization:** Controlling thread access to shared resources to prevent conflicts.

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

#### Conclusion
Multithreading is a powerful feature in Java for improving performance and efficiency. However, proper synchronization is essential to avoid concurrency issues.

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
**To understand better use this Link given by Concept and Coding by Shreyansh Jain.**

https://notebook.zohopublic.in/public/notes/74tdo52a4834de5554f09bc9ec3f11572cd11

#### Thread Life Cycle:

![Thread Life Cycle](Useful_Important_Concept_Images/MultiThreading_Thread_Life_Cycle.jpg)




