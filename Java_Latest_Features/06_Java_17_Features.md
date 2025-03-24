### Why Were Sealed Classes Introduced?

 - In Java, any class marked `public` or `protected` can be extended by any other class unless marked `final`.

- Sometimes, you want to limit which classes can extend your class to maintain a strict hierarchy.

Sealed classes in Java 17 provide a way to **restrict inheritance** by specifying which classes are allowed to extend a given class. This helps enforce strict class hierarchies and improves code maintainability.

#### Example Problem Before Sealed Classes:

```java
class Animal {}  // Any class can extend this

class Dog extends Animal {}  // Allowed
class Cat extends Animal {}  // Allowed
class UnknownAnimal extends Animal {}  // Unwanted subclass
```

#### ✅ Solution: Sealed Classes
With sealed classes, you can restrict the subclasses.

---


#### Why Were Sealed Classes Introduced?
1. **Controlled Inheritance** - Prevents unintended subclassing.
2. **Better Code Maintainability** - Helps define strict hierarchies.
3. **Optimized Pattern Matching** - Works well with `switch` expressions.
4. **Improved Security** - Prevents unauthorized extension of sensitive classes.
5. **Better Performance** - Compiler can optimize method calls as it knows all possible subclasses.

#### Syntax and Rules
#### Basic Example:
```java
public sealed class Animal permits Dog, Cat {}

final class Dog extends Animal {} // No further subclassing allowed
final class Cat extends Animal {} // No further subclassing allowed
```

#### Subclassing Rules
Each subclass of a `sealed` class must be one of the following:
| Modifier      | Meaning |
|--------------|---------|
| `final`      | No further subclassing allowed. |
| `sealed`     | Can only be extended by specified subclasses. |
| `non-sealed` | Allows unrestricted subclassing. |

#### Example with Different Subclassing Rules:
```java
public sealed class Animal permits Dog, Cat, WildAnimal {}

final class Dog extends Animal {}  // Cannot be extended further

sealed class WildAnimal extends Animal permits Lion, Tiger {}  // Only Lion & Tiger can extend

non-sealed class Cat extends Animal {}  // Any class can extend Cat
```

#### Sealed Interfaces
Sealed classes also work with interfaces:
```java
sealed interface Vehicle permits Car, Bike {}

final class Car implements Vehicle {}  // Cannot be extended further
non-sealed class Bike implements Vehicle {}  // Can be extended by other classes
```

#### Advantages
- **Encapsulation:** Restricts subclassing to maintain strict design patterns.
- **Performance Optimization:** Compiler knows all possible subclasses, enabling better optimizations.
- **Security:** Prevents unauthorized extension of critical classes (e.g., `BankAccount`).
- **Better Error Handling:** The compiler can warn if an exhaustive `switch` case is missing a subclass.

---

### What are the differences between Sealed, Non-Sealed, and Final Classes?

| Modifier      | Can Be Extended? | Who Can Extend? |
|--------------|----------------|----------------|
| `sealed`     | Yes            | Only permitted subclasses |
| `non-sealed` | Yes            | Any class |
| `final`      | No             | No one |

### 1.1 `sealed`
- Restricts inheritance to specific subclasses.
- Requires a `permits` clause to define allowed subclasses.

```java
sealed class Vehicle permits Car, Bike {}
```

### 1.2 `non-sealed`
- Allows any class to extend it, removing previous restrictions.

```java
non-sealed class Car extends Vehicle {}
```

### 1.3 `final`
- Prevents further inheritance.

```java
final class Bike extends Vehicle {}
```

#### 2. Full Example

```java
// Sealed class with permitted subclasses
sealed class Vehicle permits Car, Bike {}

// Non-sealed subclass can be extended by any class
non-sealed class Car extends Vehicle {}

// Final subclass cannot be extended further
final class Bike extends Vehicle {}

// Example class extending non-sealed Car
class Sedan extends Car {}
```

#### 3. Key Benefits
- **Encapsulation**: Controls inheritance for security and maintainability.
- **Predictability**: Prevents unintended subclassing.
- **Better Performance**: Helps compilers optimize code by knowing the class hierarchy.

#### When to Use?
- Use `sealed` when you want to restrict subclassing.
- Use `non-sealed` when you want open inheritance.
- Use `final` when you want no further inheritance.


#### How do sealed classes improve performance?

Sealed classes (introduced in **Java 17**) bring several performance benefits by restricting inheritance and enabling compiler optimizations.

#### 1️⃣ Enables Exhaustive Pattern Matching
- The compiler knows all possible subclasses at compile time.
- This allows efficient **switch expressions** and **pattern matching**.
- Reduces expensive runtime type checks (`instanceof`).

#### 2️⃣ Improves JVM Optimization (Inlining & Devirtualization)
- The **JIT (Just-In-Time) compiler** can **inline** method calls more effectively.
- It enables **devirtualization** (converting virtual calls into direct calls), reducing method lookup time.

