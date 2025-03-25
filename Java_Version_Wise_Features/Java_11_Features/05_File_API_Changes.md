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
