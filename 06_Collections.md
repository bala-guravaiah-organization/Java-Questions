### Explain Comaprable and Comparator and their differences ?

Both `Comparable` and `Comparator` interfaces are used for sorting collections of objects in Java. These interfaces should be implemented in custom classes to use sorting methods from `Arrays` and `Collections` classes.

#### Key Differences & Usage

### 1. Comparable (`java.lang.Comparable`)
- ✅ Defines **natural ordering** of objects.
- ✅ Requires implementing the `compareTo(T obj)` method.
- ✅ Sorting logic is **defined within the entity class** itself.
- ✅ Used when a class has a **single natural sorting order**.

#### Sorting Rules in `compareTo(T obj)`:
- Return **negative integer** if `this` object is **less than** the passed object.
- Return **positive integer** if `this` object is **greater than** the passed object.
- Return **zero** if both objects are **equal**.

### 2. Comparator (`java.util.Comparator`)
- ✅ Defines **custom sorting** outside the entity class.
- ✅ Requires implementing the `compare(T obj1, T obj2)` method.
- ✅ Can have **multiple sorting criteria** (e.g., sorting by name, salary, age).
- ✅ Allows sorting **without modifying** the original entity class.

#### Examples

#### 1. Comparable Example (Sorting by Name)

#### **Employee.java**
```java
// Employee class implementing Comparable to define natural ordering (by name)
class Employee implements Comparable<Employee> {
    int id;
    String name;
    double salary;

    // Constructor to initialize Employee object
    public Employee(int id, String name, double salary) {
        this.id = id;
        this.name = name;
        this.salary = salary;
    }

    // Implementing compareTo method to sort employees by name
    @Override
    public int compareTo(Employee other) {
        return this.name.compareTo(other.name);
    }
    
    // toString method to print Employee details
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
        // Creating a list of employees
        List<Employee> employees = new ArrayList<>();
        employees.add(new Employee(101, "Charlie", 55000));
        employees.add(new Employee(102, "Alice", 50000));
        employees.add(new Employee(103, "Bob", 60000));

        // Sorting employees by name using Comparable
        Collections.sort(employees);
        
        // Display sorted employees
        System.out.println("Employees sorted by name:");
        for (Employee emp : employees) {
            System.out.println(emp);
        }
    }
}
```

#### 2. Comparator Example (Sorting by Salary)

#### **SalaryComparator.java**
```java
import java.util.*;

// Comparator to sort employees by salary
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
        // Creating a list of employees
        List<Employee> employees = new ArrayList<>();
        employees.add(new Employee(101, "Charlie", 55000));
        employees.add(new Employee(102, "Alice", 50000));
        employees.add(new Employee(103, "Bob", 60000));

        // Sorting employees by salary using Comparator
        Collections.sort(employees, new SalaryComparator());
        
        // Display sorted employees
        System.out.println("Employees sorted by salary:");
        for (Employee emp : employees) {
            System.out.println(emp);
        }
    }
}
```

#### 3. Sorting by Name and Age

#### **Employee.java**
```java
// Employee class without Comparable, sorting handled externally via Comparator
class Employee {
    int id;
    String name;
    int age;

    // Constructor to initialize Employee object
    public Employee(int id, String name, int age) {
        this.id = id;
        this.name = name;
        this.age = age;
    }
    
    // toString method to print Employee details
    @Override
    public String toString() {
        return id + " - " + name + " - " + age;
    }
}
```

#### **NameAndAgeComparator.java**
```java
import java.util.*;

// Comparator to sort employees first by name, then by age
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

#### **EmployeeSortDemo.java**
```java
import java.util.*;

public class EmployeeSortDemo {
    public static void main(String[] args) {
        // Creating a list of employees
        List<Employee> employees = new ArrayList<>();
        employees.add(new Employee(101, "Alice", 30));
        employees.add(new Employee(102, "Bob", 25));
        employees.add(new Employee(103, "Alice", 25));
        employees.add(new Employee(104, "Charlie", 35));
        employees.add(new Employee(105, "Bob", 28));
        
        // Sorting employees by name and age using Comparator
        Collections.sort(employees, new NameAndAgeComparator());
        
        // Display sorted employees
        System.out.println("Employees sorted by name and age:");
        for (Employee emp : employees) {
            System.out.println(emp);
        }
    }
}
```

#### Output:
```
Employees sorted by name and age:
103 - Alice - 25
101 - Alice - 30
102 - Bob - 25
105 - Bob - 28
104 - Charlie - 35
```

#### Difference between Comparable and Comparator

| Feature       | Comparable 🟢 | Comparator 🔵 |
|--------------|--------------|-------------|
| **Package**  | `java.lang`  | `java.util`  |
| **Method**   | `compareTo(T obj)` | `compare(T obj1, T obj2)` |
| **Sorting Logic** | Defined **inside** the class | Defined **externally** |
| **Multiple Sorting Criteria** | ❌ No | ✅ Yes |
| **Modification Needed?** | ✅ Yes, must modify class | ❌ No, works outside class |
| **Example Usage** | `Collections.sort(list)` | `Collections.sort(list, comparator)` |

#### When to Use?
- Use **Comparable** when the **natural order** of objects needs to be defined **within** the class.
- Use **Comparator** when **custom sorting** is needed, or when sorting logic should be **external** to the entity class.
---