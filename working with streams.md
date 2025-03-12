## Filtering

### Filtering with a Predicate
The `Stream` interface supports a `filter` method that takes a predicate (a function returning a boolean) and returns a stream including all elements that match the predicate. 

#### Example: Filtering Vegetarian Dishes
```java
List<Dish> vegetarianMenu = menu.stream()
                                .filter(Dish::isVegetarian)  
                                .collect(Collectors.toList());
```

### Filtering Unique Elements
Streams also support a method called `distinct` that returns a stream with unique elements (based on the implementation of `hashCode` and `equals` methods of the objects in the stream). 

#### Example: Filtering Unique Even Numbers
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

The `distinct()` method ensures that only unique even numbers are printed.

### `limit` Method
The `limit(n)` method returns a stream with a maximum of `n` elements. If the stream is ordered, it selects the first `n` elements. 

#### Example:
```java
List<Dish> dishes = specialMenu.stream()
        .filter(dish -> dish.getCalories() > 300)
        .limit(3)
        .collect(Collectors.toList());

System.out.println(dishes);
```

#### Output:
```
[rice, chicken, french fries]
```
Here, only the first three elements matching the predicate (`calories > 300`) are selected.

Note: `limit` also works on unordered streams (e.g., if the source is a `Set`). However, the order of the result should not be assumed in such cases.

### Skipping Elements

### `skip` Method
The `skip(n)` method returns a stream that discards the first `n` elements. If the stream has fewer than `n` elements, an empty stream is returned. The `skip(n)` method is complementary to `limit(n)`.

#### Example:
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
Here, the first two elements that match the predicate (`calories > 300`) are skipped, and the rest are collected.

---
**Question:** How would you use streams to filter the first two meat dishes?

**Answer:**
You can solve this problem by composing the `filter` and `limit` methods together and using `collect(toList())` to convert the stream into a list as follows:

```java
List<Dish> dishes = 
    menu.stream()
        .filter(dish -> dish.getType() == Dish.Type.MEAT)
        .limit(2)
        .collect(Collectors.toList());
```

This will return the first two meat dishes from the menu.


