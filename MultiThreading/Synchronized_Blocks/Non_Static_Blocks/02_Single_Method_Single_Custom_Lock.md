```java
public class SharedResource {
    private int value = 0;
    private final Object lock = new Object(); // Dedicated lock object

    public void updateValue() {
        synchronized (lock) {
            value++;
            System.out.println(Thread.currentThread().getName() + " updated value to " + value);
        }
    }
}

```
Here, `lock` is a separate object used as the `synchronization` monitor instead of this (the `SharedResource` instance itself).

### What’s Happening?

- In Java, the `synchronized` keyword requires a lock (monitor) to control access to a block of code.

- You can use any object as a lock. Commonly, people use `this` (the current object), but here, a dedicated `Object lock` is created specifically for synchronization.

- Threads must acquire the `lock` object’s monitor to enter the `synchronized` (`lock`) block, ensuring only one thread updates `value` at a time.








