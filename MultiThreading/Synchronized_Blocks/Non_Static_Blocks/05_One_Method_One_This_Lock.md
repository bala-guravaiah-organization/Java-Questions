
### 🍪 CookieJar Example - Understanding `this` as a Lock in Java

This simple example demonstrates how synchronization using `this` works in Java. We use a fun, beginner-friendly CookieJar scenario where multiple threads (kids) try to add cookies to the same jar. By using `synchronized(this)`, we ensure thread safety—only one kid can add a cookie at a time.

---

#### ✅ Example Code: Cookie Jar

```java
public class CookieJar {
    private int cookies = 0; // Number of cookies in the jar

    // Method: Add a cookie to the jar, synchronized with 'this'
    public void addCookie() {
        synchronized (this) {
            cookies = cookies + 1;
            System.out.println(Thread.currentThread().getName() + " added a cookie. Total: " + cookies);
        }
    }

    // Test it out
    public static void main(String[] args) {
        CookieJar jar = new CookieJar();

        // Kid 1 adds cookies
        Thread kid1 = new Thread(() -> {
            jar.addCookie();
            jar.addCookie();
        }, "Kid-1");

        // Kid 2 adds cookies
        Thread kid2 = new Thread(() -> {
            jar.addCookie();
            jar.addCookie();
        }, "Kid-2");

        kid1.start();
        kid2.start();
    }
}
```

---

#### 🧠 Step-by-Step Explanation for Beginners

#### 🧩 Step 1: What’s in the Code?

**Resource:**  
- `cookies`: Counts how many cookies are in the jar (starts at 0).

**Lock:**  
- `this`: The lock is the entire `CookieJar` object.

**Method:**  
- `addCookie()`: Adds 1 cookie, protected using `synchronized(this)`.

---

#### 🔐 Step 2: How Does `addCookie()` Work?

1. A kid (thread like `Kid-1`) wants to add a cookie.
2. `Kid-1` checks the lock, which is `this` (the `CookieJar` object).
3. If the lock is free, `Kid-1` enters the synchronized block.
4. Inside, the cookie count increases and a message is printed.
5. After finishing, `Kid-1` exits the block and releases the lock.

---

#### 🤼 Step 3: Two Kids Using the Same Method

- `Kid-1` and `Kid-2` both call `addCookie()`.
- Since both use the same lock (`this`), only **one** can add a cookie at a time.
- If one thread is inside the method, the other waits.

---

#### 🖨️ Step 4: Possible Output

You may see either of the following (but **not** mixed lines):

```
Kid-1 added a cookie. Total: 1  
Kid-1 added a cookie. Total: 2  
Kid-2 added a cookie. Total: 3  
Kid-2 added a cookie. Total: 4
```

**OR**

```
Kid-2 added a cookie. Total: 1  
Kid-2 added a cookie. Total: 2  
Kid-1 added a cookie. Total: 3  
Kid-1 added a cookie. Total: 4
```

🔒 Why no mixed lines? Because `this` is shared and only one thread can access the method at a time.

---

#### ⏳ Step 5: Waiting in Action

If `Kid-1` is in `addCookie()`, `Kid-2` waits.  
Only after `Kid-1` exits does `Kid-2` enter.  
It’s like there’s one key to the jar—the key **is the jar itself**.

---

#### 💡 Why Use `this`?

- Simple: No need for an extra lock object.
- Effective: Ensures only one thread accesses the critical section.
- Clear: Makes it obvious the lock is tied to the object itself.

---

#### 🧪 What If There Were Two Methods?

If you add another method like `removeCookie()` with `synchronized(this)`, it **also** uses the same lock (`this`).  
So `addCookie()` and `removeCookie()` **cannot** run at the same time either—they’ll take turns.

---