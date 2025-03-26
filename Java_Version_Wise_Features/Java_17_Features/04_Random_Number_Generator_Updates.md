
Java 17 introduced several enhancements to pseudo-random number generators (PRNGs) as part of **JEP 356: Enhanced Pseudo-Random Number Generators**. These updates improve flexibility, maintainability, and randomness quality.

---

### **Key PRNG Enhancements in Java 17**

#### 1. **New `RandomGenerator` Interface**
- Provides a common API for different PRNG algorithms.
- Example:

  ```java
  import java.util.random.RandomGenerator;

  public class RandomExample {
      public static void main(String[] args) {
          RandomGenerator random = RandomGenerator.getDefault();
          System.out.println(random.nextInt(1, 100)); // Random number between 1 and 100
      }
  }
  ```

#### 2. **New PRNG Implementations**
Java 17 introduced multiple PRNG implementations:

| Algorithm | Description |
|-----------|--------------------------------|
| `L128X128MixRandom` | Fast, statistically robust, and suitable for general use. |
| `L128X256MixRandom` | Larger state size, better randomness properties. |
| `L128X1024MixRandom` | Largest state size, best randomness but slower. |
| `L64X128MixRandom` | 64-bit variant, smaller state size. |
| `L64X128StarStarRandom` | Alternative variant with good randomness. |
| `L64X256MixRandom` | More state bits than `L64X128MixRandom`. |

#### 3. **New Factory Methods for PRNGs**
- Java 17 allows creating specific PRNGs using `RandomGeneratorFactory`.
- Example:
  
  ```java
  import java.util.random.RandomGenerator;
  import java.util.random.RandomGeneratorFactory;

  public class PRNGExample {
      public static void main(String[] args) {
          RandomGenerator generator = RandomGeneratorFactory.of("L128X256MixRandom").create();
          System.out.println(generator.nextInt(1, 100));
      }
  }
  ```

#### 4. **Streams API Support for Random Numbers**
- Java 17 enables generating random numbers using `ints()`, `longs()`, and `doubles()` from the `RandomGenerator` interface.
- Example:

  ```java
  import java.util.random.RandomGenerator;

  public class RandomStreamExample {
      public static void main(String[] args) {
          RandomGenerator generator = RandomGenerator.getDefault();
          generator.ints(5, 1, 100).forEach(System.out::println); // Generates 5 random numbers
      }
  }
  ```

---

#### **Comparison of PRNG Enhancements**

| Feature | Java 8 | Java 17 |
|---------|--------|---------|
| PRNG Interface | No common interface | `RandomGenerator` interface introduced |
| Performance | `ThreadLocalRandom`, `SplittableRandom` | More optimized PRNGs (`L128X256MixRandom`, etc.) |
| PRNG Algorithms | Limited | Multiple new PRNGs with different state sizes |
| Streams API Support | Yes | More flexible PRNG support in streams |
| Factory Methods | No | `RandomGeneratorFactory` introduced |

---
### How does `SplittableRandom` differ from Random in Java?

In Java, `SplittableRandom` and `Random` are both used for generating random numbers, but they have key differences:

#### Key Differences

| Feature           | `Random`                        | `SplittableRandom`                |
|------------------|--------------------------------|----------------------------------|
| **Introduced in** | Java 1.0                      | Java 8                           |
| **Thread Safety** | Thread-safe (uses synchronization) | Not thread-safe (no synchronization overhead) |
| **Performance**   | Slower due to locks           | Faster due to no locks          |
| **Splitting Capability** | No built-in method to create new independent generators | Can generate new independent random generators efficiently (`split()`) |
| **Usage in Parallel Streams** | Not well-suited | Designed for parallelism |
| **Seed**         | Uses a single seed value | Uses a 64-bit seed per instance |

#### When to Use:
- Use `Random` when working in a **single-threaded** context or when synchronization is needed.
- Use `SplittableRandom` for **parallel computations** (e.g., parallel streams) where you need independent random number generators.

#### Example Usage

### Using `Random`
```java
import java.util.Random;

public class RandomExample {
    public static void main(String[] args) {
        Random random = new Random();
        System.out.println(random.nextInt(100)); // Generates a number between 0-99
    }
}
```

