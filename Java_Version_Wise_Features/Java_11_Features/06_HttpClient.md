### HttpClient :
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