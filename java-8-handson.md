
#### Filter Vegetarian Dishes from the given List ?

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
---
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
---
#### filter and limit the number of meat dishes in a menu using Java Streams?

```java
List<Dish> dishes = 
    menu.stream()
        .filter(dish -> dish.getType() == Dish.Type.MEAT)
        .limit(2)
        .collect(Collectors.toList());
```
---
#### How can you use Java Streams to filter even numbers and square them?
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
---
#### How can you count distinct dishes with more than 300 calories while limiting the result to 3?

```java
long count = menu.stream()
                 .filter(dish -> dish.getCalories() > 300) // Filters high-calorie dishes
                 .distinct() // Removes duplicates
                 .limit(3) // Limits to 3 dishes
                 .count(); // Counts the remaining elements
```
---
#### process a list of words, split them into characters, flatten them into a single stream, remove duplicates, and collect the unique characters into a list.

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

#### Step-by-Step Explanation
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

#### Expected Output
```
[a, p, l, e, b, n]
```
---
#### Given a list of numbers, how would you return a list of the square of each number? 
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

#### Explanation
1. **Create a list of integers**: `Arrays.asList(1, 4, 5, 9)`
2. **Convert to a stream**: `.stream()` allows for stream processing.
3. **Apply transformation (`map`)**: `.map(i -> i * i)` squares each element.
4. **Collect the results**: `.collect(Collectors.toList())` gathers squared values into a list.
5. **Print the list**: The final list is printed to the console.

**Output**
```
[1, 16, 25, 81]
```
---
#### Given two lists of numbers, how would you return all pairs of numbers? 
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

#### Step-by-Step Execution

#### **Step 1: Convert `numbers1` to a Stream**
```java
numbers1.stream()
```
- Converts `numbers1` into a stream:
  ```
  Stream<Integer> -> {1, 2, 3}
  ```

#### **Step 2: Process Each Element `i` in `numbers1`**
```java
flatMap(i -> numbers2.stream()
```
- For `i = 1`, `numbers2.stream()` creates:
  ```
  Stream<Integer> -> {3, 4}
  ```
- This step repeats for each `i` in `numbers1`.

#### **Step 3: Map Each Element `j` in `numbers2` to a Pair**
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

#### **Step 4: Flattening Streams Using `flatMap()`**
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

#### **Step 5: Collecting the Results**
```java
.collect(Collectors.toList());
```
- Converts the **flattened stream** into a `List<int[]>`.

#### **Step 6: Printing the Output**
```java
for (int[] ks : pairs) {
    System.out.println(Arrays.toString(ks));
}
```
- Prints the pairs in a readable format.

#### Output
```
[1, 3]
[1, 4]
[2, 3]
[2, 4]
[3, 3]
[3, 4]
``` 
---
#### How would you extend the previous example to return only pairs whose sum is divisible by 3?

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

#### Step-by-Step Execution

#### **Step 1: Convert `numbers1` to a Stream**
```java
numbers1.stream()
```
- Converts `numbers1` into a stream:
  ```
  Stream<Integer> -> {1, 2, 3}
  ```

#### **Step 2: Process Each Element `i` in `numbers1`**
```java
flatMap(i -> numbers2.stream()
```
- For `i = 1`, `numbers2.stream()` creates:
  ```
  Stream<Integer> -> {3, 4}
  ```
- This step repeats for each `i` in `numbers1`.

#### **Step 3: Apply Filtering Condition**
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

#### **Step 4: Flattening Streams Using `flatMap()`**
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

#### **Step 5: Collecting the Results**
```java
.collect(Collectors.toList());
```
- Converts the **flattened stream** into a `List<int[]>`.

#### **Step 6: Printing the Output**
```java
for (int[] ks : list) {
    System.out.println(Arrays.toString(ks));
}
```
- Prints the filtered pairs in a readable format.

#### Output
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
---
#### Write a Java 8 method that takes a list of integers and returns a new list containing only the even numbers. 

```java
List<Integer> numbers1 = Arrays.asList(1, 2, 3, 4)
				.stream()
				.filter(i ->i%2==0)
				.collect(Collectors.toList());
		System.out.println(numbers1);
```
```
output : [2, 4]
```
---
#### Write a Java 8 method that takes a list of strings and a character, and returns a new list containing only the strings that start with that character. 

#### Different Approaches

