#### Predicate Interface Changes in Java 11

#### 🔍 The `not()` Method
- Java 11 introduced a **static** `not()` method in the `Predicate` interface.
- This method is used to **negate a Predicate** (i.e., reverse its logic).
- It **simplifies** the process of negating conditions compared to the older `negate()` method.
- `Predicate.not()` can be **used with method references**, making code cleaner and more readable.

#### Syntax:
```java
static <T> Predicate<T> not(Predicate<? super T> target)
```
- Takes a **Predicate** as input and returns a **negated Predicate** as output.

---

#### ⚡ Before Java 11: Negating Predicate Using `negate()`
```java
import java.util.function.Predicate;

public class PredicateNegateExample {
    public static void main(String[] args) {
        Predicate<String> isNotEmpty = s -> !s.isEmpty();
        Predicate<String> isEmpty = isNotEmpty.negate();
        
        System.out.println(isEmpty.test("Hello")); // false
        System.out.println(isEmpty.test("")); // true
    }
}
```
#### Output:
```
false
true
```

---

#### ⚡ Java 11: Using `Predicate.not()`
```java
import java.util.function.Predicate;
import java.util.List;
import java.util.stream.Collectors;

public class PredicateNotExample {
    public static void main(String[] args) {
        List<String> names = List.of("Java", "", "Spring", "");
        
        // Filter out empty strings using Predicate.not()
        List<String> nonEmptyNames = names.stream()
                .filter(Predicate.not(String::isEmpty))
                .collect(Collectors.toList());
        
        System.out.println(nonEmptyNames); // [Java, Spring]
    }
}
```
#### Output:
```
[Java, Spring]
```

---

#### 🚀 Why Use `Predicate.not()`?
✅ **More Readable** → No need to call `negate()` manually.
✅ **Method References** → Works seamlessly with method references like `String::isEmpty`.
✅ **Cleaner Streams** → Simplifies filtering logic in Stream API.

---