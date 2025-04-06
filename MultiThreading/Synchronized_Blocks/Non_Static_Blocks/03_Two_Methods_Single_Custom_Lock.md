### 🧸 Shared ToyBox Example in Java 
This example demonstrates how **two methods in a class use a single custom lock**. It means both methods share the same lock, and only **one thread can execute either method at a time**.

---

#### 🚀 Example Code: Shared Toy Box

```java
public class ToyBox {
    private int balls = 0;         // Number of balls in the box
    private int cars = 0;          // Number of toy cars in the box
    private final Object lock = new Object(); // One lock for both

    // Method 1: Add a ball to the box
    public void addBall() {
        synchronized (lock) {
            balls = balls + 1;
            System.out.println("Added a ball. Total balls: " + balls);
        }
    }

    // Method 2: Add a car to the box
    public void addCar() {
        synchronized (lock) {
            cars = cars + 1;
            System.out.println("Added a car. Total cars: " + cars);
        }
    }

    // Test it out
    public static void main(String[] args) {
        ToyBox box = new ToyBox();

        // Kid 1 adds balls
        Thread kid1 = new Thread(() -> {
            box.addBall();
            box.addBall();
        }, "Kid-1");

        // Kid 2 adds cars
        Thread kid2 = new Thread(() -> {
            box.addCar();
            box.addCar();
        }, "Kid-2");

        kid1.start();
        kid2.start();
    }
}
```

---

#### 🧠 Step-by-Step Explanation for Beginners

#### Step 1: What’s in the Code?

**Resources:**
- `balls`: Counts balls in the toy box (starts at 0).
- `cars`: Counts toy cars in the toy box (starts at 0).

**Lock:**
- `lock`: One key (lock) used for both `addBall()` and `addCar()`.

**Methods:**
- `addBall()`: Adds 1 ball, protected by `lock`.
- `addCar()`: Adds 1 car, protected by the same `lock`.

---

#### Step 2: How Does `addBall()` Work?

- A kid (thread like Kid-1) tries to add a ball.
- Kid-1 checks the `lock`:
  - If free, Kid-1 takes the lock and enters the block.
  - If taken, Kid-1 waits.
- Inside the block, it increments `balls` and prints.
- Then, Kid-1 releases the lock.

---

#### Step 3: How Does `addCar()` Work?

- Another kid (Kid-2) tries to add a car.
- Kid-2 checks the **same lock**:
  - If free, Kid-2 enters.
  - If not, Kid-2 waits.
- Inside, it increments `cars` and prints.
- Then, it releases the lock.

---

#### Step 4: Two Kids with One Lock

- **Kid-1** and **Kid-2** both need the same lock.
- They **cannot run at the same time**.
- If Kid-1 is adding balls, Kid-2 must **wait** until Kid-1 finishes.

---

#### Step 5: What You Might See (Output)

Possible outputs:

```
Added a ball. Total balls: 1
Added a ball. Total balls: 2
Added a car. Total cars: 1
Added a car. Total cars: 2
```

OR

```
Added a car. Total cars: 1
Added a car. Total cars: 2
Added a ball. Total balls: 1
Added a ball. Total balls: 2
```

Notice: The output is **not mixed**. One kid finishes all their actions before the other starts.

---

#### Step 6: Waiting in Action

- If **Kid-1** is inside `addBall()`, holding the lock:
  - **Kid-2** tries `addCar()`, sees the lock is taken, and waits.
- Only one can enter the toy box at a time!

---

#### 🔐 Why One Lock?

- **Like a Single Door**: Only one person (thread) can enter the room (critical section).
- **Why?** To **avoid conflicts** or keep things **safe**.

#### When to Use One Lock?

✅ Use **One Lock** If:
- Both actions must be done safely (no overlap).
- You want to avoid race conditions.

❌ Use **Two Locks** If:
- Actions are unrelated (can happen at the same time).
- Example: Updating two independent counters.

---

