### Pattern Matching for Switch in Java

Pattern Matching for `switch` is a feature in Java that enhances the `switch` statement by allowing type patterns. This improves readability, reduces boilerplate code, and simplifies type checks.

#### Key Features
1. **Type Checking and Casting in One Step**  
   - No need for explicit casting inside the `case` block.
2. **Guarded Patterns (`when condition`)**  
   - Allows adding conditions inside `case` statements.
3. **Null Handling**  
   - Java 21 allows `case null` inside a `switch`.
4. **More Readable and Structured Code**  
   - Reduces complex `if-else` chains.

#### Example Code
Below is a Java program demonstrating Pattern Matching for `switch`:

```java
class PatternMatchingExample {
    static void process(Object obj) {
        switch (obj) {
            // Case 1: Matches when 'obj' is a String and binds it to variable 's'
            case String s when s.length() > 5:
                System.out.println("Long String: " + s);
                break;

            // Case 2: Matches when 'obj' is an Integer and binds it to variable 'i'
            case Integer i when i > 100:
                System.out.println("Large Integer: " + i);
                break;

            // Case 3: Explicitly handling 'null' (Introduced in Java 21)
            case null:
                System.out.println("Null value");
                break;

            // Default case for handling any other data type
            default:
                System.out.println("Unknown type");
        }
    }

    public static void main(String[] args) {
        process("HelloWorld");  // Matches the first case (String > 5 chars)
        process(150);           // Matches the second case (Integer > 100)
        process(null);          // Matches the 'null' case
        process(50.5);          // Falls to default case (double type)
    }
}
```

#### Explanation
- **Type Patterns (`case String s` / `case Integer i`)**
  - Automatically checks the type and assigns a variable (`s`, `i`) without explicit casting.
- **Guarded Patterns (`when condition`)**
  - Adds additional conditions inside the case (e.g., `s.length() > 5` ensures only long strings match).
- **`case null` Handling**
  - Java 21 allows `case null` inside a `switch`, making null checks explicit.
- **Default Case (`default`)**
  - Catches all unmatched cases (like other data types).

#### Expected Output
```
Long String: HelloWorld
Large Integer: 150
Null value
Unknown type
```

### Benefits
✅ **Less Boilerplate:** No need for explicit casting  
✅ **More Readable:** Cleaner and structured  
✅ **Null Handling:** `case null` is explicitly supported  
✅ **Better Performance:** Eliminates unnecessary `if-else` chains  

---

### How does Pattern Matching for `switch` differ from the `traditional switch` statement?


Java's **Pattern Matching for `switch`** enhances the traditional `switch` statement by allowing type patterns, eliminating explicit type casting, and supporting additional conditions inside `case` labels. This makes code more readable and efficient.

---

### 1. Traditional `switch` Statement
#### **Limitations:**
- Works only with **primitive types** (`int`, `char`, etc.), `String`, and `enum`.
- Requires explicit type casting for objects.
- No built-in support for conditions inside `case` labels.

#### **Example (Before Java 17)**
```java
static void process(Object obj) {
    if (obj instanceof String) {
        String s = (String) obj;  // Explicit casting required
        System.out.println("String: " + s.length());
    } else if (obj instanceof Integer) {
        Integer i = (Integer) obj;  // Explicit casting required
        System.out.println("Integer: " + i);
    } else {
        System.out.println("Unknown type");
    }
}
```

---

### 2. Pattern Matching for `switch` (Java 17+)
#### **Enhancements:**
- Works with **objects of different types**, not just primitives.
- Eliminates the need for explicit casting.
- Supports **guarded patterns (`when condition`)** for additional checks.
- Supports **`null` handling** inside `switch`.

#### **Example (Java 21)**
```java
static void process(Object obj) {
    switch (obj) {
        case String s when s.length() > 5:  // No explicit casting required
            System.out.println("Long String: " + s);
            break;
        case Integer i when i > 100:
            System.out.println("Large Integer: " + i);
            break;
        case null:  // Explicitly handling null
            System.out.println("Null value");
            break;
        default:
            System.out.println("Unknown type");
    }
}
```

---

### 3. Key Differences
| Feature                     | Traditional `switch`    | Pattern Matching for `switch` |
|-----------------------------|------------------------|--------------------------------|
| **Supports objects**         | ❌ No (only primitives, `String`, `enum`) | ✅ Yes (Any object type) |
| **Type checking & casting**  | ❌ Manual (`instanceof` + cast) | ✅ Automatic pattern matching |
| **Guards (`when condition`)** | ❌ Not supported | ✅ Supported for conditions |
| **`null` handling**          | ❌ Null causes `NullPointerException` | ✅ `case null` is explicitly handled |
| **Code readability**         | ❌ More verbose | ✅ More concise |