#### 3️⃣ Reduces Unnecessary Class Loading
- JVM doesn’t need to dynamically search for additional subclasses.
- Faster **class loading** and reduced **memory footprint**.

#### 4️⃣ Eliminates Unintended Extensibility
- Prevents unwanted subclassing, leading to **better predictability**.
- Compiler can perform better **static analysis and optimizations**.

#### 5️⃣ Helps with Ahead-of-Time Compilation (AOT)
- Tools like **GraalVM** benefit from knowing all implementations in advance.
- Allows **aggressive optimizations** for faster execution.

---

#### Can interfaces be sealed?

- Starting from **Java 15 (preview) and Java 17 (standard)**, Java introduced **sealed interfaces**, which restrict which classes or interfaces can implement them. This helps enforce strict hierarchy control.

#### Syntax
```java
sealed interface Animal permits Dog, Cat {}  

non-sealed class Dog implements Animal {}  
final class Cat implements Animal {}  
```

#### Key Points
1. **Use `sealed` keyword** – Specifies that only certain classes/interfaces can implement the interface.
2. **Use `permits` clause** – Lists the allowed subclasses.
3. **Subclasses must be either:**
   - `final` (no further subclassing)
   - `sealed` (further restricting subclassing)
   - `non-sealed` (removing restrictions)

#### Example
| Modifier       | Description |
|---------------|------------|
| `sealed`      | Restricts inheritance to permitted subclasses. |
| `final`       | Prevents further subclassing. |
| `non-sealed`  | Allows unrestricted subclassing. |

#### Why Use Sealed Interfaces?
- **Stronger Encapsulation** – Controls implementation hierarchy.
- **Better Code Maintenance** – Avoids unintended implementations.
- **Enhances Pattern Matching** – Works well with `instanceof` checks.

---





### How do sealed classes improve security?

Sealed classes in Java improve security by **restricting class hierarchies** and **controlling inheritance**. Below are the key ways they enhance security, along with examples:

#### 1. Prevent Unauthorized Subclassing
Sealed classes explicitly declare which classes can extend them, preventing third-party code from creating unintended subclasses that might introduce security vulnerabilities.

#### Example:
```java
public sealed class Payment permits CreditCard, DebitCard {
    // Only CreditCard and DebitCard can extend Payment
}

public final class CreditCard extends Payment {
    // Implementation of CreditCard
}

public final class DebitCard extends Payment {
    // Implementation of DebitCard
}

// The following would cause a compilation error:
// public class PayPal extends Payment {} // Not permitted
```
🔒 **Security Benefit**: Prevents unauthorized subclassing that could bypass business rules.

---

#### 2. Enhance Code Integrity
Sealed classes make the class hierarchy predictable by limiting extension to known subclasses. This prevents modification of behavior through malicious subclassing.

#### Example:
```java
public sealed class SecureTransaction permits BankTransfer, WireTransfer {
    public void processTransaction() {
        System.out.println("Processing transaction securely...");
    }
}

public final class BankTransfer extends SecureTransaction {
    // Secure implementation
}

public final class WireTransfer extends SecureTransaction {
    // Secure implementation
}
```
🔒 **Security Benefit**: Reduces the risk of altering sensitive logic via untrusted code.

---

#### 3. Better Encapsulation & Maintainability
Unlike abstract classes, where any class can extend them, sealed classes enforce controlled extension. If security policies change, all permitted subclasses can be easily audited.

#### Example:
```java
public sealed class UserAccount permits Admin, RegularUser {
    protected String username;
}

public final class Admin extends UserAccount {
    // Admin-specific logic
}

public final class RegularUser extends UserAccount {
    // Regular user-specific logic
}
```
🔒 **Security Benefit**: Prevents accidental exposure of sensitive functionality.

---

#### 4. Improved Security in API Design
When exposing APIs, restricting subclassing ensures that only verified implementations are used. This avoids unauthorized behavior modifications, preventing potential security loopholes.

#### Example:
```java
public sealed interface SecureAPI permits VerifiedClient, TrustedService {
    void requestAccess();
}

public final class VerifiedClient implements SecureAPI {
    public void requestAccess() {
        System.out.println("Verified client accessing API...");
    }
}

public final class TrustedService implements SecureAPI {
    public void requestAccess() {
        System.out.println("Trusted service accessing API...");
    }
}
```
🔒 **Security Benefit**: Protects APIs from misuse or unauthorized extensions.

---

#### 5. Avoids Reflection-based Attacks
Since subclassing is explicitly restricted, reflection-based attacks that create unauthorized subclasses are less effective.

#### Example:
```java
public sealed class SecureData permits EncryptedData, HashedData {
    private SecureData() {} // Prevents direct instantiation
}

public final class EncryptedData extends SecureData {
    // Encryption logic
}

public final class HashedData extends SecureData {
    // Hashing logic
}
```
🔒 **Security Benefit**: Reduces risks of runtime manipulations via reflection.



