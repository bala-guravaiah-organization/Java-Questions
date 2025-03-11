
## **Question 42: Explain System.out.println() statement**

**Answer:**
- `System` is a class in the `java.lang` package.
- `out` is a static member of the `System` class and an instance of `java.io.PrintStream`.
- `println()` is a method of the `PrintStream` class, which is used to print messages to the console with a newline.

### **Example:**

```java
public class PrintlnExample {
    public static void main(String[] args) {
        System.out.println("Hello, World!");
        System.out.println(100);
        System.out.println(3.14);
        System.out.println(true);
    }
}
```

#### **Output:**
```
Hello, World!
100
3.14
true
```

---

## **Question 43: Explain Auto-boxing and Un-boxing**

**Answer:**
In Java 1.5, Auto-boxing and Un-boxing were introduced to automatically convert primitive types to their corresponding wrapper classes and vice versa.

### **Key Points:**
- **Auto-boxing:** Conversion of a primitive type into its corresponding wrapper class.
- **Un-boxing:** Conversion of a wrapper class object back into a primitive type.

### **Example:**

```java
public class BoxingExample {
    public static void main(String[] args) {
        // Auto-boxing: int to Integer
        int num = 10;
        Integer boxedNum = num;
        System.out.println("Auto-boxing: " + boxedNum);
        
        // Un-boxing: Integer to int
        Integer obj = new Integer(20);
        int unboxedNum = obj;
        System.out.println("Un-boxing: " + unboxedNum);
    }
}
```

#### **Output:**
```
Auto-boxing: 10
Un-boxing: 20
```

**Question 46: Explain static keyword in Java**

**Answer:** In Java, a `static` member is a member of a class that isn’t associated with an instance of a class. Instead, the member belongs to the class itself.

### Static is applicable for:
- **Variable**
- **Method**
- **Block**
- **Nested class**

### **Static Variable:**
- If a variable is declared as `static`, it is known as a *static variable*.
- **Only one copy** of the variable is created and shared among all instances of the class.
- The static variable gets memory **only once** in the class area when the class is loaded.
- **Use case:** Declare common properties for all objects, e.g., company name of employees.

### **Static Method:**
- A method declared with the `static` keyword belongs to the class rather than to any object.
- **Access directly** using the class name without creating an object.
- **Rules:**
  - Cannot access **non-static** methods or variables.
  - Cannot use `this` or `super` inside a static method.
- **Example:** The `main()` method is static, allowing Java to start an application without creating an object.

### **Static Block:**
- Executed **once** when the class is loaded.
- Used to initialize **static variables**.

### **Static Nested Classes:**
- A special type of **inner class** where the inner class is static.
- Can **only access static members** of the outer class.
- **Advantage:** Improves code readability and maintainability.
- Unlike normal inner classes, **a static nested class can exist without an instance of the outer class**.

### **How to create an object of a static inner class?**
```java
OuterClass.StaticNestedClass nestedClassObject = new OuterClass.StaticNestedClass();
```

### **Error Scenario:**
- **Compile-time error occurs** when trying to access a **non-static** member inside a static nested class.

---

### **Example: Using Inner Class Object**
```java
class OuterClass {
    static class StaticNestedClass {
        void display() {
            System.out.println("Static Nested Class Method");
        }
    }
    public static void main(String[] args) {
        OuterClass.StaticNestedClass obj = new OuterClass.StaticNestedClass();
        obj.display();
    }
}
```
**Output:**
```
Static Nested Class Method
```

### **Example: Static Members in Static Inner Class**
```java
class OuterClass {
    static class StaticNestedClass {
        static void staticMethod() {
            System.out.println("Static method in static nested class");
        }
    }
    public static void main(String[] args) {
        OuterClass.StaticNestedClass.staticMethod(); // No need to create an object
    }
}
```
**Output:**
```
Static method in static nested class
```

**Question 47: What is an Inner Class in Java, how it can be instantiated, and what are the types of Inner Classes?**

**Answer:** In Java, when you define one **non-static** class within another class, it is called an **Inner Class (Nested Class)**. Inner classes allow logically grouping classes that are only used in one place, thereby increasing encapsulation and making the code more readable and maintainable.

### **Key Points about Inner Classes:**
- An **inner class is associated with the object** of the outer class and can access all variables and methods of the outer class.
- **Static variables and static methods are not allowed** in non-static inner classes since they are associated with instances.
- To create an **instance of an inner class**, an instance of the outer class is required first.

### **Types of Inner Classes:**
1. **Member Inner Class** (Regular Inner Class)
2. **Static Nested Class**
3. **Method-local Inner Class**
4. **Anonymous Inner Class**

