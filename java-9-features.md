## Java 9 Streams: Slicing a Stream

### Introduction
Java 9 introduced two new methods, `takeWhile` and `dropWhile`, to efficiently slice streams based on a predicate. These methods provide an optimized way to select or ignore elements without processing the entire stream, which is useful for handling large or infinite streams.

---

## 1. Slicing Using a Predicate

### `takeWhile` Method
The `takeWhile` method selects elements from a stream as long as they satisfy a given predicate. It stops processing once it encounters the first element that does not match the predicate.

#### Example:
```java
import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

public class StreamSlicingExample {
    public static void main(String[] args) {
        List<Dish> specialMenu = Arrays.asList(
            new Dish("seasonal fruit", true, 120, Dish.Type.OTHER),
            new Dish("prawns", false, 300, Dish.Type.FISH),
            new Dish("rice", true, 350, Dish.Type.OTHER),
            new Dish("chicken", false, 400, Dish.Type.MEAT),
            new Dish("french fries", true, 530, Dish.Type.OTHER)
        );

        List<Dish> slicedMenu = specialMenu.stream()
                .takeWhile(dish -> dish.getCalories() < 320)
                .collect(Collectors.toList());

        System.out.println(slicedMenu);
    }
}
```

#### Output:
```
[seasonal fruit, prawns]
```
Here, the stream stops processing as soon as it encounters "rice" (which has 350 calories and does not satisfy the condition).

---

### `dropWhile` Method
The `dropWhile` method discards elements at the beginning of the stream while they match the given predicate. Once an element fails the predicate, all subsequent elements are included in the stream.

#### Example:
```java
List<Dish> slicedMenu2 = specialMenu.stream()
        .dropWhile(dish -> dish.getCalories() < 320)
        .collect(Collectors.toList());

System.out.println(slicedMenu2);
```

#### Output:
```
[rice, chicken, french fries]
```
Here, elements are dropped while their calories are less than 320. Once "rice" (350 calories) is encountered, all remaining elements are included in the result.

---

## 2. Key Differences Between `takeWhile` and `dropWhile`

| Feature           | `takeWhile` | `dropWhile` |
|------------------|------------|------------|
| **Behavior**     | Selects elements while the predicate holds true | Skips elements while the predicate holds true |
| **Execution Stops When** | The first element fails the predicate | The first element fails the predicate, then includes all remaining elements |
| **Best Use Case** | Extracting a subset of a sorted stream | Removing unwanted leading elements from a stream |

---

### Summary
- **`takeWhile`** extracts elements from a stream until a condition is met.
- **`dropWhile`** removes elements from a stream until a condition is met.
- Both are **efficient** and help in processing large streams.

These methods improve performance compared to filtering, especially for sorted streams.

---