### Using `SplittableRandom`
```java
import java.util.SplittableRandom;

public class SplittableRandomExample {
    public static void main(String[] args) {
        SplittableRandom splittableRandom = new SplittableRandom();
        System.out.println(splittableRandom.nextInt(100)); // Generates a number between 0-99
    }
}
```

#### Conclusion
- `Random` is **thread-safe** but slower due to synchronization.
- `SplittableRandom` is **faster** and better suited for parallel programming.
- Use `SplittableRandom` when performance matters in multi-threaded environments.

---

### How does `RandomGeneratorFactory` help in selecting different PRNGs?

`RandomGeneratorFactory` is a feature introduced in Java 17 that allows developers to select and create different **Pseudorandom Number Generators (PRNGs)** based on their needs. It provides a flexible API to list, choose, and instantiate various PRNG implementations.

#### Why Use RandomGeneratorFactory?
- **Flexibility**: Choose different PRNGs based on performance, randomness quality, and reproducibility.
- **Better Performance**: Some generators like `L128X256MixRandom` offer improved performance.
- **Reproducibility**: PRNGs support seeding, making results predictable for testing purposes.

#### Listing Available PRNGs
You can list all available PRNG implementations using the following code:

```java
import java.util.random.*;

public class ListPRNGs {
    public static void main(String[] args) {
        RandomGeneratorFactory.all()
            .map(RandomGeneratorFactory::name)
            .sorted()
            .forEach(System.out::println);
    }
}
```

#### Selecting a Specific PRNG
To use a specific PRNG, use `RandomGeneratorFactory.of("PRNG_NAME")`:

```java
import java.util.random.*;

public class SelectPRNG {
    public static void main(String[] args) {
        // Choose a PRNG (e.g., "L128X128MixRandom")
        RandomGeneratorFactory<RandomGenerator> factory = RandomGeneratorFactory.of("L128X128MixRandom");
        
        // Create an instance
        RandomGenerator rng = factory.create();

        // Generate random numbers
        System.out.println("Random Int: " + rng.nextInt());
        System.out.println("Random Double: " + rng.nextDouble());
    }
}
```

#### Common PRNG Implementations in Java 17
| PRNG Name             | Description                                  |
|----------------------|----------------------------------------------|
| `SecureRandom`      | Cryptographically strong PRNG for security  |
| `SplittableRandom`  | Fast and parallel-friendly                   |
| `L128X128MixRandom` | High-quality and efficient PRNG             |
| `L128X256MixRandom` | Better performance for large-scale systems  |
| `L64X128MixRandom`  | Good balance of speed and randomness        |

#### Conclusion
`RandomGeneratorFactory` makes it easier to work with different random number generators in Java 17. By choosing the right PRNG, developers can optimize their applications based on performance and randomness needs.

---


### How does SplittableRandom differ from Random in Java?

Java provides two main classes for generating random numbers: `SplittableRandom` and `Random`. Both serve different purposes, and understanding their differences helps in selecting the right one for your application.

#### Comparison Table

| Feature            | `SplittableRandom` | `Random` |
|--------------------|-------------------|----------|
| **Introduced In**  | Java 8            | Java 1.0 |
| **Thread Safety**  | Not thread-safe (designed for independent use) | Thread-safe (but may cause contention) |
| **Performance**    | Faster in multi-threaded environments | Slower due to atomic synchronization |
| **Parallelism**    | Designed for parallel streams and concurrent use | Not optimized for parallel execution |
| **Splitting Capability** | Can create new independent instances using `split()` | No splitting support |
| **Seed Management** | Uses a different internal algorithm with better randomness | Uses a single seed with atomic updates |
| **Usage in Streams** | Works well with parallel streams | May cause contention issues in concurrent usage |
| **Best Use Case**  | High-performance, parallel applications | Single-threaded or basic random number generation |

#### Example Usage

#### Using `SplittableRandom`
```java
import java.util.SplittableRandom;

public class SplittableRandomExample {
    public static void main(String[] args) {
        SplittableRandom random = new SplittableRandom();
        System.out.println(random.nextInt(1, 100)); // Random number between 1 and 100
    }
}
```

