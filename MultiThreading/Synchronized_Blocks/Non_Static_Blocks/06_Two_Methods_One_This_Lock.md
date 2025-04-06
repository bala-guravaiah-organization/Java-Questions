### Pet Store Synchronization Example in Java
---

#### Example Code: Pet Store
```java
public class PetStore {
    private int dogs = 0;    // Number of dogs in the store
    private int cats = 0;    // Number of cats in the store

    // Method 1: Add a dog to the store
    public void addDog() {
        synchronized (this) {
            dogs = dogs + 1;
            System.out.println(Thread.currentThread().getName() + " added a dog. Total dogs: " + dogs);
        }
    }

    // Method 2: Add a cat to the store
    public void addCat() {
        synchronized (this) {
            cats = cats + 1;
            System.out.println(Thread.currentThread().getName() + " added a cat. Total cats: " + cats);
        }
    }

    // Test it out
    public static void main(String[] args) {
        PetStore store = new PetStore();

        // Worker 1 adds dogs
        Thread worker1 = new Thread(() -> {
            store.addDog();
            store.addDog();
        }, "Worker-1");

        // Worker 2 adds cats
        Thread worker2 = new Thread(() -> {
            store.addCat();
            store.addCat();
        }, "Worker-2");

        worker1.start();
        worker2.start();
    }
}
```

---

#### Step-by-Step Explanation for Beginners

#### Step 1: What’s in the Code?
**Resources:**
- `dogs`: Counts dogs in the store (starts at 0).
- `cats`: Counts cats in the store (starts at 0).

**Lock:**
- `this`: The lock is the PetStore object itself. Both methods use this as their key.

**Methods:**
- `addDog()`: Adds 1 dog, protected by `this`.
- `addCat()`: Adds 1 cat, also protected by `this`.

---

#### Step 2: How Does `addDog()` Work?
- A worker (thread, like Worker-1) wants to add a dog.
- Worker-1 checks the lock (`this`, the PetStore object):
  - If no one has it, Worker-1 takes it and enters the `synchronized (this)` block.
  - If someone else has it, Worker-1 waits.
- Inside, Worker-1 adds 1 to `dogs` and prints a message.
- When done, Worker-1 releases the lock.

#### Step 3: How Does `addCat()` Work?
- Another worker (Worker-2) wants to add a cat.
- Worker-2 checks the same lock (`this`):
  - If free, takes it and enters.
  - If not, waits.
- Inside, Worker-2 adds 1 to `cats` and prints a message.
- Then releases the lock.

---

#### Step 4: Two Workers with One Lock
- Worker-1 calls `addDog()` and needs the lock (`this`).
- Worker-2 calls `addCat()` and also needs the lock (`this`).
- **Key Idea:** Since both use the same lock (`this`), only one can run at a time.

---

#### Step 5: Sample Output
```
Worker-1 added a dog. Total dogs: 1
Worker-1 added a dog. Total dogs: 2
Worker-2 added a cat. Total cats: 1
Worker-2 added a cat. Total cats: 2
```
OR
```
Worker-2 added a cat. Total cats: 1
Worker-2 added a cat. Total cats: 2
Worker-1 added a dog. Total dogs: 1
Worker-1 added a dog. Total dogs: 2
```

**Why not mixed?** One thread completes both actions before the other starts, since they share the same lock.

---

#### Step 6: Waiting in Action
- If Worker-1 is in `addDog()` (holding `this`):
  - Worker-2 tries `addCat()` but sees `this` is taken and waits.
  - Worker-2 starts only after Worker-1 finishes.

It’s like one key for the store—only one worker can use it at a time.

---

#### Why Use `this` for Both?

**Like One Store Key:**
- `this` is the whole PetStore.
- When locked with `synchronized (this)`, no one else can enter synchronized blocks.

**Compared to Two Locks:**
- With two separate locks (e.g., `dogLock` and `catLock`), both workers could run at the same time.
- Using `this` makes them take turns.

---