#### 1️⃣ Best Approach: Using `mapToInt().sum()` ✅
```java
List<Integer> words = List.of(1, 2, 3, 4, 5);
int sum = words.stream()
    .filter(num -> num % 2 == 0)
    .mapToInt(num -> num)
    .sum();
System.out.println(sum);
```
#### ✅ Why Best?
- **Fastest**: Uses **primitive int stream**, avoiding unnecessary boxing/unboxing.
- **Short & Readable**: Computes sum in a single pass.
- **No Optional Handling Needed**: Directly returns `int`.

---

#### 2️⃣ Alternative Approach: Using `reduce()` with `Optional<Integer>`
```java
List<Integer> words = List.of(1, 2, 3, 4, 5);
Optional<Integer> optionalInteger = words.stream()
    .filter(num -> num % 2 == 0)
    .reduce((num1, num2) -> num1 + num2);
System.out.println(optionalInteger.orElse(0));
```
#### 🟡 Why Second?
- **Less Efficient**: Uses **auto-boxing**, leading to performance overhead.
- **Requires Optional Handling**: Needs `orElse(0)` to handle empty lists safely.
- **Useful for Custom Reduction Logic**: Best when more than just sum is needed.

---

#### 3️⃣ Worst Approach: Using `Collectors.summarizingInt()` ❌
```java
List<Integer> words = List.of(1, 2, 3, 4, 5);
IntSummaryStatistics intSummaryStatistics = words.stream()
    .filter(num -> num % 2 == 0)
    .collect(Collectors.summarizingInt(i -> i));
System.out.println(intSummaryStatistics.getSum());
```
#### ❌ Why Worst?
- **Unnecessary Computation Overhead**: Calculates min, max, count, and average when only sum is needed.
- **More Memory Usage**: Stores extra statistics that are not needed.
- **Less Readable**: Adds unnecessary complexity.

---
#### **output :** 6 
---
#### Final Ranking
| Approach | Performance | Readability | Suitability |
|----------|------------|-------------|-------------|
| `mapToInt().sum()` ✅ | **Best** (Primitive Stream) | **Best** (Simple & Direct) | **Best for Summing** |
| `reduce()` 🟡 | **Slower** (Auto-boxing overhead) | **Okay** (Needs Optional Handling) | **Better for Custom Reduction** |
| `Collectors.summarizingInt()` ❌ | **Worst** (Unnecessary computations) | **Worst** (Extra memory use) | **Bad Choice for Just Sum** |

---
#### Write a Java 8 method that takes a list of strings and returns the length of the longest string in the list.

```java
List<String> words = List.of("apple", "banana", "cherry", "blueberry");
		OptionalInt optionalInt = words.stream()
		.mapToInt(string -> string.length())
		.max();
		System.out.println(optionalInt.getAsInt());
 ```
 ```
 output:9
 ```
---
 #### Write a Java 8 method that takes a list of integers and returns a new list containing only the odd numbers, sorted in ascending order.

 ```java    
 List<Integer> numbers = List.of(5, 2, 8, 3, 1, 7, 4);
		List<Integer> oddNumberList = 
		 numbers.stream()
				.filter(num -> num%2!=0)
				.sorted()
				.collect(Collectors.toList());
		System.out.println(oddNumberList);  
```
```
output : [1, 3, 5, 7]
```
---
#### Write a Java 8 method that takes a list of strings and returns a new list containing the first letter of each string, in uppercase.

```java
List<String> words = List.of("apple", "banana", "cherry", "blueberry");
		List<String> list = words.stream()
		.map(string -> string.substring(0,1).toUpperCase() + string.substring(1))
		.collect(Collectors.toList());
		System.out.println(list);
```
```
output : [Apple, Banana, Cherry, Blueberry]
```
---
#### Write a Java 8 method that takes a list of integers and returns the product of all the numbers in the list.

