#### What are Nested Classes in Java?
Java supports four types of nested classes:
1. **Static Nested Classes** - Independent of the outer class.
2. **Non-static Nested Classes (Inner Classes)** - Have access to outer class members.
3. **Local Classes** - Defined inside a method.
4. **Anonymous Classes** - Declared without a name for one-time use.

---

#### What is the Role of Nestmates in Java 11?
**Nestmates** are classes that belong to the same **nest** (i.e., a primary class and its nested classes). Java 11 introduced **NestHost** and **NestMembers** attributes to allow **direct access** to private members without requiring synthetic bridge methods.

---

#### Why was Nest-Based Access Introduced in Java 11?
#### Before Java 11:
- The compiler generated **bridge methods** to allow private member access.
- These additional methods increased **bytecode size** and **runtime complexity**.

#### With Java 11:
- **No extra bridge methods**, reducing bytecode size.
- **Improved runtime efficiency** by allowing direct access.
- **Simplified code**, making it cleaner and easier to maintain.

---

#### Example: Nest-Based Access in Java 11
#### ✅ Before Java 11 (Using Bridge Methods)
```java
class OuterClass {
    private String secret = "This is a secret";

    class InnerClass {
        String getSecret() {
            return secret; // Compiler generates a bridge method
        }
    }
}
```

#### ✅ After Java 11 (Using Nest-Based Access)
```java
class OuterClass {
    private String secret = "This is a secret";

    class InnerClass {
        String getSecret() {
            return secret; // No bridge method required
        }
    }
}

public class Main {
    public static void main(String[] args) {
        OuterClass.InnerClass inner = new OuterClass().new InnerClass();
        System.out.println(inner.getSecret()); // Output: This is a secret
    }
}
```

#### Key Changes in Java 11:
- **No synthetic bridge methods** are created.
- **Direct private member access** is allowed.
- **Improved performance** and **reduced bytecode size**.

---

#### Benefits of Nest-Based Access Control
✔ **Reduces bytecode size** (No extra bridge methods).
✔ **Improves runtime efficiency** (Direct access to private members).
✔ **Enhances maintainability** (Cleaner and more readable code).
✔ **Ensures security** (Only valid nestmates can access private members).