
### Filter Vegetarian Dishes from the given List ?

**Dish.java**
```java
import java.util.*;
import java.util.stream.Collectors;

class Dish {
    private final String name;
    private final boolean vegetarian;
    private final int calories;

    public Dish(String name, boolean vegetarian, int calories) {
        this.name = name;
        this.vegetarian = vegetarian;
        this.calories = calories;
    }

    public boolean isVegetarian() {
        return vegetarian;
    }

    public String getName() {
        return name;
    }

    @Override
    public String toString() {
        return String.format("%s (%s, %d kcal)", name, vegetarian ? "Vegetarian" : "Non-Vegetarian", calories);
    }
}

```
**MenuService.java**

```java
class MenuService {
    public static List<Dish> getVegetarianDishes(List<Dish> menu) {
        return menu.stream()
                   .filter(Dish::isVegetarian)
                   .collect(Collectors.toList());
    }
}
```
**VegetarianDishFilter.java**

```java
public class VegetarianDishFilter {
    public static void main(String[] args) {
        List<Dish> menu = Arrays.asList(
            new Dish("Pasta", true, 350),
            new Dish("Chicken Curry", false, 500),
            new Dish("Salad", true, 150),
            new Dish("Steak", false, 700),
            new Dish("Paneer Tikka", true, 400)
        );

        List<Dish> vegetarianMenu = MenuService.getVegetarianDishes(menu);
        
        System.out.println("Vegetarian Dishes: " + vegetarianMenu);
    }
}
```

#### Filter Unique Even Numbers from the List ?
```java
List<Integer> numbers = Arrays.asList(1, 2, 1, 3, 3, 2, 4);
numbers.stream()
       .filter(i -> i % 2 == 0)
       .distinct()
       .forEach(System.out::println);
```
**Output:**
```
2
4
```

### Get the first three elements whose `calories > 300`

```java
List<Dish> dishes = menu.stream()
        .filter(d -> d.getCalories() > 300)
        .skip(2)
        .collect(Collectors.toList());

System.out.println(dishes);
```

#### Output:
```
[french fries]
```

### filter and limit the number of meat dishes in a menu using Java Streams?

```java
List<Dish> dishes = 
    menu.stream()
        .filter(dish -> dish.getType() == Dish.Type.MEAT)
        .limit(2)
        .collect(Collectors.toList());
```

### How can you use Java Streams to filter even numbers and square them?
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(2, 3, 4, 5, 6, 7);

        // Intermediate Operations (lazy)
        List<Integer> squaredNumbers = numbers.stream()
                .filter(n -> n % 2 == 0)  // Intermediate
                .map(n -> n * n)          // Intermediate
                .collect(Collectors.toList()); // Terminal

        System.out.println(squaredNumbers); // Output: [4, 16, 36]
    }
}
```
### How can you filter dishes with more than 300 calories?
```java
List<String> names = menu.stream()
        .filter(dish -> {
            System.out.println("Filtering: " + dish.getName());
            return dish.getCalories() > 300;
        })
        .map(dish -> {
            System.out.println("Mapping: " + dish.getName());
            return dish.getName();
        })
        .limit(3)
        .collect(Collectors.toList());
```
**console output:**
```
Filtering: Pasta
Mapping: Pasta
Filtering: Chicken Curry
Mapping: Chicken Curry
Filtering: Steak
Mapping: Steak
```
**Final output**
```
["Pasta", "Chicken Curry", "Steak"]
```

### How can you count distinct dishes with more than 300 calories while limiting the result to 3?

```java
long count = menu.stream()
                 .filter(dish -> dish.getCalories() > 300) // Filters high-calorie dishes
                 .distinct() // Removes duplicates
                 .limit(3) // Limits to 3 dishes
                 .count(); // Counts the remaining elements