#### 🔥 Conclusion
Sealed classes enhance security by **preventing unauthorized subclassing, enforcing code integrity, improving encapsulation, and securing API designs**. This makes Java applications **more predictable and less vulnerable** to subclass-based exploits. 🚀

---

### What happens if a subclass does not declare itself as `final`, `sealed`, or `non-sealed`? 
- Requires Declaration in Subclasses
- If a subclass of a sealed class does not explicitly declare itself as `final`, `sealed`, or `non-sealed`, it will result in a **compilation error**.

#### Example of Compilation Error:
```java
public sealed class Vehicle permits Car, Bike {}

public class Car extends Vehicle { // ❌ Compilation Error!
    // Must be declared as final, sealed, or non-sealed
}
```

#### Corrected Versions:
```java
public final class Car extends Vehicle { 
    // No further subclassing allowed
}
```
OR
```java
public sealed class Car extends Vehicle permits SportsCar {
    // Restricts extension to only SportsCar
}
```
OR
```java
public non-sealed class Car extends Vehicle {
    // Can be extended by any other class
}
```

#### 🔹 Key Points to Remember:
- Every subclass of a `sealed` class **must** explicitly define its own inheritance behavior.
- A subclass **must be either `final`, `sealed`, or `non-sealed`**.
- If omitted, it results in a **compilation error**.
- This rule ensures a **clear and controlled class hierarchy**.

---

### Can a Sealed Class Extend Another Sealed Class?
Yes, a sealed class **can extend another sealed class**, but it must also explicitly declare its permitted subclasses.

#### Example:
```java
public sealed class Animal permits Mammal, Bird {
    // Common behavior for all animals
}

public sealed class Mammal extends Animal permits Dog, Cat {
    // Common behavior for mammals
}

public final class Dog extends Mammal {
    // Dog-specific behavior
}
}

public final class Cat extends Mammal {
    // Cat-specific behavior
}

public final class Bird extends Animal {
    // Bird-specific behavior
}
```
#### 🔹 Key Takeaways:
- A **sealed class can extend another sealed class**.
- The subclass must **define its own permitted subclasses**.
- This maintains a **controlled and predictable hierarchy**.
- Prevents **unauthorized subclassing at multiple levels**.

---

### How do sealed classes help in pattern matching with `switch`?
Sealed classes improve pattern matching in `switch` by ensuring **exhaustiveness**, meaning the compiler can check that all possible cases are handled.

#### Example:
```java
public sealed interface Shape permits Circle, Rectangle, Triangle {}

public final class Circle implements Shape {
    double radius;
}

public final class Rectangle implements Shape {
    double length, width;
}

public final class Triangle implements Shape {
    double base, height;
}

public class ShapeProcessor {
    public static String describeShape(Shape shape) {
        return switch (shape) {
            case Circle c -> "Circle with radius: " + c.radius;
            case Rectangle r -> "Rectangle with length: " + r.length + " and width: " + r.width;
            case Triangle t -> "Triangle with base: " + t.base + " and height: " + t.height;
        };
    }
}
```

#### 🔹 Key Benefits:
- **Exhaustiveness Check**: The compiler ensures all subclasses are covered, reducing runtime errors.
- **No Need for Default Case**: Since `Shape` is sealed, the compiler knows all valid types.
- **Type Safety**: Prevents unexpected or unauthorized subclasses from appearing.


#### 🔥 Conclusion
Sealed classes enhance security by **preventing unauthorized subclassing, enforcing code integrity, improving encapsulation, and securing API designs**. Additionally, they require subclasses to explicitly declare their inheritance behavior, ensuring a **structured and predictable class hierarchy**. They also enable safer and more predictable **pattern matching in `switch` expressions**, leading to cleaner and more maintainable code. 🚀

---


### What is the isSealed() method in the java.lang.Class class?

The `isSealed()` method in the `java.lang.Class` class was introduced in **Java 17**. It is used to check whether a class or interface is **sealed**.

#### Syntax
```java
public boolean isSealed()
```

#### Returns
- `true` → If the class or interface is **sealed**.
- `false` → If the class or interface is **not sealed**.

#### Example Usage
```java
sealed class Animal permits Dog, Cat {}

final class Dog extends Animal {}

final class Cat extends Animal {}

public class Test {
    public static void main(String[] args) {
        System.out.println(Animal.class.isSealed()); // true
        System.out.println(Dog.class.isSealed());    // false
    }
}
```

#### Key Points
- Sealed classes were introduced in **Java 15 (Preview)** and finalized in **Java 17**.
- `isSealed()` helps in **runtime reflection** to check if a class is sealed.
- Use `getPermittedSubclasses()` to list permitted subclasses.

#### Additional Methods
If you want to retrieve the permitted subclasses of a sealed class, use:
```java
Class<?>[] subclasses = Animal.class.getPermittedSubclasses();
```

#### Conclusion
The `isSealed()` method is useful for checking sealed class restrictions at runtime. This feature helps maintain strict inheritance control in Java applications.

---















