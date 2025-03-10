## 1.Why is String Immutable in Java?

String is immutable in Java for the following reasons:

1. **String Pool**: 
   - Java maintains a special memory area called the **String Pool** in the heap.
   - When a new String is created, if an identical String already exists in the pool, the existing reference is returned instead of creating a new object.
   - If Strings were mutable, modifying one reference would affect all other references, leading to incorrect behavior.

2. **Security**: 
   - Strings are used in security-sensitive areas like **network connections, database URLs, usernames, and passwords**.
   - If Strings were mutable, an attacker could alter these values, causing security vulnerabilities.

3. **Multithreading**: 
   - Since Strings are immutable, they are **thread-safe**.
   - Multiple threads can share the same String instance without synchronization, improving performance.

4. **Caching and Performance**: 
   - The **hashcode** of a String is frequently used in Java (e.g., in HashMaps).
   - Since Strings are immutable, their hashcode **does not change**, allowing efficient caching and improving performance.

5. **Class Loaders**: 
   - Strings are used by Java **ClassLoaders** to load classes dynamically.
   - Immutability ensures that the correct class is loaded, preventing security risks from modified class names.

### Conclusion
The immutability of Strings in Java improves **performance, security, thread-safety, and memory optimization**, making them a crucial part of the Java language.

## Question 2: Why is String Immutable?

String is immutable in Java for the following reasons:

1. **String Pool**: 
   - Java maintains a special memory area called the **String Pool** in the heap.
   - When a new String is created, if an identical String already exists in the pool, the existing reference is returned instead of creating a new object.
   - If Strings were mutable, modifying one reference would affect all other references, leading to incorrect behavior.

2. **Security**: 
   - Strings are used in security-sensitive areas like **network connections, database URLs, usernames, and passwords**.
   - If Strings were mutable, an attacker could alter these values, causing security vulnerabilities.

3. **Multithreading**: 
   - Since Strings are immutable, they are **thread-safe**.
   - Multiple threads can share the same String instance without synchronization, improving performance.

4. **Caching and Performance**: 
   - The **hashcode** of a String is frequently used in Java (e.g., in HashMaps).
   - Since Strings are immutable, their hashcode **does not change**, allowing efficient caching and improving performance.

5. **Class Loaders**: 
   - Strings are used by Java **ClassLoaders** to load classes dynamically.
   - Immutability ensures that the correct class is loaded, preventing security risks from modified class names.

### Conclusion
The immutability of Strings in Java improves **performance, security, thread-safety, and memory optimization**, making them a crucial part of the Java language.

---

## Question 3: What does the `equals()` method of the `String` class do?

### Answer:
- In Java, the `Object` class is the parent of all classes, and it has an `equals()` method that **compares object references**.
- However, the `String` class **overrides** the `equals()` method to compare the **contents** of two strings instead of their references.

### Example:

```java
public class StringEqualsExample {
    public static void main(String[] args) {
        String s1 = new String("Java");
        String s2 = new String("Java");

        System.out.println(s1 == s2); // false (compares references)
        System.out.println(s1.equals(s2)); // true (compares content)
    }
}
```

## Question 4: Explain the output of the below program related to the equals() method of StringBuilder.

```java
public class Demo {
    public static void main(String[] args) {
        StringBuilder sb1 = new StringBuilder("hello");
        StringBuilder sb2 = new StringBuilder("hello");

        if (sb1.equals(sb2)) {
            System.out.println("Equal");
        } else {
            System.out.println("Not Equal");
        }
    }
}
```

### Answer:
This is a common interview question. If you expected the output to be `Equal`, you were mistaken. The output of the above program is `Not Equal` because `StringBuilder` (and `StringBuffer`) does not override the `equals()` and `hashCode()` methods from `Object` class.

By default, `Object`'s `equals()` method is used, which checks for reference equality rather than content equality. Since `sb1` and `sb2` are different objects, the condition evaluates to false, resulting in `Not Equal` being printed.

### Why doesn't StringBuilder override equals and hashCode?
Hash codes are used in data structures like `HashMap`, `HashSet`, `Hashtable`, and `ConcurrentHashMap`, which rely on hash-based storage. These structures require that keys do not change once inserted, so that values can be retrieved correctly using their hash codes.

Since `StringBuilder` and `StringBuffer` are **mutable**, their hash codes would change if their content changed. This makes them a poor choice for hash-based data structures, and hence, their `equals()` and `hashCode()` methods are not overridden.

### Equals and HashCode Contract:
The contract states:
- If two objects are equal according to the `equals()` method, then their `hashCode()` must also be the same.
- The reverse is **not** necessarily true: if two objects have the same hash code, they may or may not be equal.

If `StringBuilder` had overridden `equals()`, it would also need to override `hashCode()` to maintain this contract. However, as explained earlier, there is no need for `StringBuilder` to have its own `hashCode()` implementation.

## Question 5: When to use String, StringBuffer, and StringBuilder?
- **String**: Use when immutability is required.
- **StringBuffer**: Use when mutability and thread safety are required.
- **StringBuilder**: Use when mutability is required but thread safety is not needed.

## Question 6: Explain the equals and hashCode contract
The **equals and hashCode contract** states:
1. If two objects are equal according to the `equals()` method, then their `hashCode()` must also be the same.
2. The reverse is not necessarily true: if two objects have the same hash code, they may or may not be equal.

This contract ensures that objects function correctly in hash-based collections like `HashMap` and `HashSet`.