---

### 4. Which One to Use?
- Use **traditional `switch`** if working with **primitive values** or `enum`.
- Use **pattern matching for `switch`** when handling **multiple object types dynamically**.

---
### What are the advantages of using Pattern Matching in switch over `instanceof` checks?

Pattern Matching in `switch` provides a more readable and efficient alternative to `instanceof` checks in Java. It eliminates explicit type casting, improves code structure, and offers better performance.

---

#### **1. Eliminates Explicit Type Casting**
#### **Traditional Approach (`instanceof` + Casting):**
```java
static void process(Object obj) {
    if (obj instanceof String) {
        String s = (String) obj;  // Explicit casting required
        System.out.println("String length: " + s.length());
    } else if (obj instanceof Integer) {
        Integer i = (Integer) obj;  // Explicit casting required
        System.out.println("Integer value: " + i);
    }
}
```

#### **Pattern Matching in `switch` (Java 17+):**
```java
static void process(Object obj) {
    switch (obj) {
        case String s -> System.out.println("String length: " + s.length()); // No casting needed
        case Integer i -> System.out.println("Integer value: " + i);
        default -> System.out.println("Unknown type");
    }
}
```
✅ **Advantage:** Eliminates redundant type casting, making the code cleaner and safer.

---

#### **2. More Readable and Concise**
- **`switch` makes the structure clearer** compared to multiple `if-else` conditions.
- **No need for repeated `instanceof` checks** and separate type casting.

✅ **Advantage:** Improves readability and reduces boilerplate code.

---

#### **3. Supports Guarded Patterns (`when condition`)**
Pattern Matching in `switch` allows additional conditions **inside** `case` labels.

#### **Example (Java 21):**
```java
static void process(Object obj) {
    switch (obj) {
        case String s when s.length() > 5 -> System.out.println("Long String: " + s);
        case Integer i when i > 100 -> System.out.println("Large Integer: " + i);
        default -> System.out.println("Unknown type");
    }
}
```
✅ **Advantage:** Allows filtering values **inside** `case` statements instead of adding extra `if` checks.

---

#### **4. Handles `null` Gracefully**
In traditional `instanceof` checks, a `null` value causes a `NullPointerException`.

#### **Traditional Approach (Throws Exception):**
```java
if (obj instanceof String) {  // Throws NullPointerException if obj is null
    String s = (String) obj;
    System.out.println(s.length());
}
```

#### **Pattern Matching in `switch` (Java 21) Handles `null` Explicitly:**
```java
switch (obj) {
    case null -> System.out.println("Null value"); // Explicitly handled
    case String s -> System.out.println("String: " + s);
    default -> System.out.println("Unknown type");
}
```
✅ **Advantage:** Safer handling of `null`, reducing the risk of `NullPointerException`.

---

### **5. Improved Performance**
Pattern Matching optimizes **type checks and casting** at the compiler level, making it more efficient than multiple `if-else` checks.

✅ **Advantage:** **Better performance** due to compiler optimizations.

---

### **Comparison Table**
| Feature                     | `instanceof` Checks | Pattern Matching in `switch` |
|-----------------------------|---------------------|-----------------------------|
| **Type Checking**           | ✅ Supported       | ✅ Supported               |
| **Explicit Casting Required** | ✅ Yes (Manual)   | ❌ No (Automatic)          |
| **Code Readability**        | ❌ Verbose         | ✅ More Concise            |
| **Guarded Conditions (`when`)** | ❌ Not supported | ✅ Supported               |
| **Handles `null`**          | ❌ Causes Exception | ✅ Explicitly Handled       |
| **Performance**             | ❌ Slower (More `if-else`) | ✅ Optimized by Compiler |

---

#### **Conclusion**
Pattern Matching in `switch` is a **better alternative** to `instanceof` checks because:
✔ It eliminates **explicit type casting**  
✔ It improves **code readability and structure**  
✔ It allows **guarded conditions (`when`)**  
✔ It **handles `null` safely**  
✔ It **performs better** than multiple `if-else` checks  

**If you're using Java 17+, Pattern Matching in `switch` is the recommended approach!** 🚀

---

### Can You Use Pattern Matching for Switch with Primitive Data Types?

#### **No, Pattern Matching for `switch` Does Not Work with Primitives**
Pattern Matching for `switch` in Java only works with **reference types (objects)** and **does not support primitive types** like `int`, `double`, or `char`.

---

