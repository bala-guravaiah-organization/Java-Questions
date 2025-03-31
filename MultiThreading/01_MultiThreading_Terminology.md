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
