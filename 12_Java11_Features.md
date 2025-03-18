#### What are java 11 Features ?

- Java 11 Importance
- Running Java Programs Without Compilation
- String Enhancements
- Predicate Interface Enhancements
- The `not()` method example is great.
- HTTP Client API
- Local Variable Type Inference (`var`) in Lambdas


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

---
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

#### 🔍 File API Changes in Java 11
#### 🆕 `readString()` and `writeString()` Methods
- Java 11 simplifies reading and writing text files.
- Previously, reading required using `FileInputStream`, `BufferedReader`, and manual handling.
- Java 11 introduces:
  - **`readString(Path path)`** → Reads all file content into a string.
  - **`writeString(Path path, CharSequence csq, OpenOption... options)`** → Writes text to a file.

#### Example Usage
```java
import java.nio.file.*;
import java.io.IOException;

public class FileAPIExample {
    public static void main(String[] args) throws IOException {
        Path filePath = Files.writeString(Files.createTempFile("test", ".txt"), "Java 11 features");
        String content = Files.readString(filePath);
        System.out.println("File Content: " + content);
    }
}
```

#### ✅ Expected Output:
```
File Content: Java 11 features
```

#### 🚀 Why Use `readString()` and `writeString()`?
- **Less boilerplate** compared to older file handling methods.
- **Ensures proper file closure** after reading.
- **Supports different file options**, such as `StandardOpenOption.APPEND` to append content instead of overwriting.
---

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

#### 🚀 Introduction
- Java 11 introduces a new **HTTP Client API** for making HTTP requests and receiving responses.
- The `HttpClient` is available in the `java.net.http` package.
- HTTP requests are a fundamental part of modern programming, and Java previously relied on libraries like `HttpURLConnection` or third-party options such as `Apache HttpClient`.
- The enhanced `HttpClient` API was initially introduced as an **experimental feature** in Java 9 but became **standard** in Java 11.
- It is now **recommended** over other HTTP client APIs, offering built-in support without requiring external dependencies.

#### Key Features
- **Asynchronous and Synchronous request handling**
- **Support for HTTP/1.1 and HTTP/2**
- **WebSocket support**
- **Built-in timeout handling**
- **Better performance and ease of use**

#### 🛠 Steps to Use `HttpClient`
1. **Create an HttpClient instance** using `HttpClient.newBuilder()`.
2. **Create an HttpRequest instance** using `HttpRequest.newBuilder()`.
3. **Send the request** using `httpClient.send()` and retrieve the response object.

#### Example Usage
#### 🌐 Sending a GET Request
```java
import java.net.http.*;
import java.net.URI;
import java.io.IOException;

public class HttpClientExample {
    public static void main(String[] args) throws IOException, InterruptedException {
        // Create HttpClient instance
        HttpClient client = HttpClient.newBuilder().build();

        // Create HttpRequest instance
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://jsonplaceholder.typicode.com/posts/1"))
                .GET()
                .build();

        // Send request and get response
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());

        // Print response
        System.out.println("Response Code: " + response.statusCode());
        System.out.println("Response Body: " + response.body());
    }
}
```

#### ✅ Expected Output:
```
Response Code: 200
Response Body: {
  "userId": 1,
  "id": 1,
  "title": "Sample Title",
  "body": "Sample Body"
}
```

#### 🚀 Why Use `HttpClient`?
- **More efficient** than `HttpURLConnection`.
- **Supports modern web standards** (e.g., HTTP/2 and WebSockets).
- **Simplifies HTTP request handling** without additional libraries.
- **Works natively in Java 11** without requiring third-party dependencies.


#### Asynchronous HTTP Client in Java 11

#### Making Asynchronous HTTP Calls
- Java 11 provides the **`sendAsync()`** method in `HttpClient` to perform asynchronous HTTP requests.
- This method returns a **`CompletableFuture<HttpResponse<T>>`**, allowing non-blocking execution.

#### Example Usage:
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.util.concurrent.CompletableFuture;

public class AsyncHttpClientExample {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();

        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://jsonplaceholder.typicode.com/todos/1"))
                .GET()
                .build();

        CompletableFuture<HttpResponse<String>> responseFuture =
                client.sendAsync(request, HttpResponse.BodyHandlers.ofString());

        responseFuture.thenApply(HttpResponse::body)
                      .thenAccept(System.out::println)
                      .join(); // Ensures main thread waits for completion

        System.out.println("Request sent asynchronously...");
    }
}
```

#### ✅ Expected Output:
```
Request sent asynchronously...
{
  "userId": 1,
  "id": 1,
  "title": "delectus aut autem",
  "completed": false
}
```

#### 🚀 Why Use `sendAsync()`?
- **Non-blocking execution**: The main thread continues execution while the request is processed.
- **Chaining with `thenApply()` and `thenAccept()`**: Process responses in a functional way.
- **Better performance**: Suitable for handling multiple requests in parallel.

#### 📌 Making a Synchronous POST Request
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.net.http.HttpRequest.BodyPublishers;
import java.io.IOException;

public class SyncPostRequest {
    public static void main(String[] args) throws IOException, InterruptedException {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://jsonplaceholder.typicode.com/posts"))
                .header("Content-Type", "application/json")
                .POST(BodyPublishers.ofString("{\"title\":\"Java 11\",\"body\":\"HttpClient POST\",\"userId\":1}"))
                .build();
        
        HttpResponse<String> response = client.send(request, HttpResponse.BodyHandlers.ofString());
        
        System.out.println("Response Code: " + response.statusCode());
        System.out.println("Response Body: " + response.body());
    }
}
```
#### ✅ Expected Output:
```
Response Code: 201
Response Body: {
  "title": "Java 11",
  "body": "HttpClient POST",
  "userId": 1,
  "id": 101
}
```

#### 📌 Making an Asynchronous POST Request
For non-blocking operations, you can use `HttpClient`'s asynchronous capabilities with `sendAsync()`.

#### Example Usage
```java
import java.net.URI;
import java.net.http.HttpClient;
import java.net.http.HttpRequest;
import java.net.http.HttpResponse;
import java.net.http.HttpRequest.BodyPublishers;
import java.util.concurrent.CompletableFuture;

public class AsyncPostRequest {
    public static void main(String[] args) {
        HttpClient client = HttpClient.newHttpClient();
        HttpRequest request = HttpRequest.newBuilder()
                .uri(URI.create("https://jsonplaceholder.typicode.com/posts"))
                .header("Content-Type", "application/json")
                .POST(BodyPublishers.ofString("{\"title\":\"Java 11\",\"body\":\"HttpClient Async POST\",\"userId\":1}"))
                .build();
        
        CompletableFuture<HttpResponse<String>> responseFuture = client.sendAsync(request, HttpResponse.BodyHandlers.ofString());
        
        responseFuture.thenApply(HttpResponse::body)
                .thenAccept(System.out::println)
                .join(); // Ensures the program waits for completion
    }
}
```
#### ✅ Expected Output:
```
{
  "title": "Java 11",
  "body": "HttpClient Async POST",
  "userId": 1,
  "id": 101
}
```

#### 🚀 Why Use Asynchronous Requests?
- **Non-blocking**: The program continues executing while waiting for the response.
- **Better performance**: Useful for high-throughput applications.
- **Uses `CompletableFuture`**: Enables chaining and handling responses efficiently.

#### What is Nest-Based Access Control?
Java 11 introduced **nest-based access control**, allowing nested classes to access each other's **private members** without requiring accessibility-broadening bridge methods. This feature enhances **security**, **reduces bytecode size**, and **improves performance**.

---

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








