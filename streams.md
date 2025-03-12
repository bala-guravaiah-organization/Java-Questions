## **What is stream Processing?**
Stream processing is a method of handling data as a continuous sequence of elements. Instead of processing data in bulk, it allows for efficient and real-time operations.

### **Key Concepts:**
1. **Definition of Stream Processing**  
   - A **stream** is a sequence of data items that are processed one at a time.

2. **Data Flow in Streams**  
   - Programs can **read data** from an **input stream** one item at a time.  
   - Programs can **write data** to an **output stream** similarly.  

3. **Chaining Streams**  
   - The **output stream** of one program can serve as the **input stream** of another.  
---

### **What Are Streams?**
Streams are an update to the Java API that allow you to manipulate collections of data in a **declarative way**. Instead of coding an **ad hoc implementation**, you express a **query** to process the data efficiently.

you can think of streams as **fancy iterators** over a collection of data.


# Java 7 vs Java 8: Stream Processing

## **Java 7 Approach**
In Java 7, filtering, sorting, and mapping a list required explicit loops and anonymous classes.

### **Code Example (Java 7)**
### **Dish Class**
```java
public class Dish {
    private String name;
    private int calories;
    
    public Dish(String name, int calories) {
        this.name = name;
        this.calories = calories;
    }
    
    public String getName() {
        return name;
    }
    
    public int getCalories() {
        return calories;
    }
}
```
```java
import java.util.*;

public class Java7Example {
    public static void main(String[] args) {
        List<Dish> lowCaloricDishes = new ArrayList<>();
        
        // Filtering dishes with calories < 400
        for (Dish dish : menu) {
            if (dish.getCalories() < 400) {
                lowCaloricDishes.add(dish);
            }
        }
        
        // Sorting dishes based on calories
        Collections.sort(lowCaloricDishes, new Comparator<Dish>() {
            public int compare(Dish dish1, Dish dish2) {
                return Integer.compare(dish1.getCalories(), dish2.getCalories());
            }
        });
        
        // Extracting names of the sorted dishes
        List<String> lowCaloricDishesName = new ArrayList<>();
        for (Dish dish : lowCaloricDishes) {
            lowCaloricDishesName.add(dish.getName());
        }
    }
}
```
### **Steps in Java 7 Approach:**
1. **Filter** dishes with calories below 400.
2. **Sort** them using an anonymous class.
3. **Map** them to their names manually.

---

### **Java 8 Approach (Using Streams)**
Java 8 introduced the **Stream API**, which simplifies filtering, sorting, and mapping operations.

### **Code Example (Java 8)**
```java
import static java.util.Comparator.comparing;
import static java.util.stream.Collectors.toList;

public class Java8Example {
    public static void main(String[] args) {
        List<String> lowCaloricDishesName = menu.stream()
                .filter(d -> d.getCalories() < 400)   // Filter dishes below 400 calories
                .sorted(comparing(Dish::getCalories)) // Sort by calories
                .map(Dish::getName)                   // Extract dish names
                .collect(toList());                   // Store results in a list
    }
}
```

### **Advantages of Java 8 Streams:**
1. **Declarative approach** – No need for explicit loops. Specify what you want to acheive as oppsed to implement an operation(using for loop and if blocks).
2. **Improved readability** – Code is more concise and expressive.
3. **Better performance** – Streams can be parallelized easily.
4. **Reusable and composable** – Functional-style operations allow easy modifications.

---

### **What are the differences between streams and collections**
### **Key Differences:**

| Feature           | Collections | Streams |
|------------------|------------|---------|
| **Data Storage** | Stores all elements in memory | Computes elements on demand |
| **Traversal** | Can be iterated multiple times | Can be traversed only once |
| **Performance** | Eager evaluation | Lazy evaluation, efficient for large data processing |
| **Modification** | Allows adding/removing elements | Immutable (cannot modify the source) |
| **Parallel Processing** | Requires manual handling (e.g., ForkJoinPool) | Easily parallelized using `parallelStream()` |

### **Real-World Example:**
- A **collection** is like a DVD, where all data is stored before watching.
- A **stream** is like a video being streamed online, where frames are loaded only when needed.

```java
List<String> title = Arrays.asList("Modern", "Java", "In", "Action");
Stream<String> s = title.stream();
s.forEach(System.out::println); // Prints each word
s.forEach(System.out::println); // Throws IllegalStateException
```

💡 **Note:** A stream is **consumed** once and cannot be reused.

---

### what is External vs. Internal Iteration in Java ?

### **1. External Iteration (Java 7 and Earlier)**

### **Definition:**
External iteration is when the programmer explicitly controls how data is iterated, typically using loops or iterators.

### **Example: Using for-each Loop (Java 7)**
```java
List<String> names = new ArrayList<>();
for (Dish dish : menu) {                  
    names.add(dish.getName());    
}
```
✅ **Explicitly iterates over `menu` and extracts names manually.**

