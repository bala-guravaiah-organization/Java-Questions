#### Local Variable Declaration for Lambda in Java 11

#### 🔍 Overview
- Java 11 **introduces support for Local-Variable syntax** in lambda expressions.
- Although lambda parameters **infer types**, using the `var` keyword enables **annotations** like `@NotNull` or `@Nullable`.
- Example:
  ```java
  (@NotNull var str) -> "$" + str;
  ```

#### Local-Variable Type Inference Recap
- Java 10 introduced **local-variable type inference**, allowing `var` instead of explicit types.
- Example:
  ```java
  void m1() {
      var x = "ABC"; // Compiler infers String type
      var y = 10;     // Compiler infers int type
      y = 25;         // Valid
      // y = "A";    // Invalid (type cannot change)
  }
  ```
- **Rules:**
  1. **`var` requires initialization** where declared.
  2. **Cannot be used for method/constructor parameters**.
  3. **Compiler infers data type** based on assigned value.

#### `var` in Lambda Expressions (Java 11 Feature)
- Java 11 **allows `var`** in **lambda parameters**.
- Useful when **annotations are needed**.

#### ✅ Rules for Lambda Local Variables:
1. **All parameters must use `var`** (no skipping allowed).
   ```java
   (var s1, s2) -> s1 + s2; // ❌ Invalid (mixed usage)
   ```
2. **No mixing of `var` and explicit types**.
   ```java
   (var s1, String y) -> s1 + y; // ❌ Invalid
   ```
3. **Parentheses are required** when using `var` in lambda.
   ```java
   var s1 -> s1; // ❌ Invalid, needs parentheses
   (var s1) -> s1; // ✅ Valid
   ```

#### Example Usage:
```java
import java.util.*;

public class LambdaVarExample {
    public static void main(String[] args) {
        List<String> list = Arrays.asList("Java", "11", "Lambda");
        list.forEach((var str) -> System.out.println(str.toUpperCase()));
    }
}
```

#### ✅ Expected Output:
```
JAVA
11
LAMBDA
```

#### 🚀 Why Use `var` in Lambda?
- **Allows annotations** on lambda parameters.
- **Consistent syntax** with Java 10’s local-variable inference.
- **Improves readability** in some cases.
