## **Question 37: What are Comparable and Comparator?**

**Answer:**\
Both `Comparable` and `Comparator` interfaces are used for sorting collections of objects in Java. These interfaces should be implemented in custom classes to use sorting methods from `Arrays` and `Collections` classes.

## **Key Differences & Usage**

### **1. Comparable (`java.lang.Comparable`)**

- Defines natural ordering of objects.
- Requires implementing the `compareTo(T obj)` method.
- Sorting logic is defined within the entity class itself.
- Used when a class has a single natural sorting order.

#### **Sorting Rules in `compareTo(T obj)`:**

- Return **negative integer** if `this` object is less than the passed object.
- Return **positive integer** if `this` object is greater than the passed object.
- Return **zero** if both objects are equal.

### **2. Comparator (`java.util.Comparator`)**

- Defines custom sorting outside the entity class.
- Requires implementing the `compare(T obj1, T obj2)` method.
- Can have multiple sorting criteria (e.g., sorting by name, salary, age).

## **Examples**

### **1. Comparable Example (Sorting by Name)**

#### **Employee.java**

```java
class Employee implements Comparable<Employee> {
    int id;
    String name;
    double salary;

    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    @Override
    public int compareTo(Employee other) {
        return this.name.compareTo(other.name);
    }
    
    @Override
    public String toString() {
        return id + " - " + name + " - " + salary;
    }
}
```

#### **ComparableDemo.java**

```java
import java.util.*;

public class ComparableDemo {
    public static void main(String[] args) {
        List<Employee> employees = new ArrayList<>();
        employees.add(new Employee(101, "Charlie", 55000));
        employees.add(new Employee(102, "Alice", 50000));
        employees.add(new Employee(103, "Bob", 60000));

        Collections.sort(employees);
        
        System.out.println("Employees sorted by name:");
        for (Employee emp : employees) {
            System.out.println(emp);
        }
    }
}
```

#### **Output:**

```
Employees sorted by name:
102 - Alice - 50000.0
103 - Bob - 60000.0
101 - Charlie - 55000.0
```

### **2. Comparator Example (Sorting by Salary)**

#### **SalaryComparator.java**

```java
import java.util.*;

class SalaryComparator implements Comparator<Employee> {
    @Override
    public int compare(Employee e1, Employee e2) {
        return Double.compare(e1.salary, e2.salary);
    }
}
```

#### **ComparatorDemo.java**

```java
import java.util.*;

public class ComparatorDemo {
    public static void main(String[] args) {
        List<Employee> employees = new ArrayList<>();
        employees.add(new Employee(101, "Charlie", 55000));
        employees.add(new Employee(102, "Alice", 50000));
        employees.add(new Employee(103, "Bob", 60000));

        Collections.sort(employees, new SalaryComparator());
        
        System.out.println("Employees sorted by salary:");
        for (Employee emp : employees) {
            System.out.println(emp);
        }
    }
}
```

#### **Output:**

```
Employees sorted by salary:
102 - Alice - 50000.0
101 - Charlie - 55000.0
103 - Bob - 60000.0
```

---

## **Question 38: How to compare a list of Employees based on name and age such that if name of the employee is the same, then sorting should be based on age?**

**Answer:**
We can use a `Comparator` to implement this sorting logic by first comparing names and then comparing ages if names are equal.

### **Employee.java**

```java
class Employee {
    int id;
    String name;
    int age;

    public Employee(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
    
    @Override
    public String toString() {
        return id + " - " + name + " - " + age;
    }
}
```

### **NameAndAgeComparator.java**

```java
import java.util.*;

class NameAndAgeComparator implements Comparator<Employee> {
    @Override
    public int compare(Employee e1, Employee e2) {
        int nameCompare = e1.name.compareTo(e2.name);
        if (nameCompare == 0) {
            return Integer.compare(e1.age, e2.age);
        }
        return nameCompare;
    }
}
```

### **EmployeeSortDemo.java**

```java
import java.util.*;

public class EmployeeSortDemo {
    public static void main(String[] args) {
        List<Employee> employees = new ArrayList<>();
        employees.add(new Employee(101, "Alice", 30));
        employees.add(new Employee(102, "Bob", 25));
        employees.add(new Employee(103, "Alice", 25));
        employees.add(new Employee(104, "Charlie", 35));
        employees.add(new Employee(105, "Bob", 28));
        
        Collections.sort(employees, new NameAndAgeComparator());
        
        System.out.println("Employees sorted by name and age:");
        for (Employee emp : employees) {
            System.out.println(emp);
        }
    }
}
```

### **Output:**

```
Employees sorted by name and age:
103 - Alice - 25
101 - Alice - 30
102 - Bob - 25
105 - Bob - 28
104 - Charlie - 35
```

This ensures that employees are sorted by name, and if the names are the same, they are sorted by age in ascending order.

---

## **Question 39: Difference between Comparable and Comparator**

| Feature       | Comparable | Comparator |
|--------------|------------|------------|
| Package      | `java.lang` | `java.util` |
| Method       | `compareTo(T obj)` | `compare(T obj1, T obj2)` |
| Sorting Logic | Defined inside the class | Defined externally |
| Multiple Sorting Criteria | No | Yes |
| Example Usage | `Collections.sort(list)` | `Collections.sort(list, comparator)` |

Use `Comparable` when the natural order of objects needs to be defined within the class. Use `Comparator` when custom sorting is needed, or when sorting logic should be external to the entity class.