#### **Why Doesn't It Work with Primitives?**
1. **Pattern Matching relies on Object Types** – Primitive types don’t have an `instanceof` relationship.
2. **Switch already supports primitives natively** – Traditional `switch` can efficiently handle `int`, `char`, etc.

---

#### **Valid Example: Using Pattern Matching in `switch`**
```java
static void process(Object obj) {
    switch (obj) {
        case String s -> System.out.println("String: " + s.length());
        case Integer i -> System.out.println("Integer: " + i);
        default -> System.out.println("Unknown type");
    }
}
```
✅ **Works because `String` and `Integer` are objects**.

---

#### **Invalid Example: Using Pattern Matching with a Primitive**
```java
static void process(int num) {
    switch (num) {   // ❌ Error: Cannot use pattern matching with primitives
        case Integer i -> System.out.println("Number: " + i);
        default -> System.out.println("Unknown type");
    }
}
```
❌ **Compilation Error:** Pattern Matching does not support primitives.

---

#### **Workaround: Convert Primitives to Objects**
If you need to use `switch` with pattern matching, **wrap the primitive in its wrapper class**:
```java
static void process(Number num) {
    switch (num) {
        case Integer i -> System.out.println("Integer: " + i);
        case Double d -> System.out.println("Double: " + d);
        default -> System.out.println("Unknown Number Type");
    }
}
```
✅ **Works because `Integer` and `Double` are objects.**

---

#### **Conclusion**
- ❌ **Pattern Matching does NOT work with primitive types** (`int`, `double`, `char`, etc.).
- ✅ **It only works with reference types** (`String`, `Integer`, `Double`, custom classes, etc.).
- ✅ **Workaround:** Convert primitives to their wrapper classes (`Integer`, `Double`, etc.) if needed.

**Use Pattern Matching for objects, and use traditional `switch` for primitives!** 🚀

---

### What Happens If No Case Matches in a Pattern Matching `switch`?

#### **1. Default Case is Executed (Recommended)**
If no pattern matches, **the `default` case executes**, providing a fallback for unexpected inputs.

#### **Example: Handling Unknown Types**
```java
static void process(Object obj) {
    switch (obj) {
        case String s -> System.out.println("String: " + s);
        case Integer i -> System.out.println("Integer: " + i);
        default -> System.out.println("Unknown type");
    }
}

public static void main(String[] args) {
    process(3.14); // No matching case, so 'default' executes
}
```
**Output:**
```
Unknown type
```
✅ **Best Practice**: Always include `default` to prevent runtime errors.

---

#### **2. Throws `MatchException` if `default` is Missing**
If no `case` matches and **no `default` case is provided**, Java throws a `MatchException` (Java 21+).

#### **Example: No Default Case**
```java
static void process(Object obj) {
    switch (obj) {
        case String s -> System.out.println("String: " + s);
        case Integer i -> System.out.println("Integer: " + i);
    }
}

public static void main(String[] args) {
    process(3.14); // No matching case and no default case
}
```
**Runtime Exception:**
```
Exception in thread "main" java.lang.MatchException: No match found for case 3.14
```
❌ **Avoid this**: Always add a `default` case unless you're sure all cases are covered.

---

#### **3. Ensuring Exhaustiveness for Sealed Classes**
For **sealed classes**, Java ensures all subclasses are covered, so `default` may not be necessary.

#### **Example: Exhaustive Sealed Class Switch**
```java
sealed interface Shape permits Circle, Square {}

record Circle(double radius) implements Shape {}
record Square(double side) implements Shape {}

static void process(Shape shape) {
    switch (shape) {
        case Circle c -> System.out.println("Circle with radius: " + c.radius());
        case Square s -> System.out.println("Square with side: " + s.side());
    }
}
```
✅ **Safe**: Since `Shape` only has `Circle` and `Square`, no `default` is needed.

---

#### **Conclusion**
| Case | Behavior |
|------|----------|
| **Matching case found** | Executes the matching case block |
| **No match + `default` case exists** | Executes `default` |
| **No match + no `default` case** | Throws `MatchException` (Java 21+) |
| **Sealed class (all types covered)** | No `default` needed |

✅ **Best Practice**: Always include a `default` case unless using a sealed class with full coverage.

---
### Why is Pattern Matching for `switch` considered more efficient than chained `if-else` statements?

Pattern Matching for `switch` (introduced as a preview feature in Java 17) is considered **more efficient** than chained `if-else` statements due to several key reasons:

#### 1. **Better Compilation to Bytecode**
- The `switch` statement with pattern matching can be optimized by the JVM to use **jump tables** or **lookup tables**, leading to **constant-time (O(1))** execution in some cases.
- Chained `if-else` requires sequential evaluation, resulting in **O(n)** time complexity in the worst case.

