#### Explain Garbage Collection in Java and its benifits?

Garbage Collection (GC) in Java is an automatic memory management process where the Java Virtual Machine (JVM) reclaims memory occupied by objects that are no longer in use. This helps in preventing memory leaks and optimizing application performance.

#### Benefits of Garbage Collection in Java

1. **Automatic Memory Management**  
   - Java handles memory deallocation automatically using GC, reducing manual intervention.

2. **Prevention of Memory Leaks**  
   - Unused objects are identified and removed, ensuring optimal memory usage.

3. **Improved Performance**  
   - JVM optimizes GC execution to maintain application responsiveness.

4. **No Manual Deallocation**  
   - Unlike C/C++, Java does not require explicit `free()` or `delete()` calls.

5. **Enhances Security & Stability**  
   - Prevents issues like dangling pointers and memory corruption.

#### How Garbage Collection Works in Java
1. **GC identifies unreachable objects** – Objects with no references become eligible for GC.
2. **GC removes those objects** – JVM reclaims memory by cleaning up unused objects.
3. **Memory is reused** – The freed-up memory is allocated for new objects.

#### Requesting Garbage Collection in Java
Although GC runs automatically, you can request it manually using:

```java
System.gc(); // Suggests JVM to run GC (Not guaranteed)
Runtime.getRuntime().gc(); // Alternative way to request GC
```

#### Best Practices to Optimize Garbage Collection
- Use **WeakReference** for cache objects.
- Set unused objects to `null` explicitly if they hold large memory.
- Monitor GC performance using **JVisualVM** or **Garbage Collection Logs**.
- Choose an appropriate **GC algorithm** based on your application’s needs.

---

#### Conclusion
Garbage Collection in Java simplifies memory management by automatically reclaiming unused memory. Understanding different GC algorithms and JVM memory areas helps in optimizing performance for large-scale applications.

#### Example of `System.gc();`
The following example demonstrates how Java garbage collection works when objects are no longer referenced:

```java
class DemoGC {
    @Override
    protected void finalize() throws Throwable {
        System.out.println("Garbage collected object: " + this);
    }
}

public class GarbageCollectionExample {
    public static void main(String[] args) {
        DemoGC obj1 = new DemoGC();
        DemoGC obj2 = new DemoGC();
        
        // Removing references
        obj1 = null;
        obj2 = null;
        
        //Suggests the Java Virtual Machine (JVM) to run the Garbage Collector (GC), but it does not guarantee that GC will execute immediately.
        System.gc();
        
        // Giving time for GC to complete
        try {
            Thread.sleep(1000);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
    }
}
```

#### Explanation:
1. We create two objects (`obj1` and `obj2`) of the `DemoGC` class.
2. Both objects are set to `null`, making them eligible for garbage collection.
3. `System.gc();` requests garbage collection, though execution is not guaranteed.
4. The `finalize()` method is overridden to print a message when an object is garbage collected.
5. `Thread.sleep(1000);` ensures enough time for GC to run before the program exits.

#### Best Practices to Optimize Garbage Collection
- Use **WeakReference** for cache objects.
- Set unused objects to `null` explicitly if they hold large memory.
- Monitor GC performance using **JVisualVM** or **Garbage Collection Logs**.
- Choose an appropriate **GC algorithm** based on your application’s needs.

#### Conclusion
Garbage Collection in Java simplifies memory management by automatically reclaiming unused memory. Understanding different GC algorithms and JVM memory areas helps in optimizing performance for large-scale applications.

---

#### What does System.gc(); do?
```java
System.gc(); // Suggests JVM to run GC (Not guaranteed)
```
**suggests** the Java Virtual Machine (JVM) to run the Garbage Collector (GC), but it does not guarantee that GC will execute immediately.

#### What Happens When System.gc(); is Called?
- Request Sent to JVM: The call requests the JVM to perform garbage collection.
- JVM Decides Execution: The JVM may choose to ignore this request or execute it when it finds it necessary.
- Eligible Objects Are Collected: If the JVM runs the GC, it will clean up objects that are no longer referenced (like obj1 and obj2 in the example).
- Finalize Method May Run: If an object is garbage collected, its finalize() method (if overridden) will execute before the object is destroyed.

```java 
Example Output:
Garbage collected object: DemoGC@1d44bcfa
Garbage collected object: DemoGC@266474c2

```
---
