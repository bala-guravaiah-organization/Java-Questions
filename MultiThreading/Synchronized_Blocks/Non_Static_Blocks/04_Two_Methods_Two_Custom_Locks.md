```java
public class BankAccount {
    private int savings = 0;         // Money in savings
    private int checking = 0;        // Money in checking
    private final Object savingsLock = new Object();  // Lock for savings
    private final Object checkingLock = new Object(); // Lock for checking

    // Method 1: Deposit money into savings
    public void depositSavings(int amount) {
        synchronized (savingsLock) {
            savings = savings + amount;
            System.out.println("Deposited " + amount + " to savings. Total: " + savings);
        }
    }

    // Method 2: Deposit money into checking
    public void depositChecking(int amount) {
        synchronized (checkingLock) {
            checking = checking + amount;
            System.out.println("Deposited " + amount + " to checking. Total: " + checking);
        }
    }

    // Test it out
    public static void main(String[] args) {
        BankAccount account = new BankAccount();

        // Person 1 deposits to savings
        Thread person1 = new Thread(() -> {
            account.depositSavings(10);
            account.depositSavings(20);
        }, "Person-1");

        // Person 2 deposits to checking
        Thread person2 = new Thread(() -> {
            account.depositChecking(5);
            account.depositChecking(15);
        }, "Person-2");

        person1.start();
        person2.start();
    }
}

```
### Step-by-Step Explanation

**Step 1: What’s in the Code?**

- **Resources:**
    - `savings`: Tracks money in the savings account (starts at 0).

    - `checking`: Tracks money in the checking account (starts at 0).

- **Locks:**
    - `savingsLock`: A key just for the savings account.

    - `checkingLock`: A different key just for the checking account.

- **Methods:**
    - `depositSavings()`: Adds money to `savings`, protected by `savingsLock`.

    - `depositChecking()`: Adds money to `checking`, protected by `checkingLock`.

---

**Step 2: How Does `depositSavings()` Work?**
1. A person (thread, like Person-1) wants to deposit money into savings.

2. Person-1 checks the `savingsLock`:
    - If no one has it, Person-1 takes it and enters the `synchronized (savingsLock) block`.

    - If someone else has it, Person-1 waits.

3. Inside, Person-1 adds the amount to `savings` (e.g., `savings` goes from 0 to 10) and prints a message.

4. When finished, Person-1 leaves the block and gives back the `savingsLock`.

---
**Step 3: How Does depositChecking() Work?**
1. Another person (thread, like Person-2) wants to deposit money into checking.

2. Person-2 checks the `checkingLock`:
    - If no one has it, Person-2 takes it and enters the `synchronized (checkingLock)` block.

    - If someone else has it, Person-2 waits.

3. Inside, Person-2 adds the amount to `checking` (e.g., `checking` goes from 0 to 5) and prints a message.

4. When finished, Person-2 gives back the `checkingLock`.

---

**Step 4: Two People at Once**
- **Person-1** runs `depositSavings()` and needs the `savingsLock`.

- **Person-2** runs `depositChecking()` and needs the `checkingLock`.

- **Key Idea :** Since `savingsLock` and `checkingLock` are different, Person-1 and Person-2 can deposit money at the same time! Savings and checking are separate, so they don’t need to wait for each other.

---

**Step 5: What You Might See (Output)**
Running the `main` method could show:

```
Deposited 10 to savings. Total: 10
Deposited 5 to checking. Total: 5
Deposited 20 to savings. Total: 30
Deposited 15 to checking. Total: 20
```
The lines might mix because Person-1 and Person-2 work together. That’s fine—each person only changes their own account (`savings` or `checking`), and the locks keep it safe.

---

***Step 6: What If Two People Want Savings?**
- If Person-1 is in `depositSavings()` (holding `savingsLock`) and Person-3 tries `depositSavings()`:
    - Person-3 waits because Person-1 has the  `savingsLock`.

    - When Person-1 finishes, Person-3 gets the `savingsLock` and deposits money.

- Same idea for `checkingLock` in `depositChecking()`.

---