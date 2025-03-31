
#### Core Methods
- `Thread`
- `Runnable`

#### Modern Tools
- `ExecutorService`
- `CompletableFuture`
-  Virtual Threads (JDK 21+)

#### Specialized
- `ForkJoinPool`
- `ScheduledExecutorService`

#### Niche
- JNI (Java Native Interface)
- External Processes
- Frameworks


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
---
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
#### 3. Using Lambda Expression

Lambda expressions provide a concise way to express instances of functional interfaces in Java 8 and later. One common use case is simplifying thread creation using `Runnable` or `ExecutorService`.

#### Example: Using Lambda with Runnable
```java
public class Main {
    public static void main(String[] args) {
        Thread thread = new Thread(() -> System.out.println("Thread is running..."));
        thread.start();
    }
}
```
---
#### Key Points

####   `start()` vs `run()`
- **Use `start()`** to create and start a new thread.
- **Calling `run()` directly** executes the code in the current thread, not a new one.

#### Thread Safety
- When multiple threads access shared resources, use **synchronization mechanisms** (e.g., `synchronized` blocks/methods) to prevent race conditions.

---
#### 4. Modern Preference: ExecutorService
- Instead of manually creating threads, **ExecutorService** is recommended for better thread management and resource utilization.
- Example using `ExecutorService`:

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ExecutorExample {
    public static void main(String[] args) {
        ExecutorService executor = Executors.newSingleThreadExecutor();
        executor.execute(() -> System.out.println("Task executed using ExecutorService"));
        executor.shutdown();
    }
}
```

Using `ExecutorService` allows better control over thread lifecycle and avoids unnecessary thread creation overhead.

---

### 5. Using Callable and Future with ExecutorService

Unlike `Runnable`, which cannot return a result or throw checked exceptions, the `Callable` interface allows a thread to:
- Return a computed value.
- Throw checked exceptions.

It is typically used with `ExecutorService` and paired with `Future` to retrieve the result asynchronously.

---

#### Steps to Implement:
1. Implement the `Callable` interface and override the `call()` method.
2. Submit the `Callable` task to an `ExecutorService`.
3. Retrieve the result using a `Future` object.
4. Shut down the `ExecutorService` after execution.

---

#### Code Example
```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws Exception {
        // Create an ExecutorService with a single thread
        ExecutorService executor = Executors.newSingleThreadExecutor();
        
        // Define a Callable task that returns a result
        Callable<String> callable = () -> {
            Thread.sleep(1000); // Simulate work
            return "Task completed!";
        };

        // Submit the task and receive a Future object
        Future<String> future = executor.submit(callable);
        
        // Retrieve and print the result (blocks until available)
        System.out.println(future.get());
        
        // Shutdown the executor
        executor.shutdown();
    }
}
```

---

#### Key Points:
✅ `Callable<T>` returns a result, unlike `Runnable`.
✅ Use `Future<T>` to obtain the result asynchronously.
✅ `future.get()` blocks until the computation is complete.
✅ Always shutdown the `ExecutorService` after execution.

---

#### Use Case
📌 When you need a thread to perform a computation and return a result, such as:
- Fetching data from an external source.
- Performing a complex calculation.
- Executing background tasks with results.

---

#### 🔥 Pro Tip
Instead of blocking with `future.get()`, use `isDone()` to check if the task is complete before retrieving the result:
```java
if (future.isDone()) {
    System.out.println(future.get());
}
```
This approach avoids unnecessary blocking and improves performance.

---

#### 6. Using ThreadPoolExecutor Directly

While `ExecutorService` (e.g., via `Executors.newFixedThreadPool()`) is a common abstraction, using `ThreadPoolExecutor` directly provides more fine-grained control over thread pool configurations. This allows you to specify parameters such as:
- Core pool size
- Maximum pool size
- Keep-alive time
- Task queue type and capacity

#### Example Usage
Below is an example demonstrating how to use `ThreadPoolExecutor` directly:

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        // Creating a ThreadPoolExecutor with custom settings
        ThreadPoolExecutor executor = new ThreadPoolExecutor(
            2, // Core pool size
            4, // Maximum pool size
            60L, // Keep-alive time
            TimeUnit.SECONDS, // Time unit
            new LinkedBlockingQueue<>() // Task queue
        );

        // Submitting a task for execution
        executor.execute(() -> System.out.println("Thread is running..."));

        // Shutdown the executor after task execution
        executor.shutdown();
    }
}
```