#### 2. **Faster Type Checking**
- Pattern Matching in `switch` avoids multiple `instanceof` checks and explicit type casts by matching patterns directly.
- In `if-else`, each `instanceof` check and casting operation adds overhead, making execution slower.

#### 3. **JVM Optimizations**
- The JVM can optimize `switch` statements more aggressively than chained `if-else`, sometimes converting them into **jump tables**, which reduces branching and improves CPU pipeline efficiency.

#### 4. **Code Readability & Maintainability**
- Pattern Matching in `switch` results in **cleaner and more expressive** code compared to verbose chained `if-else` statements.

---
#### Example: Pattern Matching `switch` vs `if-else`

### **Using Pattern Matching `switch` (Efficient & Readable)**
```java
sealed interface Shape permits Circle, Rectangle, Square {}

record Circle(double radius) implements Shape {}
record Rectangle(double length, double width) implements Shape {}
record Square(double side) implements Shape {}

static double area(Shape shape) {
    return switch (shape) {
        case Circle c -> Math.PI * c.radius() * c.radius();
        case Rectangle r -> r.length() * r.width();
        case Square s -> s.side() * s.side();
    };
}
```

#### **Equivalent `if-else` Implementation (Less Efficient)**
```java
static double area(Shape shape) {
    if (shape instanceof Circle c) {
        return Math.PI * c.radius() * c.radius();
    } else if (shape instanceof Rectangle r) {
        return r.length() * r.width();
    } else if (shape instanceof Square s) {
        return s.side() * s.side();
    }
    throw new IllegalArgumentException("Unknown shape");
}
```

---
#### **Key Takeaways**
✅ **Pattern Matching for `switch` is optimized** for performance by the JVM.  
✅ **Reduces redundant type checks** compared to `if-else`.  
✅ **More readable and maintainable** compared to complex `if-else` chains.  

---
### How would you refactor legacy code that uses `instanceof` and type casting to use Pattern Matching `switch` instead?


Before **Pattern Matching for `switch`** (introduced in **Java 17 Preview**), handling different object types required **manual `instanceof` checks** followed by explicit type casting. This approach was **verbose, error-prone, and less readable**.  

By refactoring with **Pattern Matching for `switch`**, we can simplify the code, eliminate redundant type checks, and improve maintainability.

### Legacy Code (Before Refactoring)
```java
import java.util.List;

public class LegacyInstanceOfExample {
    public static void main(String[] args) {
        List<Object> objects = List.of("Hello", 42, 3.14, true);

        for (Object obj : objects) {
            if (obj instanceof String) {
                String s = (String) obj;
                System.out.println("String: " + s.toUpperCase());
            } else if (obj instanceof Integer) {
                Integer i = (Integer) obj;
                System.out.println("Integer squared: " + (i * i));
            } else if (obj instanceof Double) {
                Double d = (Double) obj;
                System.out.println("Double rounded: " + Math.round(d));
            } else if (obj instanceof Boolean) {
                Boolean b = (Boolean) obj;
                System.out.println("Boolean value: " + (b ? "YES" : "NO"));
            } else {
                System.out.println("Unknown type: " + obj);
            }
        }
    }
}
```

### Problems with the Legacy Code
- **Repetitive `instanceof` checks**
- **Manual type casting (`(String) obj`, `(Integer) obj`)**
- **Less readable and harder to maintain**

---

### Refactored Code (Using Pattern Matching for `switch`)
```java
import java.util.List;

public class RefactoredPatternMatchingExample {
    public static void main(String[] args) {
        List<Object> objects = List.of("Hello", 42, 3.14, true, null);

        for (Object obj : objects) {
            processObject(obj);
        }
    }

    static void processObject(Object obj) {
        switch (obj) {
            case String s -> System.out.println("String: " + s.toUpperCase());
            case Integer i -> System.out.println("Integer squared: " + (i * i));
            case Double d -> System.out.println("Double rounded: " + Math.round(d));
            case Boolean b -> System.out.println("Boolean value: " + (b ? "YES" : "NO"));
            case null -> System.out.println("Null encountered.");
            default -> System.out.println("Unknown type: " + obj);
        }
    }
}
```

---

### Key Improvements in the Refactored Code
✅ **No explicit type casting**: The type is inferred automatically.  
✅ **More concise and readable**: Each case directly assigns a variable of the correct type.  
✅ **Handles `null` safely**: No need for `if (obj == null)` checks.  
✅ **Future-proof and extensible**: Easy to add new types in a structured way.  