### **Example: Using Iterator (Java 7)**
```java
List<String> names = new ArrayList<>();
Iterator<Dish> iterator = menu.iterator();
while (iterator.hasNext()) {            
    Dish dish = iterator.next();
    names.add(dish.getName());
}
```
✅ **Manually manages iteration using an iterator.**

### **Disadvantages of External Iteration:**
- **Verbose**: Requires manual iteration, making code longer.
- **Difficult to parallelize**: Cannot easily leverage multi-core processors.
- **Error-prone**: More chances for mistakes in iteration logic.

---

### **2. Internal Iteration (Java 8 Streams)**

### **Definition:**
Internal iteration delegates iteration logic to the **Stream API**, making code more declarative and concise.

### **Example: Using Streams (Java 8)**
```java
List<String> names = menu.stream()
                         .map(Dish::getName)  
                         .collect(Collectors.toList());   
```
✅ **Streams handle iteration automatically, reducing boilerplate code.**

### **Advantages of Internal Iteration:**
- **Concise & Readable**: No need for explicit loops.
- **Optimized Execution**: Java handles iteration efficiently.
- **Easier Parallel Processing**: Just use `.parallelStream()`.

---


### **3. Comparison Table: External vs. Internal Iteration**

| Feature               | External Iteration (Java 7) | Internal Iteration (Java 8) |
|----------------------|----------------------|----------------------|
| **Control**         | Programmer controls iteration | Java manages iteration |
| **Code Complexity** | More verbose, requires loops | Concise and expressive |
| **Performance**     | Slower, manually optimized | Faster, optimized by JVM |
| **Parallel Processing** | Hard to implement | Built-in ith `.parallelStream()` |
| **Readability**     | Imperative and lengthy | Functional and declarative |

---
### What are Java 8 Stream Operations

The `Stream` interface in `java.util.stream` defines many operations, classified into two categories:
- **Intermediate Operations**: These return another stream and allow method chaining.
- **Terminal Operations**: These produce a final result or side effect and close the stream.
---

### **Intermediate Operations**
Intermediate operations return another stream and are **lazy**, meaning they are not executed until a terminal operation is invoked.

### **Example of Intermediate Operations**
```java
List<String> names = menu.stream()
        .filter(dish -> {
            System.out.println("Filtering: " + dish.getName());
            return dish.getCalories() > 300;
        })
        .map(dish -> {
            System.out.println("Mapping: " + dish.getName());
            return dish.getName();
        })
        .limit(3)
        .collect(toList());
```
#### **Output:**
```
Filtering: pork
Mapping: pork
Filtering: beef
Mapping: beef
Filtering: chicken
Mapping: chicken
[pork, beef, chicken]
```
- Only the first three dishes are selected due to **short-circuiting**.
---

### **Terminal Operations**
Terminal operations consume the stream and produce a final result, such as a `List`, `Integer`, or even `void`.

### **Example:**
```java
long count = menu.stream()
                 .filter(dish -> dish.getCalories() > 300)
                 .distinct()
                 .limit(3)
                 .count(); // Terminal operation
```
---
### What are the Differences Between Intermediate and Terminal Operations

| Feature               | Intermediate Operations        | Terminal Operations          |
|-----------------------|--------------------------------|------------------------------|
| **Definition**       | Transforms a stream into another stream | Produces a result or side-effect and consumes the stream |
| **Execution**       | **Lazy** (Executed only when a terminal operation is called) | **Eager** (Triggers stream processing immediately) |
| **Return Type**      | Returns another `Stream<T>` (supports chaining) | Returns a non-stream value (like `List<T>`, `int`, `boolean`, etc.) |
| **Effect on Stream** | Does not terminate the stream | Terminates the stream (cannot reuse it) |
| **Examples**        | `filter()`, `map()`, `sorted()`, `distinct()`, `limit()`, `skip()` | `forEach()`, `collect()`, `reduce()`, `count()`, `min()`, `max()`, `anyMatch()` |
| **Execution Order** | Deferred until a terminal operation is encountered | Executes immediately when invoked |
| **Usage**           | Used for data transformation | Used to get final results from the stream |

---

### **Comparison: Intermediate vs. Terminal Operations**

### **Table 1: Intermediate Operations**
| Operation | Type | Return Type | Function Descriptor |
|-----------|------|-------------|---------------------|
| `filter`  | Intermediate | `Stream<T>` | `Predicate<T>` → `boolean` |
| `map`     | Intermediate | `Stream<R>` | `Function<T, R>` → `R` |
| `limit`   | Intermediate | `Stream<T>` | - |
| `sorted`  | Intermediate | `Stream<T>` | `Comparator<T>` → `(T, T) -> int` |
| `distinct` | Intermediate | `Stream<T>` | - |

### **Table 2: Terminal Operations**
| Operation | Type | Return Type | Purpose |
|-----------|------|-------------|---------|
| `forEach` | Terminal | `void` | Consumes each element and applies a lambda function. |
| `count`   | Terminal | `long` | Returns the number of elements in the stream. |
| `collect` | Terminal | Generic (`List`, `Map`, `Integer`, etc.) | Reduces the stream into a collection. |

---