```

### process a list of words, split them into characters, flatten them into a single stream, remove duplicates, and collect the unique characters into a list.

```java
public class MergeArraysDemo {
	public static void main(String[] args) {

		// {"apple", "banana"}
		List<String> words = Arrays.asList("apple", "banana");

		List<String> uniqueCharacters = words.stream()
				// ["a", "p", "p", "l", "e"], ["b", "a", "n", "a", "n", "a"]
				.map(word -> word.split(""))
				// ["a", "p", "p", "l", "e"] + ["b", "a", "n", "a", "n", "a"]
				// =="a", "p", "p", "l", "e", "b", "a", "n", "a", "n", "a"
				.flatMap(Arrays::stream)
				// "a", "p", "l", "e", "b", "n"
				.distinct().collect(Collectors.toList());

		// Print the unique characters
		System.out.println(uniqueCharacters);
	}
}
```

### Step-by-Step Explanation
1. **Splitting Words into Character Arrays (`map(word -> word.split(""))`)**
   - Each word is split into an array of characters.
   - Example: 
     ```
     "apple"  -> ["a", "p", "p", "l", "e"]
     "banana" -> ["b", "a", "n", "a", "n", "a"]
     ```

2. **Flattening Arrays into a Single Stream (`flatMap(Arrays::stream)`)**
   - This converts multiple arrays into a single stream of characters.
   - Example:
     ```
     ["a", "p", "p", "l", "e"] + ["b", "a", "n", "a", "n", "a"]
     -> Stream: "a", "p", "p", "l", "e", "b", "a", "n", "a", "n", "a"
     ```

3. **Removing Duplicates (`distinct()`)**
   - Eliminates duplicate characters from the stream.
   - Example:
     ```
     "a", "p", "p", "l", "e", "b", "a", "n", "a", "n", "a" 
     -> "a", "p", "l", "e", "b", "n"
     ```

4. **Collecting into a List (`collect(Collectors.toList())`)**
   - Stores the final unique characters into a `List<String>`.

### Expected Output
```
[a, p, l, e, b, n]
```

### Given a list of numbers, how would you return a list of the square of each number? 
**For example, given [1, 2, 3, 4, 5] you should return [1, 4, 9, 16, 25].**

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class MergeArraysDemo {
    public static void main(String[] args) {

        // Step 1: Create a list of integers
        List<Integer> list = Arrays.asList(1, 4, 5, 9)
            .stream()  // Step 2: Convert the list into a stream
            .map(i -> i * i) // Step 3: Square each element
            .collect(Collectors.toList()); // Step 4: Collect the squared values into a new list
        
        // Step 5: Print the resulting list
        System.out.println(list);
    }
}
```

### Explanation
1. **Create a list of integers**: `Arrays.asList(1, 4, 5, 9)`
2. **Convert to a stream**: `.stream()` allows for stream processing.
3. **Apply transformation (`map`)**: `.map(i -> i * i)` squares each element.
4. **Collect the results**: `.collect(Collectors.toList())` gathers squared values into a list.
5. **Print the list**: The final list is printed to the console.

**Output**
```
[1, 16, 25, 81]
```
### Given two lists of numbers, how would you return all pairs of numbers? 
**For example, given a list [1, 2, 3] and a list [3, 4] you should return [(1, 3), (1, 4), (2, 3), (2, 4),(3, 3), (3, 4)].**


```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class MergeArraysDemo {
    public static void main(String[] args) {
        List<Integer> numbers1 = Arrays.asList(1, 2, 3);
        List<Integer> numbers2 = Arrays.asList(3, 4);
        
        // Generating pairs using Java Streams
        List<int[]> pairs = 
            numbers1.stream()
                    .flatMap(i -> numbers2.stream()
                                          .map(j -> new int[]{i, j})
                    )
                    .collect(Collectors.toList());
        
        // Printing the pairs
        for (int[] ks : pairs) {
            System.out.println(Arrays.toString(ks));        
        }
    }
}
```

### Step-by-Step Execution

### **Step 1: Convert `numbers1` to a Stream**
```java
numbers1.stream()
```
- Converts `numbers1` into a stream:
  ```
  Stream<Integer> -> {1, 2, 3}
  ```

### **Step 2: Process Each Element `i` in `numbers1`**
```java
flatMap(i -> numbers2.stream()
```
- For `i = 1`, `numbers2.stream()` creates:
  ```
  Stream<Integer> -> {3, 4}
  ```
- This step repeats for each `i` in `numbers1`.

### **Step 3: Map Each Element `j` in `numbers2` to a Pair**
```java
.map(j -> new int[]{i, j})
```
For each `j` from `numbers2`, create an `int[]` containing `(i, j)`:

| i (from numbers1) | j (from numbers2) | Output (`new int[]{i, j}`) |
|------------------|------------------|--------------------------|
| 1               | 3                | [1,3]                    |
| 1               | 4                | [1,4]                    |
| 2               | 3                | [2,3]                    |
| 2               | 4                | [2,4]                    |
| 3               | 3                | [3,3]                    |
| 3               | 4                | [3,4]                    |