```java
List<Integer> numbers = List.of(5, 2, 8, 3, 1, 7, 4);
		Integer integer = 
				numbers.stream()
				.mapToInt(num -> num)
				.reduce(1,(num1,num2) -> num1*num2);
		System.out.println(integer);
 ```
 ```
 output: 6720
 ```
 ---
 #### Write a Java 8 method that takes a list of strings and returns a new list containing only the strings that have a length of three.        
 ```java
 List<String> words = List.of("apple", "banana", "cherry", "blueberry", "abc");
		List<String> list = words.stream()
			 .filter(string -> string.length() == 3)
			 .collect(Collectors.toList());
		System.out.println(list);
 ```
 ```
 output : [abc]
 ```
 ---
  #### Write a Java 8 method that takes a list of strings and returns a new list containing only the strings that contain the letter 'a' in them. 

  ```java
  List<String> words = List.of("apple", "banana", "cherry", "blueberry", "abc");
		List<String> list = words.stream()
			.filter(string -> string.contains("a"))
			.collect(Collectors.toList());
		System.out.println(list);
 ```
 ```
 output: [apple, banana, abc]
 ```
 ---
#### Write a Java 8 lambda expression to sort a list of integers in descending order.
```java
List<Integer> list = Arrays.asList(1,2,3,4,5,6,7,4,66,0,5)
		.stream()
		.sorted((num1,num2)-> num2-num1)
		.collect(Collectors.toList());
		System.out.println(list);
 ```
 ```
 output : [66, 7, 6, 5, 5, 4, 4, 3, 2, 1, 0]
 ```
 ---
#### Write a Java 8 method that takes a list of strings and returns a new list containing the strings sorted in reverse alphabetical order.   
```java
 List<String> words = List.of("apple", "banana", "cherry", "blueberry", "abc");
		 List<String> list = words.stream()
		 .sorted(Comparator.reverseOrder())
		 .collect(Collectors.toList());
		 System.out.println(list);	 
```
```
output : [cherry, blueberry, banana, apple, abc]
```   
---           
#### Write a Java 8 method that takes a list of integers and returns the sum of the square of each number in the list. 

```java
List<Integer> numbers = List.of(5, 2, 8, 3, 1, 7, 4);
		Integer integer = numbers.stream()
				.mapToInt(num -> num*num)
				.sum();
		System.out.println(integer);
```
```
output :  168
```    
---    
#### Write a Java 8 method that takes a list of strings and returns a new list containing only the strings that have more than five characters. 
```java
List<String> words = List.of("apple", "banana", "cherry", "blueberry", "abc");
		List<String> list = words.stream()
		.filter(string -> string.length()>5)
		.collect(Collectors.toList());
		System.out.println(list);
 ```
 ```
 output : [banana, cherry, blueberry]       
```
---
#### Write a Java 8 method that takes a list of strings and returns a new list containing the strings that have the first letter capitalized and the rest of the letters in lowercase.
```java
List<String> words = List.of("aPPle", "banAna", "cHerry", "bluEberry", "abC");
		List<String> list = words.stream()
				.map(string -> string.substring(0, 1).toUpperCase() + string.substring(1).toLowerCase())
				.collect(Collectors.toList());
		System.out.println(list);
```
```
output : [Apple, Banana, Cherry, Blueberry, Abc]
```
---
#### Write a Java 8 method that takes a list of integers and returns the average of all the odd numbers in the list.
```java
List<Integer> numbers = List.of(5, 2, 8, 3, 1, 7, 4);
		OptionalDouble average = numbers.stream()
			   .filter(num -> num%2!=0)
			   .mapToInt(num -> num)
			   .average();
		System.out.println(average.getAsDouble());
 ```
 ```
 output : 4.0
```  
---     
#### Write a Java 8 method that takes a list of strings and returns a new list containing the strings that have the letter 'e' as the second letter. 

