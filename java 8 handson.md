
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