#### Use Case
You should use `ThreadPoolExecutor` directly when:
- You need custom thread pool behavior beyond the defaults provided by `Executors` factory methods.
- You want to fine-tune thread pool parameters for specific performance needs.
- You need to handle task rejection policies or manage queue types explicitly.

By understanding and configuring `ThreadPoolExecutor`, you can optimize resource usage and improve application performance efficiently.

---
### 7. ForkJoinPool and Recursive Task Execution


The `ForkJoinPool` is a specialized `ExecutorService` designed for parallel processing of recursive, divide-and-conquer algorithms. It is particularly useful for tasks like sorting (quicksort, merge sort) and parallel computation.

#### Key Components:
- **ForkJoinPool**: Manages and executes ForkJoinTasks in parallel.
- **RecursiveTask<T>**: A subclass of `ForkJoinTask` that returns a result.
- **RecursiveAction**: A subclass of `ForkJoinTask` that does not return a result.

#### Example: Using `ForkJoinPool` with `RecursiveTask`

The following example demonstrates how to use `ForkJoinPool` for computing a Fibonacci-like sequence using recursion.

```java
import java.util.concurrent.*;

public class MyTask extends RecursiveTask<Integer> {
    private final int number;

    public MyTask(int number) {
        this.number = number;
    }

    @Override
    protected Integer compute() {
        if (number <= 1) return number;
        MyTask task1 = new MyTask(number - 1);
        MyTask task2 = new MyTask(number - 2);
        task1.fork(); // Start task1 in parallel
        return task2.compute() + task1.join(); // Compute task2 and wait for task1
    }
}

public class Main {
    public static void main(String[] args) {
        ForkJoinPool pool = new ForkJoinPool();
        MyTask task = new MyTask(5);
        int result = pool.invoke(task); // Fibonacci-like example
        System.out.println("Result: " + result); // Output: 5
        pool.shutdown();
    }
}
```

#### Explanation
1. **RecursiveTask<Integer>** is used since we need to return an integer result.
2. **Base Case**: If `number <= 1`, return `number`.
3. **Recursive Step**:
   - Create two subtasks (`task1` and `task2`).
   - `fork()` is used to execute `task1` asynchronously.
   - `compute()` is called directly on `task2` (to avoid excessive thread creation).
   - `join()` waits for `task1` to finish and collects its result.

#### Use Cases
- **Parallel Sorting** (Merge Sort, Quick Sort)
- **Matrix Multiplication**
- **Parallelized Tree Traversals**
- **Graph Algorithms** (like Parallel BFS/DFS)
- **Large Data Processing (Recursive Computation)**

---

#### 8. Using CompletableFuture (Asynchronous Execution)

Introduced in Java 8, `CompletableFuture` provides a powerful way to handle asynchronous computations, effectively creating threads under the hood via an Executor (default is `ForkJoinPool.commonPool()`).

#### Example:

```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) throws Exception {
        CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> {
            try {
                Thread.sleep(1000); // Simulate work
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            return "Task completed!";
        });

        System.out.println(future.get()); // Waits for result
    }
}
```

#### Custom Executor Example:

```java
ExecutorService executor = Executors.newFixedThreadPool(2);
CompletableFuture<String> future = CompletableFuture.supplyAsync(() -> "Task completed!", executor);
executor.shutdown();
```

#### Use Case:
- Asynchronous programming with a functional style.
- Useful for chaining tasks and handling long-running operations without blocking the main thread.

---

### 9. Using Timer and TimerTask

The `java.util.Timer` class allows scheduling tasks to run in a background thread, either once or repeatedly.

