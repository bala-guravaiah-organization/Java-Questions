#### Java 11 String Enhancements

Java 11 introduced several useful methods for the `String` class to simplify common operations.

#### `repeat(int count)`
- Returns a new string where the original string is repeated **`count`** times.
- If a **negative value** is provided, it throws an `IllegalArgumentException`.
- If the resulting string length exceeds the maximum allowed size, it may throw an **OutOfMemoryError**.

#### Example:
```java
public class StringRepeatExample {
    public static void main(String[] args) {
        String str = "Hello ";
        System.out.println(str.repeat(3));
    }
}
```
**Output:**
```
Hello Hello Hello
```

---

#### `isBlank()`
- Returns `true` if the string is empty or contains **only whitespace characters**.

#### Example:
```java
public class IsBlankExample {
    public static void main(String[] args) {
        String str1 = "";
        String str2 = "   ";
        String str3 = "Hello";

        System.out.println(str1.isBlank()); // true
        System.out.println(str2.isBlank()); // true
        System.out.println(str3.isBlank()); // false
    }
}
```

---

#### `strip()`
- Works like `trim()`, **removes leading and trailing spaces**, but it is **Unicode-aware**.

#### Example:
```java
public class StripExample {
    public static void main(String[] args) {
        String str = "  Hello World  ";
        System.out.println("|" + str.strip() + "|");
    }
}
```
**Output:**
```
|Hello World|
```

---

#### `stripLeading()`
- Removes **only leading spaces** from the string.

#### Example:
```java
public class StripLeadingExample {
    public static void main(String[] args) {
        String str = "   Java 11";
        System.out.println("|" + str.stripLeading() + "|");
    }
}
```
**Output:**
```
|Java 11|
```
---

#### `stripTrailing()`
- Removes **only trailing spaces** from the string.

#### Example:
```java
public class StripTrailingExample {
    public static void main(String[] args) {
        String str = "Java 11   ";
        System.out.println("|" + str.stripTrailing() + "|");
    }
}
```
**Output:**
```
|Java 11|
```
#### Difference Between `strip()` and `trim()`

#### Key Differences
| Method   | Behavior |
|----------|----------|
| **`trim()`**  | Removes only characters **≤ U+0020 (space)**. |
| **`strip()`** | Removes **all Unicode whitespace characters**, following the Unicode standard. |

---

#### 🔍 Detailed Explanation
- The **`trim()`** method has existed since early Java versions when Unicode **was not fully standardized**.
- The **`strip()`** method uses `Character.isWhitespace()`, which works with **Unicode code points**.
- **`strip()` is recommended** because it follows the **Unicode standard** and removes a broader range of whitespace characters.

**Reference:** [JDK-8200373](https://bugs.openjdk.org/browse/JDK-8200373)

---

#### Example Comparison
```java
public class StripVsTrimExample {
    public static void main(String[] args) {
        String str = "\u2003Hello World\u2003"; // Includes Unicode whitespace
        System.out.println("|" + str.trim() + "|");  // May not remove Unicode spaces
        System.out.println("|" + str.strip() + "|"); // Removes all Unicode whitespace
    }
}
```

#### ✅ Output:
```
| Hello World |  // trim() does not remove Unicode spaces
|Hello World|   // strip() removes all Unicode whitespace
```

---

#### 🚀 Conclusion
- **Use `strip()` for modern Unicode-aware whitespace removal.**
- **Use `trim()` only if working strictly with ASCII spaces (≤ U+0020).**
---
#### Java 11 `lines()` Method

#### 🔍 What is `lines()`?
- The `lines()` method **splits a string into multiple lines** and returns them as a **Stream**.
- It automatically recognizes different types of line separators:
  - **Line feed** (`"\n"`, U+000A)
  - **Carriage return** (`"\r"`, U+000D)
  - **Carriage return + line feed** (`"\r\n"`, U+000D U+000A)

---

#### Example Usage
```java
import java.util.stream.Stream;

public class LinesExample {
    public static void main(String[] args) {
        String multiLineString = "Hello\nWorld\rJava\r\n11";

        Stream<String> lines = multiLineString.lines();
        lines.forEach(System.out::println);
    }
}
```

#### ✅ Expected Output:
```
Hello
World
Java
11
```
---

#### 🚀 Why Use `lines()`?
- **Easier** way to process multiline strings.
- **More efficient** than manually splitting strings using `split("\\n")`.
- **Works with different newline formats** automatically.
---

#### 🔍 Collection Enhancements in Java 11
#### 🆕 `toArray()` Method
- Java 11 introduced a new **default method** `toArray()` in the **Collection** interface.
- This method uses a **functional interface** to convert collections into arrays in a more flexible way.

#### Example Usage
```java
import java.util.*;

public class ToArrayExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Java", "11", "Features");
        String[] array = list.toArray(String[]::new);
        System.out.println(Arrays.toString(array));
    }
}
```

#### ✅ Expected Output:
```
[Java, 11, Features]
```

#### 🚀 Why Use `toArray()`?
- **More concise** and avoids manual array creation.
- **Works with different collection types** flexibly.
- **Eliminates type casting issues** in traditional `toArray()` methods.
---