### Strongly Encapsulated JDK Internals in Java 17

In Java 17, internal JDK APIs are **fully encapsulated**, meaning that direct reflection on them is blocked unless explicitly allowed. This improves security, maintainability, and encourages the use of official Java APIs.

---

#### Example 1: Accessing `Unsafe` API (Fails in Java 17)

#### **Code Without Opening Modules**
```java
import sun.misc.Unsafe;
import java.lang.reflect.Field;

public class UnsafeExample {
    public static void main(String[] args) throws Exception {
        Field unsafeField = Unsafe.class.getDeclaredField("theUnsafe");
        unsafeField.setAccessible(true);
        Unsafe unsafe = (Unsafe) unsafeField.get(null);
        
        System.out.println("Unsafe instance: " + unsafe);
    }
}
```

#### **Expected Output in Java 17**
```
Exception in thread "main" java.lang.reflect.InaccessibleObjectException:
Unable to make field private static final sun.misc.Unsafe sun.misc.Unsafe.theUnsafe accessible:
module java.base does not "opens sun.misc" to unnamed module
```

#### **Solution: Using `--add-opens` to Bypass Encapsulation**
To make this work, run it with:
```sh
java --add-opens java.base/sun.misc=ALL-UNNAMED UnsafeExample
```
This allows reflection access to the `sun.misc` package.

---

### Example 2: Replacing `Unsafe` with `VarHandle` (Recommended)
Instead of `Unsafe`, Java 9+ provides `VarHandle`, which is an official API.

#### **Code Using `VarHandle` Instead of `Unsafe`**
```java
import java.lang.invoke.MethodHandles;
import java.lang.invoke.VarHandle;

public class VarHandleExample {
    private volatile int value = 42; // Volatile for atomic access

    public static void main(String[] args) throws Exception {
        VarHandleExample obj = new VarHandleExample();
        
        // Get VarHandle for 'value' field
        VarHandle varHandle = MethodHandles.lookup()
                .findVarHandle(VarHandleExample.class, "value", int.class);

        // Read original value
        System.out.println("Original Value: " + varHandle.getVolatile(obj));

        // Atomically update value
        varHandle.setVolatile(obj, 100);
        System.out.println("Updated Value: " + varHandle.getVolatile(obj));
    }
}
```

#### **Expected Output**
```
Original Value: 42
Updated Value: 100
```

---

### Example 3: Replacing `sun.reflect.Reflection.getCallerClass()`
`sun.reflect.Reflection.getCallerClass()` was used to determine the calling class, but it was removed in Java 17.

#### **Code Using `getCallerClass()` (Fails in Java 17)**
```java
import sun.reflect.Reflection;

public class ReflectionExample {
    public static void main(String[] args) {
        Class<?> callerClass = Reflection.getCallerClass();
        System.out.println("Caller Class: " + callerClass.getName());
    }
}
```
#### **Expected Output in Java 17**
```
Error: java.lang.NoClassDefFoundError: sun/reflect/Reflection
```

#### **Solution: Using `StackWalker` (Recommended)**
Instead of `Reflection.getCallerClass()`, use `StackWalker`, introduced in Java 9.

#### **Code Using `StackWalker`**
```java
import java.lang.StackWalker;

public class StackWalkerExample {
    public static void main(String[] args) {
        Class<?> callerClass = StackWalker.getInstance()
                                          .walk(frames -> frames.skip(1).findFirst().get().getDeclaringClass());

        System.out.println("Caller Class: " + callerClass.getName());
    }
}
```
#### **Expected Output**
```
Caller Class: StackWalkerExample
```

---

#### Key Takeaways

| Feature                      | `sun.misc.Unsafe` (Blocked in Java 17) | `VarHandle` (Recommended) |
|-----------------------------|--------------------------------------|--------------------------|
| Accessibility               | Restricted by strong encapsulation  | Public API (Java 9+)    |
| Security                    | Unsafe and prone to breaking        | Safe and future-proof   |
| JVM Options                 | Needs `--add-opens`                 | No extra setup needed   |

| Feature                      | `sun.reflect.Reflection` (Blocked in Java 17) | `StackWalker` (Recommended) |
|-----------------------------|----------------------------------------------|----------------------------|
| Accessibility               | Fully removed from JDK                      | Public API (Java 9+)       |
| Security                    | Unsafe due to reflection                    | Safe and future-proof      |
| Compatibility               | Not available in Java 17+                   | Works in Java 9+           |

For better security and maintainability, it is recommended to migrate from `sun.misc.Unsafe` to `VarHandle` and from `Reflection.getCallerClass()` to `StackWalker`.

---