```java
List<String> words = List.of("aPPle", "banAna", "cHerry", "bluEberry", "abC", "bed");
		List<String> list = words.stream().filter(string -> string.substring(1, 2).contains("e"))
				.collect(Collectors.toList());
		System.out.println(list);

```
```
output : [bed]
```   
---
#### Write a Java 8 method that takes a list of strings and returns the number of strings that contain the letter 'o' in them.
```java
List<String> words = List.of("aPPle", "banAna", "cHerry", "bluEberry", "abC", "bed");
		long count = words.stream().filter(string -> string.contains("o"))
				.count();
		System.out.println(count); 
```
```
output : 0
```
---
#### Write a Java 8 lambda expression to sort a list of doubles in ascending order.
```java
List<Double> numbers = List.of(3.5, 1.2, 4.8, 2.9);
		List<Double> list = numbers.stream()
			   .sorted((d1,d2) -> Double.compare(d1, d2))
			   .collect(Collectors.toList());
		System.out.println(list);
```
```
Output : [1.2, 2.9, 3.5, 4.8]
```  
--- 
#### Write a Java 8 method that takes a list of strings and returns a new list containing only the strings that are not longer than four characters.  
```java
List<String> words = List.of("aPPle", "banAna", "cHerry", "bluEberry", "abC", "bed");
		List<String> list = words.stream()
				.filter(string -> string.length() <= 3)
				.collect(Collectors.toList());
		System.out.println(list);
```
```
output : [abC, bed]
```
---        
#### Write a Java 8 method that takes a list of integers and returns the product of all the odd numbers in the list. 
```java
List<Integer> numbers = List.of(5, 2, 8, 3, 1, 7, 4);
		Integer integer = numbers.stream()
		.filter(num -> num%2!=0)
		.reduce(1,(num1,num2) -> num1*num2);
		System.out.println(integer);
```
```
output : 105
```
---
 #### Write a Java 8 method that takes a list of strings and returns the total number of characters in all the strings. 
 ```java
 List<String> words = List.of("apple", "banana", "cherry", "blueberry", "abc", "bed");
		List<Integer> list = words.stream()
				.map(string -> string.length())
				.collect(Collectors.toList());
		System.out.println(list);
```
```
output : [5, 6, 6, 9, 3, 3]
```
---
#### Write a Java 8 method that takes a list of strings and returns a new list containing only the strings that start with a vowel. 
```java
String vowels = "AEIOUaeiou";
		List<String> words = List.of("apple", "banana", "cherry", "blueberry", "abc", "bed");
		List<String> list2 = words.stream()
		.filter(string -> vowels.contains(string.substring(0,1)))
		.collect(Collectors.toList());
		System.out.println(list2);        
```
```
output : [apple, abc]
```
---
#### Write a Java 8 method that takes a list of strings and returns a new list containing only the strings that end with the letter 's'. 

```java
List<String> words = List.of("apple", "banana", "cherry", "blueberry", "singles");
		List<String> list = words.stream()
		.filter(string -> string.endsWith("s"))
		.collect(Collectors.toList());
		System.out.println(list);
```
```
output : [singles]
```
---
 #### Write a Java 8 lambda expression to sort a list of Doubles in descending order.
 ```java
 List<Double> numbers = List.of(3.5, 1.2, 4.8, 2.9);
		List<Double> list = numbers.stream()
		       .sorted((num1,num2) -> Double.compare(num2,num1))
		       .collect(Collectors.toList());
		System.out.println(list);
```
```
output : [4.8, 3.5, 2.9, 1.2]
```
---
#### Write a Java 8 method that takes a list of strings and returns a new list containing the strings that have at least one digit in them.

```java
List<String> words = List.of("apple", "hello123", "world", "java8", "test1ng", "code");

words.stream()
     .filter(s -> s.chars().anyMatch(Character::isDigit))
     .collect(Collectors.toList());

```
---

#### Explanation

#### **1️⃣ `s.chars()` - Convert String to IntStream**
Each character in the string is converted to its **ASCII (Unicode) value**:
```java
"java8".chars() → Stream of [106, 97, 118, 97, 56]
```
- `'j'` → 106
- `'a'` → 97
- `'v'` → 118
- `'a'` → 97
- `'8'` → 56

#### **2️⃣ `.anyMatch(Character::isDigit)` - Check for Digits**
- **`Character.isDigit(c)`** checks if a character is a digit (`0-9`).
- **`.anyMatch()`** stops processing once it finds the first digit (short-circuiting).

#### **Example Execution Table**
| String     | `s.chars()` Output | Any Digit? | Keep? |
|------------|---------------------------|------------|------|
| `"apple"`  | `[97, 112, 112, 108, 101]` | ❌ No digits | ❌ Remove |
| `"hello123"` | `[104, 101, 108, 108, 111, 49, 50, 51]` | ✅ Digits found | ✅ Keep |
| `"world"`  | `[119, 111, 114, 108, 100]` | ❌ No digits | ❌ Remove |
| `"java8"`  | `[106, 97, 118, 97, 56]` | ✅ Digits found | ✅ Keep |
| `"test1ng"` | `[116, 101, 115, 116, 49, 110, 103]` | ✅ Digits found | ✅ Keep |
| `"code"`  | `[99, 111, 100, 101]` | ❌ No digits | ❌ Remove |

✅ **Final Output:**
```java
[hello123, java8, test1ng]
```
---


   

