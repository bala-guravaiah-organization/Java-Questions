#### Why is Java 11 Important?

- Java 11 is the second Long-Term Support (LTS) release after Java 8.**
- Oracle JDK is no longer free for commercial use starting from Java 11.**
- You can use it **freely during development**, but a **paid license** is required for commercial deployment.  
  - ⚠️ Without a license, you may receive an invoice from Oracle.
- **Java 10 was the last free Oracle JDK** available for download.
- **Oracle ended free support** for Java 8 in January 2019, requiring payment for extended support.
- While Java 8 can still be used, it no longer receives security patches or updates.
---
#### Running Java Programs Without Explicit Compilation

#### 🔍 How It Works Internally (Without `javac`)

From **Java 11 onwards**, you can run a Java program **without** explicitly compiling it using `javac`.  
The `java` command **compiles and executes** the program internally.  

#### 🛠 Internal Process:
1. **Detects Source File**  
   - When running `java Test.java`, the `java` command detects it as a **source file**, not a compiled `.class` file.

2. **Implicit Compilation**  
   - The Java runtime **automatically compiles** `Test.java` into bytecode **in memory** using the **internal compiler API (`javax.tools.JavaCompiler`)**.

3. **Execution by JVM**  
   - The compiled bytecode is **immediately loaded into the JVM** and executed without storing a `.class` file.

---

#### Example Usage
```java
// Test.java
public class Test {
    public static void main(String[] args) {
        System.out.println("Hello, Java 11!");
    }
}
```
Run directly using:
```sh
java Test.java
```
👉 **Output:**
```
Hello, Java 11!
```