---

### When Should You Refactor Legacy Code?
- If you are using **Java 17 or later**.
- When dealing with **multiple `instanceof` checks** in a method.
- If the codebase requires **readability and maintainability improvements**.

---

### Using Pattern Matching with Custom Class Hierarchies
If you have a hierarchy of custom classes, `switch` with pattern matching makes it easier to handle different cases efficiently.

### Example
```java
abstract class Animal {}

class Dog extends Animal {
    String bark() { return "Woof!"; }
}

class Cat extends Animal {
    String meow() { return "Meow!"; }
}

public class PatternMatchingWithHierarchy {
    public static void main(String[] args) {
        List<Animal> animals = List.of(new Dog(), new Cat());
        
        for (Animal animal : animals) {
            processAnimal(animal);
        }
    }

    static void processAnimal(Animal animal) {
        switch (animal) {
            case Dog d -> System.out.println("Dog says: " + d.bark());
            case Cat c -> System.out.println("Cat says: " + c.meow());
            default -> System.out.println("Unknown animal");
        }
    }
}
```

### Output
```
Dog says: Woof!
Cat says: Meow!
```

---

### Key Improvements in the Refactored Code
✅ **No explicit type casting**: The type is inferred automatically.  
✅ **More concise and readable**: Each case directly assigns a variable of the correct type.  
✅ **Handles `null` safely**: No need for `if (obj == null)` checks.  
✅ **Future-proof and extensible**: Easy to add new types in a structured way.  

---

### Advanced Example: Complex Polymorphic Structures
For more advanced scenarios, consider a **hierarchy of shapes** with different behaviors.

### Example
```java
sealed interface Shape permits Circle, Rectangle, Triangle {}

record Circle(double radius) implements Shape {}
record Rectangle(double width, double height) implements Shape {}
record Triangle(double base, double height) implements Shape {}

public class PatternMatchingWithShapes {
    public static void main(String[] args) {
        List<Shape> shapes = List.of(new Circle(5), new Rectangle(4, 6), new Triangle(3, 4));

        for (Shape shape : shapes) {
            processShape(shape);
        }
    }

    static void processShape(Shape shape) {
        switch (shape) {
            case Circle c -> System.out.println("Circle with radius: " + c.radius());
            case Rectangle r -> System.out.println("Rectangle with area: " + (r.width() * r.height()));
            case Triangle t -> System.out.println("Triangle with area: " + (0.5 * t.base() * t.height()));
            default -> System.out.println("Unknown shape");
        }
    }
}
```

### Output
```
Circle with radius: 5.0
Rectangle with area: 24.0
Triangle with area: 6.0
```

---

## Key Improvements in the Refactored Code
✅ **No explicit type casting**: The type is inferred automatically.  
✅ **More concise and readable**: Each case directly assigns a variable of the correct type.  
✅ **Handles `null` safely**: No need for `if (obj == null)` checks.  
✅ **Future-proof and extensible**: Easy to add new types in a structured way.  

---

## Trick Questions
### Can you use Pattern Matching switch inside a Java stream operation?

When working with Java Streams, we can use Pattern Matching inside stream operations like `.map()` to transform elements based on their types efficiently.

#### Example Usage

Here’s an example demonstrating how to use **Pattern Matching for `switch`** inside a Stream operation:

#### Code Example

```java
import java.util.List;

public class PatternMatchingInStream {
    public static void main(String[] args) {
        List<Object> elements = List.of("Java", 100, 3.14, true, "Spring");

        elements.stream()
                .map(element -> switch (element) {
                    case String s -> "String: " + s.toUpperCase();
                    case Integer i -> "Integer: " + (i * 2);
                    case Double d -> "Double: " + (d / 2);
                    case Boolean b -> "Boolean: " + (!b);
                    default -> "Unknown Type";
                })
                .forEach(System.out::println);
    }
}
```

#### Explanation
- The `switch` statement inside `.map()` performs **Pattern Matching** to check the type of each element.
- Based on the type, different transformations are applied:
  - Converts `String` to **uppercase**.
  - Multiplies `Integer` values by **2**.
  - Divides `Double` values by **2**.
  - Negates `Boolean` values.
- The `default` case ensures that any unknown types are handled safely.

#### Expected Output
```
String: JAVA
Integer: 200
Double: 1.57
Boolean: false
String: SPRING
```

#### Key Takeaways
✅ **Pattern Matching for `switch`** eliminates the need for manual type checking and casting.  
✅ Works seamlessly inside **Java Stream operations** like `.map()`.  
✅ Requires **Java 17+** (finalized in Java 21).  