#### Using `Random`
```java
import java.util.Random;

public class RandomExample {
    public static void main(String[] args) {
        Random random = new Random();
        System.out.println(random.nextInt(100) + 1); // Random number between 1 and 100
    }
}
```

#### When to Use?
- Use **`SplittableRandom`** when working with **parallel streams** or **multi-threaded applications** for better performance.
- Use **`Random`** for **simple single-threaded** scenarios.

#### Conclusion
`SplittableRandom` is a better choice for high-performance applications, especially those leveraging parallelism, while `Random` remains a good option for simple use cases.

---

### Xoshiro256++ and Xoroshiro128++ PRNG Implementations in Java

This repository contains Java implementations of two high-performance pseudorandom number generators (PRNGs):

1. **Xoshiro256++** – A high-quality PRNG with a long period and better randomness.
2. **Xoroshiro128++** – A faster PRNG with a shorter period but slightly lower statistical quality.

Both algorithms were designed by **David Blackman and Sebastiano Vigna**.

---

### Differences Between `Xoshiro256++` and `Xoroshiro128++`

| Feature            | **Xoshiro256++** | **Xoroshiro128++** |
|--------------------|-----------------|--------------------|
| **State Size**     | 256 bits (4 × 64-bit) | 128 bits (2 × 64-bit) |
| **Output Quality** | Higher quality, suitable for general-purpose use | Faster but lower statistical quality |
| **Period**        | \(2^{256} - 1\) | \(2^{128} - 1\) |
| **Speed**         | Slightly slower than Xoroshiro128++ | Faster due to smaller state |
| **Suitability**   | Cryptography, Monte Carlo simulations | Performance-critical applications like simulations, procedural generation |
| **Statistical Issues** | No known weaknesses | Has slight issues with low-dimensional fractional filling in Monte Carlo simulations |

#### When to Use Which?
- **Use `Xoshiro256++`** when you need **higher statistical quality and a longer period**.
- **Use `Xoroshiro128++`** if you need a **fast PRNG for non-critical applications**.

---

### Java Implementations

#### `Xoshiro256++` Implementation in Java
```java
import java.util.Random;

public class Xoshiro256PlusPlus {
    private long[] state = new long[4];

    public Xoshiro256PlusPlus(long seed) {
        Random random = new Random(seed);
        for (int i = 0; i < 4; i++) {
            state[i] = random.nextLong();
        }
    }

    private long rotl(long x, int k) {
        return (x << k) | (x >>> (64 - k));
    }

    public long nextLong() {
        long result = rotl(state[0] + state[3], 23) + state[0];

        long t = state[1] << 17;
        state[2] ^= state[0];
        state[3] ^= state[1];
        state[1] ^= state[2];
        state[0] ^= state[3];

        state[2] ^= t;
        state[3] = rotl(state[3], 45);

        return result;
    }

    public static void main(String[] args) {
        Xoshiro256PlusPlus rng = new Xoshiro256PlusPlus(123456L);
        for (int i = 0; i < 10; i++) {
            System.out.println(rng.nextLong());
        }
    }
}
```

---

#### `Xoroshiro128++` Implementation in Java
```java
import java.util.Random;

public class Xoroshiro128PlusPlus {
    private long[] state = new long[2];

    public Xoroshiro128PlusPlus(long seed) {
        Random random = new Random(seed);
        state[0] = random.nextLong();
        state[1] = random.nextLong();
    }

    private long rotl(long x, int k) {
        return (x << k) | (x >>> (64 - k));
    }

    public long nextLong() {
        long s0 = state[0];
        long s1 = state[1];
        long result = rotl(s0 + s1, 17) + s0;

        s1 ^= s0;
        state[0] = rotl(s0, 49) ^ s1 ^ (s1 << 21);
        state[1] = rotl(s1, 28);

        return result;
    }

    public static void main(String[] args) {
        Xoroshiro128PlusPlus rng = new Xoroshiro128PlusPlus(123456L);
        for (int i = 0; i < 10; i++) {
            System.out.println(rng.nextLong());
        }
    }
}
```

---

#### Summary
- `Xoshiro256++` is **better for high-quality randomness** and **scientific applications**.
- `Xoroshiro128++` is **faster but has minor statistical flaws**.

---