#### Example:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {
        Timer timer = new Timer();
        TimerTask task = new TimerTask() {
            @Override
            public void run() {
                System.out.println("Task is running...");
            }
        };
        timer.schedule(task, 0, 1000); // Run every 1 second
        
        // Uncomment the following line to stop the timer after a certain time
        // timer.cancel();
    }
}
```

#### Use Case:
- Suitable for simple scheduled tasks.
- Less flexible than `ScheduledExecutorService`, but useful for lightweight task scheduling.

---
#### 10. using ScheduledExecutorService

`ScheduledExecutorService` is a more robust and flexible alternative to `Timer`. It is part of the `java.util.concurrent` package and provides methods to schedule tasks with delays or at fixed rates.

#### Why Use `ScheduledExecutorService`?
- Supports scheduling periodic or one-time tasks.
- More flexible and scalable than `Timer`.
- Handles exceptions better, ensuring that tasks continue executing even if one fails.

#### Scheduling a Task at Fixed Intervals
The following example demonstrates how to use `ScheduledExecutorService` to execute a task every second:

```java
import java.util.concurrent.*; // Import necessary package

public class Main {
    public static void main(String[] args) {
        // Create a ScheduledExecutorService with a single-threaded pool
        ScheduledExecutorService executor = Executors.newScheduledThreadPool(1);
        
        // Schedule a task to run at a fixed rate
        // The task will run immediately (0 seconds delay) and then repeat every 1 second
        executor.scheduleAtFixedRate(() -> System.out.println("Task is running..."), 
                                     0, 1, TimeUnit.SECONDS);
        
        // Uncomment the following line to stop the executor after some time
        // executor.shutdown(); // Shuts down the executor gracefully
    }
}
```

#### Key Methods of `ScheduledExecutorService`
1. `schedule(Runnable command, long delay, TimeUnit unit)`: Schedules a task to run after a delay.
2. `scheduleAtFixedRate(Runnable command, long initialDelay, long period, TimeUnit unit)`: Runs a task periodically with a fixed interval between start times.
3. `scheduleWithFixedDelay(Runnable command, long initialDelay, long delay, TimeUnit unit)`: Runs a task with a delay between the end of one execution and the start of the next.

#### Use Cases
- Running periodic background tasks (e.g., log rotation, monitoring, or polling).
- Scheduling one-time delayed tasks.
- Replacing traditional `Timer` and `TimerTask` for better control and error handling.

#### Stopping the Executor
To gracefully stop the executor, call:
```java
executor.shutdown(); // Initiates an orderly shutdown in which previously submitted tasks are executed, but no new tasks will be accepted.
```
Or, to force termination immediately:
```java
executor.shutdownNow(); // Attempts to stop all actively executing tasks and halts the processing of waiting tasks.
```

---

#### 11. using Virtual Threads in Java 21+

Java 21 introduces **virtual threads** as part of **Project Loom**. Virtual threads are lightweight and managed by the JVM instead of the OS, making them ideal for high-concurrency applications with minimal overhead.

#### Creating Virtual Threads

#### Using `Thread.ofVirtual()`
```java
public class Main {
    public static void main(String[] args) throws InterruptedException {
        // Create and start a virtual thread
        Thread virtualThread = Thread.ofVirtual().start(() -> {
            System.out.println("Virtual thread running...");
        });
        
        // Wait for the virtual thread to complete execution
        virtualThread.join();
    }
}
```

#### Using Executors (`Executors.newVirtualThreadPerTaskExecutor()`)
```java
import java.util.concurrent.*;

public class Main {
    public static void main(String[] args) {
        // Create a virtual thread executor
        try (var executor = Executors.newVirtualThreadPerTaskExecutor()) {
            // Submit a task to run on a virtual thread
            executor.submit(() -> System.out.println("Virtual thread running..."));
        } // Executor automatically closes after use
    }
}
```

#### Key Features of Virtual Threads
- **Lightweight:** JVM manages virtual threads, reducing OS thread overhead.
- **Massive Concurrency:** Enables creating millions of threads efficiently.
- **Ideal for I/O-bound Tasks:** Best suited for applications that spend most of their time waiting (e.g., web servers, database access).

#### Use Case
- High-scale web services
- Database connection handling
- Asynchronous programming models

**Note:** Virtual threads require Java 21 or later.

---