### **Step 4: Flattening Streams Using `flatMap()`**
Before `flatMap()`, we have multiple individual streams of `int[]` pairs:
```
Stream<int[]> from i=1 → [1,3], [1,4]
Stream<int[]> from i=2 → [2,3], [2,4]
Stream<int[]> from i=3 → [3,3], [3,4]
```
After `flatMap()`, these streams are merged into a **single** stream:
```
Single Stream<int[]> → [[1,3], [1,4], [2,3], [2,4], [3,3], [3,4]]
```

### **Step 5: Collecting the Results**
```java
.collect(Collectors.toList());
```
- Converts the **flattened stream** into a `List<int[]>`.

### **Step 6: Printing the Output**
```java
for (int[] ks : pairs) {
    System.out.println(Arrays.toString(ks));
}
```
- Prints the pairs in a readable format.

### Output
```
[1, 3]
[1, 4]
[2, 3]
[2, 4]
[3, 3]
[3, 4]
``` 
### How would you extend the previous example to return only pairs whose sum is divisible by 3?

```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class MergeArraysDemo {
    public static void main(String[] args) {
        List<Integer> numbers1 = Arrays.asList(1, 2, 3);
        List<Integer> numbers2 = Arrays.asList(3, 4);
        
        // Generating filtered pairs using Java Streams
        List<int[]> list = numbers1.stream()
            .flatMap(i -> numbers2.stream()
                      .filter(j -> (i + j) % 3 == 0) // Filter condition
                      .map(j -> new int[]{i, j})
                    )
            .collect(Collectors.toList());
        
        // Printing the pairs
        for (int[] ks : list) {
            System.out.println(Arrays.toString(ks));        
        }
    }
}
```

### Step-by-Step Execution

### **Step 1: Convert `numbers1` to a Stream**
```java
numbers1.stream()
```
- Converts `numbers1` into a stream:
  ```
  Stream<Integer> -> {1, 2, 3}
  ```

### **Step 2: Process Each Element `i` in `numbers1`**
```java
flatMap(i -> numbers2.stream()
```
- For `i = 1`, `numbers2.stream()` creates:
  ```
  Stream<Integer> -> {3, 4}
  ```
- This step repeats for each `i` in `numbers1`.

### **Step 3: Apply Filtering Condition**
```java
.filter(j -> (i + j) % 3 == 0)
```
- Only pairs where `(i + j) % 3 == 0` are kept.

| i (from numbers1) | j (from numbers2) | Sum `(i + j)` | Condition `(i + j) % 3 == 0` | Output (`new int[]{i, j}`) |
|------------------|------------------|-------------|------------------------------|--------------------------|
| 1               | 3                | 4           | ❌ (4 % 3 != 0)              | Not included            |
| 1               | 4                | 5           | ❌ (5 % 3 != 0)              | Not included            |
| 2               | 3                | 5           | ❌ (5 % 3 != 0)              | Not included            |
| 2               | 4                | 6           | ✅ (6 % 3 == 0)              | [2,4]                    |
| 3               | 3                | 6           | ✅ (6 % 3 == 0)              | [3,3]                    |
| 3               | 4                | 7           | ❌ (7 % 3 != 0)              | Not included            |

### **Step 4: Flattening Streams Using `flatMap()`**
Before `flatMap()`, we have multiple individual streams of `int[]` pairs:
```
Stream<int[]> from i=1 → []
Stream<int[]> from i=2 → [2,4]
Stream<int[]> from i=3 → [3,3]
```
After `flatMap()`, these streams are merged into a **single** stream:
```
Single Stream<int[]> → [[2,4], [3,3]]
```

### **Step 5: Collecting the Results**
```java
.collect(Collectors.toList());
```
- Converts the **flattened stream** into a `List<int[]>`.

### **Step 6: Printing the Output**
```java
for (int[] ks : list) {
    System.out.println(Arrays.toString(ks));
}
```
- Prints the filtered pairs in a readable format.

### Output
```
[2, 4]
[3, 3]
```
---

Suppose we have a list of integers, and we want to **filter even numbers**, **double them**, and **collect them into a list**.
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamExample {
    public static void main(String[] args) {
        List<Integer> numbers = Arrays.asList(1, 2, 3, 4, 5, 6);

        // Stream Processing: Filter even numbers, double them, and collect to a list
        List<Integer> processedNumbers = numbers.stream()
                .filter(n -> n % 2 == 0)  // Step 1: Filter even numbers
                .map(n -> n * 2)         // Step 2: Double each number
                .collect(Collectors.toList()); // Step 3: Collect results into a list

        System.out.println(processedNumbers); // Output: [4, 8, 12]
    }
}
```