### **How to instantiate an Inner Class?**
```java
OuterClass outerObject = new OuterClass();
OuterClass.InnerClass innerObject = outerObject.new InnerClass();
```

---

### **Example: Member Inner Class**
```java
class OuterClass {
    private String message = "Hello from Outer Class";
    
    class InnerClass {
        void display() {
            System.out.println(message);
        }
    }
    
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        OuterClass.InnerClass inner = outer.new InnerClass();
        inner.display();
    }
}
```
**Output:**
```
Hello from Outer Class
```

### **Example: Compile-time error when static variable/method is present in Inner Class**
```java
class OuterClass {
    class InnerClass {
        static int data = 100; // Compile-time error
        static void display() { // Compile-time error
            System.out.println("Static method inside inner class");
        }
    }
}
```
**Error:**
```
Inner classes cannot have static members.
```

---

## **Special Types of Inner Classes:**
### **1. Local Inner Class:**
- Defined **inside a block**, usually within a method, loop, or if clause.
- **Not a member of the enclosing class** but belongs to the block it is defined in.
- Cannot have **access modifiers**, but can be `final` or `abstract`.
- **Has access to the enclosing class's members.**
- Must be **instantiated within the block** it is defined in.

#### **Points to remember:**
- Cannot be instantiated **outside** the block where they are defined.
- Has access to **members** of the enclosing class.
- **Until Java 1.7:** Can only access `final` local variables of the enclosing block.
- **From Java 1.8 onwards:** Can access **non-final** local variables of the enclosing block.
- **Scope is restricted** to the block where they are defined.
- Can **extend an abstract class** or **implement an interface**.

#### **Example: Local Inner Class**
```java
class OuterClass {
    void outerMethod() {
        class LocalInner {
            void display() {
                System.out.println("This is a Local Inner Class");
            }
        }
        LocalInner localInner = new LocalInner();
        localInner.display();
    }
    
    public static void main(String[] args) {
        OuterClass outer = new OuterClass();
        outer.outerMethod();
    }
}
```
**Output:**
```
This is a Local Inner Class
```

#### **Compile-time Error Example: Modifying Block-Level Variables**
```java
class OuterClass {
    void outerMethod() {
        int x = 10; // Effectively final
        class LocalInner {
            void display() {
                System.out.println("Value: " + x);
            }
        }
        LocalInner obj = new LocalInner();
        obj.display();
        x = 20; // Compile-time error
    }
}
```

---

### **2. Anonymous Inner Class:**
- A **class without a name**.
- Used when you need a class only **once**.
- **Cannot have a constructor** since it has no class name.
- Cannot be declared as **static**.
- Generally used to **override methods** of a class or interface.

#### **Use Case Example:** Sorting Employees using an Anonymous Inner Class
```java
import java.util.*;

class Employee {
    String name;
    int age;
    Employee(String name, int age) {
        this.name = name;
        this.age = age;
    }
    public String toString() {
        return name + " - " + age;
    }
}

public class AnonymousInnerDemo {
    public static void main(String[] args) {
        List<Employee> list = new ArrayList<>();
        list.add(new Employee("John", 30));
        list.add(new Employee("Alice", 25));
        list.add(new Employee("Bob", 28));
        
        Collections.sort(list, new Comparator<Employee>() {
            public int compare(Employee e1, Employee e2) {
                return e1.name.compareTo(e2.name);
            }
        });
        
        System.out.println(list);
    }
}
```
**Output:**
```
[Alice - 25, Bob - 28, John - 30]
```

#### **Example: Using Anonymous Inner Class for Runnable**
```java
class Test {
    public static void main(String[] args) {
        Runnable r = new Runnable() {
            public void run() {
                System.out.println("Running thread using anonymous inner class");
            }
        };
        Thread t = new Thread(r);
        t.start();
    }
}
```
**Output:**
```
Running thread using anonymous inner class
```

#### **Example: Overriding a Parent Class Method**
```java
class Parent {
    void show() {
        System.out.println("Parent class method");
    }
}
class Test {
    public static void main(String[] args) {
        Parent obj = new Parent() {
            void show() {
                System.out.println("Anonymous Inner Class Method");
            }
        };
        obj.show();
    }
}
```
**Output:**
```
Anonymous Inner Class Method
```

#### **Compile-time Error: Using Non-Final Static Variable in Anonymous Class**
```java
class Test {
    static int x = 10;
    public static void main(String[] args) {
        new Object() {
            void display() {
                // x++; // Compile-time error: Cannot modify non-final static variable
                System.out.println(x);
            }
        };
    }
}
```