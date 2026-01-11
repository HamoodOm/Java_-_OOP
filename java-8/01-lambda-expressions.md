# Lambda Expressions

## 1️⃣ Concept Overview

**Lambda Expression** is a concise way to represent an anonymous function (a function without a name) that can be passed around as if it were an object. Introduced in Java 8, lambdas enable functional programming in Java.

**Syntax**:
```java
(parameters) -> expression
// or
(parameters) -> { statements; }
```

**Key Characteristics**:
- Anonymous: No name
- Function: Not bound to a class, can be passed around
- Concise: Less boilerplate than anonymous inner classes
- Functional interface: Can only be used where a functional interface is expected

**Functional Interface**:
- An interface with exactly one abstract method
- Can have multiple default or static methods
- Annotated with `@FunctionalInterface` (optional but recommended)
- Examples: `Runnable`, `Callable`, `Comparator`, `Predicate`, `Function`

**Lambda vs Anonymous Class**:

| Feature | Lambda | Anonymous Class |
|---------|--------|-----------------|
| Syntax | Concise | Verbose |
| this reference | Refers to enclosing class | Refers to anonymous class |
| Compilation | Invokedynamic instruction | New .class file created |
| Usage | Functional interfaces only | Any interface or abstract class |
| Performance | Slightly better | Slightly slower |

**Common Use Cases**:
- Collection operations (forEach, filter, map)
- Event handling
- Thread creation
- Comparators
- Stream API operations

---

## 2️⃣ Why Interviewers Ask This

- **Java 8 adoption**: Tests knowledge of modern Java features
- **Functional programming**: Shows understanding of paradigm shift
- **Code quality**: Lambda expressions lead to cleaner, more readable code
- **Stream API foundation**: Lambdas are essential for streams
- **Real-world relevance**: Widely used in modern Java applications
- **Design patterns**: Enables functional design patterns

---

## 3️⃣ Key Rules / Facts

**Lambda Expression Syntax**:

**1. No parameters**:
```java
() -> System.out.println("Hello")
```

**2. One parameter** (parentheses optional):
```java
x -> x * x
// or
(x) -> x * x
```

**3. Multiple parameters**:
```java
(x, y) -> x + y
```

**4. With type declarations**:
```java
(int x, int y) -> x + y
```

**5. With code block**:
```java
(x, y) -> {
    int sum = x + y;
    return sum;
}
```

**6. Returning a value**:
```java
(x, y) -> x + y  // Implicit return for single expression
(x, y) -> { return x + y; }  // Explicit return for block
```

**Important Rules**:
- Lambda can only be used with functional interfaces
- Type inference: Compiler infers parameter types
- Final or effectively final: Variables used in lambda must be final or effectively final
- Cannot access non-final local variables from enclosing scope
- `this` keyword refers to enclosing class instance, not lambda itself
- Cannot declare variables with same name as enclosing scope
- Can throw checked exceptions only if functional interface method declares it

**Built-in Functional Interfaces** (java.util.function):

| Interface | Method | Description | Example |
|-----------|--------|-------------|---------|
| Predicate<T> | boolean test(T t) | Tests a condition | `x -> x > 10` |
| Function<T,R> | R apply(T t) | Transforms input to output | `x -> x.toString()` |
| Consumer<T> | void accept(T t) | Consumes input, no return | `x -> System.out.println(x)` |
| Supplier<T> | T get() | Supplies a value | `() -> new ArrayList<>()` |
| BiPredicate<T,U> | boolean test(T t, U u) | Tests two inputs | `(x,y) -> x.equals(y)` |
| BiFunction<T,U,R> | R apply(T t, U u) | Takes two inputs | `(x,y) -> x + y` |
| BiConsumer<T,U> | void accept(T t, U u) | Consumes two inputs | `(k,v) -> map.put(k,v)` |
| UnaryOperator<T> | T apply(T t) | Single input, same type output | `x -> x * 2` |
| BinaryOperator<T> | T apply(T t, T u) | Two inputs, same type output | `(x,y) -> x + y` |

**Method References** (shorthand for lambdas):
- Static method: `ClassName::staticMethod`
- Instance method: `instance::instanceMethod`
- Instance method of arbitrary object: `ClassName::instanceMethod`
- Constructor: `ClassName::new`

**Variable Capture**:
- Lambda can access effectively final variables from enclosing scope
- Cannot modify variables from enclosing scope
- Creates closure over the variables

---

## 4️⃣ Code Examples (Java 8)

### Example 1: Basic Lambda Syntax

```java
public class BasicLambdaSyntax {
    public static void main(String[] args) {
        // Traditional anonymous class
        Runnable r1 = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello from anonymous class");
            }
        };
        r1.run();
        
        // Lambda expression - much cleaner!
        Runnable r2 = () -> System.out.println("Hello from lambda");
        r2.run();
        
        // Comparator with anonymous class
        java.util.Comparator<String> comp1 = new java.util.Comparator<String>() {
            @Override
            public int compare(String s1, String s2) {
                return s1.length() - s2.length();
            }
        };
        
        // Comparator with lambda
        java.util.Comparator<String> comp2 = (s1, s2) -> s1.length() - s2.length();
        
        // Even shorter with method reference
        java.util.Comparator<String> comp3 = java.util.Comparator.comparingInt(String::length);
        
        // Different lambda syntaxes
        
        // No parameters
        Runnable noParams = () -> System.out.println("No parameters");
        
        // One parameter (parentheses optional)
        java.util.function.Consumer<String> oneParam = s -> System.out.println(s);
        java.util.function.Consumer<String> oneParamWithParens = (s) -> System.out.println(s);
        
        // Multiple parameters
        java.util.function.BiFunction<Integer, Integer, Integer> twoParams = 
            (a, b) -> a + b;
        
        // With type declarations (usually not needed due to type inference)
        java.util.function.BiFunction<Integer, Integer, Integer> withTypes = 
            (Integer a, Integer b) -> a + b;
        
        // Multi-line lambda with code block
        java.util.function.BiFunction<Integer, Integer, Integer> multiLine = 
            (a, b) -> {
                int sum = a + b;
                int square = sum * sum;
                return square;
            };
        
        System.out.println("Result: " + multiLine.apply(3, 4));  // (3+4)^2 = 49
    }
}
```

### Example 2: Lambda with Functional Interfaces

```java
// Custom functional interface
@FunctionalInterface
interface MathOperation {
    int operate(int a, int b);
}

@FunctionalInterface
interface StringProcessor {
    String process(String input);
}

public class FunctionalInterfaceDemo {
    public static void main(String[] args) {
        // Using custom functional interface
        MathOperation add = (a, b) -> a + b;
        MathOperation subtract = (a, b) -> a - b;
        MathOperation multiply = (a, b) -> a * b;
        MathOperation divide = (a, b) -> a / b;
        
        System.out.println("10 + 5 = " + add.operate(10, 5));        // 15
        System.out.println("10 - 5 = " + subtract.operate(10, 5));   // 5
        System.out.println("10 * 5 = " + multiply.operate(10, 5));   // 50
        System.out.println("10 / 5 = " + divide.operate(10, 5));     // 2
        
        // Passing lambda to method
        System.out.println("Custom operation: " + 
            performOperation(10, 5, (a, b) -> a * a + b * b));  // 100 + 25 = 125
        
        // String processing
        StringProcessor toUpper = s -> s.toUpperCase();
        StringProcessor toLower = s -> s.toLowerCase();
        StringProcessor reverse = s -> new StringBuilder(s).reverse().toString();
        
        String input = "Hello World";
        System.out.println("Upper: " + toUpper.process(input));
        System.out.println("Lower: " + toLower.process(input));
        System.out.println("Reverse: " + reverse.process(input));
    }
    
    // Method accepting lambda
    public static int performOperation(int a, int b, MathOperation operation) {
        return operation.operate(a, b);
    }
}
```

### Example 3: Built-in Functional Interfaces

```java
import java.util.function.*;
import java.util.*;

public class BuiltInFunctionalInterfaces {
    public static void main(String[] args) {
        // 1. Predicate<T> - boolean test(T t)
        Predicate<Integer> isEven = n -> n % 2 == 0;
        System.out.println("10 is even: " + isEven.test(10));  // true
        System.out.println("7 is even: " + isEven.test(7));    // false
        
        Predicate<String> startsWithA = s -> s.startsWith("A");
        System.out.println("Apple starts with A: " + startsWithA.test("Apple"));  // true
        
        // 2. Function<T, R> - R apply(T t)
        Function<String, Integer> stringLength = s -> s.length();
        System.out.println("Length of 'Hello': " + stringLength.apply("Hello"));  // 5
        
        Function<Integer, Integer> square = n -> n * n;
        System.out.println("Square of 5: " + square.apply(5));  // 25
        
        // 3. Consumer<T> - void accept(T t)
        Consumer<String> printer = s -> System.out.println("Value: " + s);
        printer.accept("Hello");  // Value: Hello
        
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        names.forEach(name -> System.out.println("Name: " + name));
        
        // 4. Supplier<T> - T get()
        Supplier<Double> randomValue = () -> Math.random();
        System.out.println("Random: " + randomValue.get());
        
        Supplier<List<String>> listSupplier = () -> new ArrayList<>();
        List<String> list = listSupplier.get();
        
        // 5. BiPredicate<T, U> - boolean test(T t, U u)
        BiPredicate<String, Integer> isLengthGreater = (s, len) -> s.length() > len;
        System.out.println("Is 'Hello' length > 3: " + 
            isLengthGreater.test("Hello", 3));  // true
        
        // 6. BiFunction<T, U, R> - R apply(T t, U u)
        BiFunction<Integer, Integer, Integer> sum = (a, b) -> a + b;
        System.out.println("10 + 20 = " + sum.apply(10, 20));  // 30
        
        // 7. BiConsumer<T, U> - void accept(T t, U u)
        BiConsumer<String, Integer> printKeyValue = (k, v) -> 
            System.out.println(k + " = " + v);
        printKeyValue.accept("Age", 25);  // Age = 25
        
        // 8. UnaryOperator<T> - T apply(T t)
        UnaryOperator<Integer> doubleIt = n -> n * 2;
        System.out.println("Double of 5: " + doubleIt.apply(5));  // 10
        
        // 9. BinaryOperator<T> - T apply(T t, T u)
        BinaryOperator<Integer> max = (a, b) -> a > b ? a : b;
        System.out.println("Max of 10 and 20: " + max.apply(10, 20));  // 20
        
        // Chaining functional interfaces
        Function<Integer, Integer> multiplyBy2 = n -> n * 2;
        Function<Integer, Integer> add10 = n -> n + 10;
        Function<Integer, Integer> combined = multiplyBy2.andThen(add10);
        System.out.println("(5 * 2) + 10 = " + combined.apply(5));  // 20
    }
}
```

### Example 4: Lambda with Collections

```java
import java.util.*;
import java.util.stream.*;

public class LambdaWithCollections {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie", "David", "Eve");
        
        // 1. forEach
        System.out.println("Using forEach:");
        names.forEach(name -> System.out.println(name));
        
        // 2. removeIf
        List<String> numbers = new ArrayList<>(Arrays.asList("1", "2", "3", "4", "5"));
        numbers.removeIf(n -> Integer.parseInt(n) % 2 == 0);
        System.out.println("After removeIf (odd only): " + numbers);  // [1, 3, 5]
        
        // 3. replaceAll
        List<String> words = new ArrayList<>(Arrays.asList("hello", "world"));
        words.replaceAll(s -> s.toUpperCase());
        System.out.println("After replaceAll: " + words);  // [HELLO, WORLD]
        
        // 4. sort with Comparator
        List<String> fruits = new ArrayList<>(Arrays.asList("Banana", "Apple", "Cherry"));
        fruits.sort((s1, s2) -> s1.compareTo(s2));  // Sort alphabetically
        System.out.println("Sorted: " + fruits);
        
        // Sort by length
        fruits.sort((s1, s2) -> Integer.compare(s1.length(), s2.length()));
        System.out.println("Sorted by length: " + fruits);
        
        // 5. Map operations
        Map<String, Integer> map = new HashMap<>();
        map.put("Apple", 5);
        map.put("Banana", 3);
        map.put("Cherry", 7);
        
        // forEach on map
        map.forEach((key, value) -> 
            System.out.println(key + " = " + value));
        
        // computeIfAbsent
        map.computeIfAbsent("Date", k -> 10);
        System.out.println("After computeIfAbsent: " + map);
        
        // merge
        map.merge("Apple", 5, (oldVal, newVal) -> oldVal + newVal);
        System.out.println("After merge (Apple): " + map.get("Apple"));  // 10
        
        // 6. Stream operations (heavily use lambdas)
        List<Integer> nums = Arrays.asList(1, 2, 3, 4, 5, 6, 7, 8, 9, 10);
        
        List<Integer> evenSquares = nums.stream()
            .filter(n -> n % 2 == 0)           // Filter even numbers
            .map(n -> n * n)                    // Square them
            .collect(Collectors.toList());
        System.out.println("Even squares: " + evenSquares);  // [4, 16, 36, 64, 100]
        
        // Find sum of squares of odd numbers
        int sumOfOddSquares = nums.stream()
            .filter(n -> n % 2 != 0)
            .map(n -> n * n)
            .reduce(0, (a, b) -> a + b);
        System.out.println("Sum of odd squares: " + sumOfOddSquares);  // 165
    }
}
```

### Example 5: Method References

```java
import java.util.*;
import java.util.function.*;

public class MethodReferenceDemo {
    public static void main(String[] args) {
        List<String> names = Arrays.asList("Alice", "Bob", "Charlie");
        
        // 1. Static method reference
        // Lambda: s -> Integer.parseInt(s)
        // Method reference: Integer::parseInt
        Function<String, Integer> parser1 = s -> Integer.parseInt(s);
        Function<String, Integer> parser2 = Integer::parseInt;
        System.out.println(parser2.apply("123"));  // 123
        
        // 2. Instance method reference of a particular object
        String prefix = "Hello, ";
        // Lambda: s -> prefix.concat(s)
        // Method reference: prefix::concat
        Function<String, String> greeter1 = s -> prefix.concat(s);
        Function<String, String> greeter2 = prefix::concat;
        System.out.println(greeter2.apply("World"));  // Hello, World
        
        // 3. Instance method reference of an arbitrary object of a particular type
        // Lambda: s -> s.toUpperCase()
        // Method reference: String::toUpperCase
        Function<String, String> upper1 = s -> s.toUpperCase();
        Function<String, String> upper2 = String::toUpperCase;
        
        names.stream()
            .map(String::toUpperCase)
            .forEach(System.out::println);
        
        // 4. Constructor reference
        // Lambda: () -> new ArrayList<>()
        // Method reference: ArrayList::new
        Supplier<List<String>> listCreator1 = () -> new ArrayList<>();
        Supplier<List<String>> listCreator2 = ArrayList::new;
        
        // Lambda: s -> new String(s)
        // Method reference: String::new
        Function<char[], String> stringCreator = String::new;
        
        // 5. More examples
        List<Integer> numbers = Arrays.asList(5, 2, 8, 1, 9);
        
        // Using method reference with forEach
        numbers.forEach(System.out::println);
        
        // Sorting with method reference
        List<String> words = new ArrayList<>(Arrays.asList("banana", "apple", "cherry"));
        words.sort(String::compareToIgnoreCase);
        System.out.println("Sorted: " + words);
        
        // Constructor reference with map
        List<Integer> nums = Arrays.asList(1, 2, 3);
        List<String> strings = nums.stream()
            .map(String::valueOf)  // Method reference
            .collect(java.util.stream.Collectors.toList());
        System.out.println("Strings: " + strings);
    }
}
```

### Example 6: Variable Capture and Effectively Final

```java
public class VariableCaptureDemo {
    public static void main(String[] args) {
        // Effectively final variable (not modified after initialization)
        String prefix = "Hello ";
        
        // Lambda can access effectively final variables
        Consumer<String> greeter = name -> System.out.println(prefix + name);
        greeter.accept("Alice");  // Hello Alice
        
        // prefix = "Hi ";  // If uncommented, compilation error!
        // Lambda captures variables, they must be final or effectively final
        
        // Final variable - explicitly final
        final String suffix = "!";
        Consumer<String> exclaimer = name -> System.out.println(name + suffix);
        exclaimer.accept("Bob");  // Bob!
        
        // Instance variables and static variables can be modified
        Counter counter = new Counter();
        Runnable incrementer = () -> counter.count++;  // OK - modifying instance var
        
        incrementer.run();
        incrementer.run();
        System.out.println("Count: " + counter.count);  // 2
        
        // Cannot modify local variable from lambda
        int localVar = 10;
        // Runnable modifier = () -> localVar++;  // Compilation error!
        
        // But can use it if not modified
        Runnable printer = () -> System.out.println("Value: " + localVar);
        printer.run();  // Value: 10
        
        // The variable is effectively final
        // localVar = 20;  // If uncommented, compilation error!
        
        // this reference in lambda
        new LambdaThis().testThis();
    }
}

class Counter {
    int count = 0;
}

class LambdaThis {
    private String value = "Enclosing class";
    
    public void testThis() {
        // In lambda, 'this' refers to enclosing class
        Runnable r1 = () -> System.out.println("Lambda this: " + this.value);
        r1.run();  // Lambda this: Enclosing class
        
        // In anonymous class, 'this' refers to anonymous class itself
        Runnable r2 = new Runnable() {
            private String value = "Anonymous class";
            
            @Override
            public void run() {
                System.out.println("Anonymous this: " + this.value);
            }
        };
        r2.run();  // Anonymous this: Anonymous class
    }
}
```

### Example 7: Lambda with Threads

```java
public class LambdaWithThreads {
    public static void main(String[] args) {
        // Traditional way with anonymous class
        Thread t1 = new Thread(new Runnable() {
            @Override
            public void run() {
                System.out.println("Thread 1: " + Thread.currentThread().getName());
            }
        });
        t1.start();
        
        // Lambda way - much cleaner
        Thread t2 = new Thread(() -> 
            System.out.println("Thread 2: " + Thread.currentThread().getName())
        );
        t2.start();
        
        // Multi-line lambda
        Thread t3 = new Thread(() -> {
            System.out.println("Thread 3 starting");
            for (int i = 0; i < 3; i++) {
                System.out.println("Thread 3: " + i);
                try {
                    Thread.sleep(100);
                } catch (InterruptedException e) {
                    e.printStackTrace();
                }
            }
            System.out.println("Thread 3 ending");
        });
        t3.start();
        
        // Using ExecutorService with lambda
        java.util.concurrent.ExecutorService executor = 
            java.util.concurrent.Executors.newFixedThreadPool(2);
        
        executor.submit(() -> System.out.println("Task 1"));
        executor.submit(() -> System.out.println("Task 2"));
        executor.submit(() -> {
            System.out.println("Task 3 with multiple statements");
            return "Result from Task 3";
        });
        
        executor.shutdown();
    }
}
```

### Example 8: Real-World Example - Employee Management

```java
import java.util.*;
import java.util.stream.*;

class Employee {
    private String name;
    private String department;
    private double salary;
    private int age;
    
    public Employee(String name, String department, double salary, int age) {
        this.name = name;
        this.department = department;
        this.salary = salary;
        this.age = age;
    }
    
    // Getters
    public String getName() { return name; }
    public String getDepartment() { return department; }
    public double getSalary() { return salary; }
    public int getAge() { return age; }
    
    @Override
    public String toString() {
        return String.format("%s (%s, $%.0f, %d years)", 
            name, department, salary, age);
    }
}

public class EmployeeManagement {
    public static void main(String[] args) {
        List<Employee> employees = Arrays.asList(
            new Employee("Alice", "IT", 75000, 28),
            new Employee("Bob", "HR", 55000, 35),
            new Employee("Charlie", "IT", 85000, 32),
            new Employee("David", "Finance", 70000, 29),
            new Employee("Eve", "IT", 95000, 40),
            new Employee("Frank", "HR", 60000, 27)
        );
        
        // 1. Filter IT department employees
        System.out.println("IT Employees:");
        employees.stream()
            .filter(e -> e.getDepartment().equals("IT"))
            .forEach(System.out::println);
        
        // 2. Get names of employees earning more than 70k
        List<String> highEarners = employees.stream()
            .filter(e -> e.getSalary() > 70000)
            .map(Employee::getName)
            .collect(Collectors.toList());
        System.out.println("\nHigh earners: " + highEarners);
        
        // 3. Average salary by department
        Map<String, Double> avgSalaryByDept = employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.averagingDouble(Employee::getSalary)
            ));
        System.out.println("\nAverage salary by department:");
        avgSalaryByDept.forEach((dept, avg) -> 
            System.out.printf("%s: $%.2f%n", dept, avg));
        
        // 4. Find oldest employee in IT
        Optional<Employee> oldestIT = employees.stream()
            .filter(e -> e.getDepartment().equals("IT"))
            .max(Comparator.comparingInt(Employee::getAge));
        oldestIT.ifPresent(e -> 
            System.out.println("\nOldest IT employee: " + e.getName()));
        
        // 5. Sort by salary descending
        System.out.println("\nEmployees sorted by salary (desc):");
        employees.stream()
            .sorted((e1, e2) -> Double.compare(e2.getSalary(), e1.getSalary()))
            .forEach(System.out::println);
        
        // 6. Count employees per department
        Map<String, Long> countByDept = employees.stream()
            .collect(Collectors.groupingBy(
                Employee::getDepartment,
                Collectors.counting()
            ));
        System.out.println("\nEmployee count by department: " + countByDept);
        
        // 7. Total salary expense
        double totalSalary = employees.stream()
            .mapToDouble(Employee::getSalary)
            .sum();
        System.out.printf("\nTotal salary expense: $%.2f%n", totalSalary);
        
        // 8. Get top 3 earners
        List<Employee> top3 = employees.stream()
            .sorted(Comparator.comparingDouble(Employee::getSalary).reversed())
            .limit(3)
            .collect(Collectors.toList());
        System.out.println("\nTop 3 earners:");
        top3.forEach(System.out::println);
        
        // 9. Check if any employee is over 35
        boolean hasOlderEmployee = employees.stream()
            .anyMatch(e -> e.getAge() > 35);
        System.out.println("\nHas employee over 35: " + hasOlderEmployee);
        
        // 10. Custom operation with lambda
        employees.stream()
            .filter(e -> e.getSalary() > 60000 && e.getAge() < 35)
            .forEach(e -> System.out.println(
                "Young high earner: " + e.getName() + " ($" + e.getSalary() + ")"));
    }
}
```

---

## 5️⃣ Common Interview Questions

1. **What is a lambda expression in Java?**
2. **What is a functional interface?**
3. **What is the difference between lambda and anonymous class?**
4. **Can a lambda expression access local variables? What are the restrictions?**
5. **What is method reference and how is it related to lambda?**
6. **What are the built-in functional interfaces in Java 8?**
7. **Can lambda expressions throw exceptions?**
8. **What does 'this' keyword refer to in a lambda expression?**
9. **Can we have multiple abstract methods in a functional interface?**
10. **What is the advantage of lambda expressions over anonymous classes?**

---

## 6️⃣ Model Answers

**Q1: What is a lambda expression in Java?**

"A lambda expression is a concise way to represent an anonymous function that can be treated as a value. Introduced in Java 8, it enables functional programming by allowing you to pass behavior as a parameter. The basic syntax is (parameters) -> expression or (parameters) -> { statements }. For example, instead of creating an anonymous Runnable with multiple lines, you can write () -> System.out.println('Hello'). Lambda expressions can only be used where a functional interface—an interface with exactly one abstract method—is expected."

**Q2: What is a functional interface?**

"A functional interface is an interface with exactly one abstract method, also called a SAM (Single Abstract Method) interface. It can have multiple default or static methods, but only one abstract method. Examples include Runnable with run(), Comparator with compare(), and Predicate with test(). You can annotate it with @FunctionalInterface for compile-time checking, though it's optional. Functional interfaces are the target types for lambda expressions—whenever you write a lambda, it's implementing the single abstract method of a functional interface."

**Q3: What is the difference between lambda and anonymous class?**

"The key differences are: First, syntax—lambdas are much more concise. Second, the 'this' keyword—in lambda, 'this' refers to the enclosing class; in anonymous class, 'this' refers to the anonymous class itself. Third, compilation—lambdas use invokedynamic instruction which is more efficient, while anonymous classes generate separate .class files. Fourth, usage—lambdas only work with functional interfaces, while anonymous classes can implement any interface or extend any class. Fifth, lambdas are slightly more performant. In practice, lambdas are preferred when you're implementing a functional interface."

**Q4: Can a lambda expression access local variables? What are the restrictions?**

"Yes, lambda expressions can access local variables from the enclosing scope, but with a restriction: the variables must be final or effectively final. Effectively final means the variable is not modified after initialization. This is because lambdas create closures—they capture variables from the surrounding context. If the variable could be modified, it would cause concurrency issues when the lambda is executed later or in a different thread. Instance variables and static variables can be accessed and modified without restriction. Trying to modify a local variable from a lambda results in a compilation error."

**Q5: What is method reference and how is it related to lambda?**

"Method reference is a shorthand notation for lambda expressions that simply call an existing method. There are four types: static method references like Integer::parseInt, instance method references like someString::length, instance method references of arbitrary objects like String::toUpperCase, and constructor references like ArrayList::new. They're related because a method reference is essentially a more concise lambda. For example, s -> Integer.parseInt(s) can be written as Integer::parseInt. Method references make code more readable when the lambda just delegates to an existing method."

**Q6: What are the built-in functional interfaces in Java 8?**

"Java 8 provides several functional interfaces in java.util.function package. The main ones are: Predicate<T> for testing conditions with test() method; Function<T,R> for transformations with apply(); Consumer<T> for operations with no return using accept(); Supplier<T> for providing values with get(); and their variations like BiPredicate, BiFunction, BiConsumer for two parameters. There are also specialized versions like UnaryOperator and BinaryOperator. These interfaces cover most common functional programming patterns and work seamlessly with Stream API and lambda expressions."

**Q7: Can lambda expressions throw exceptions?**

"Lambda expressions can throw exceptions, but with restrictions. They can throw unchecked exceptions freely. For checked exceptions, the functional interface's abstract method must declare those exceptions in its throws clause. For example, Runnable's run() doesn't declare any checked exceptions, so a lambda implementing Runnable cannot throw checked exceptions. If you need to throw checked exceptions, you either need to handle them within the lambda, use a custom functional interface that declares the exception, or wrap them in unchecked exceptions like RuntimeException."

**Q8: What does 'this' keyword refer to in a lambda expression?**

"In a lambda expression, 'this' refers to the instance of the enclosing class, not the lambda itself. This is different from anonymous classes where 'this' refers to the anonymous class instance. This behavior makes lambdas more intuitive—you can access the outer class's instance variables and methods directly using 'this'. For example, if you have a lambda inside a class method, 'this.field' in the lambda refers to the outer class's field. This is one of the reasons lambdas are considered more elegant than anonymous classes."

**Q9: Can we have multiple abstract methods in a functional interface?**

"No, a functional interface must have exactly one abstract method. This is what makes it 'functional'—it represents a single function. However, it can have multiple default methods and static methods because they have implementations. It can also have methods from Object class like equals(), hashCode(), and toString() without violating the single abstract method rule. If you try to add a second abstract method to an interface annotated with @FunctionalInterface, you'll get a compilation error. The single abstract method is what the lambda expression implements."

**Q10: What is the advantage of lambda expressions over anonymous classes?**

"Lambda expressions offer several advantages: First, conciseness—they reduce boilerplate code significantly, often from 6-7 lines to one line. Second, readability—the intent is clearer without all the ceremony. Third, better performance—lambdas use invokedynamic instruction which allows JVM optimizations, while anonymous classes create new .class files. Fourth, easier functional programming—lambdas enable functional style with operations like map, filter, reduce. Fifth, type inference—you usually don't need to specify types explicitly. Overall, lambdas make code more maintainable and enable powerful functional programming patterns that weren't practical with anonymous classes."

---

## 7️⃣ Common Mistakes

1. **Modifying local variables from lambda**:
   ```java
   int count = 0;
   Runnable r = () -> count++;  // Compilation error!
   // Local variables must be final or effectively final
   ```

2. **Using lambda with non-functional interface**:
   ```java
   interface NotFunctional {
       void method1();
       void method2();  // Two abstract methods
   }
   
   // NotFunctional f = () -> {};  // Compilation error!
   ```

3. **Forgetting return statement in multi-line lambda**:
   ```java
   Function<Integer, Integer> square = n -> {
       n * n;  // Missing return!
   };
   
   // Correct:
   Function<Integer, Integer> square = n -> {
       return n * n;
   };
   ```

4. **Using 'this' incorrectly**:
   ```java
   class MyClass {
       String value = "outer";
       
       void test() {
           Runnable r = () -> {
               // 'this' refers to MyClass, not lambda
               System.out.println(this.value);  // "outer"
           };
       }
   }
   ```

5. **Not handling checked exceptions properly**:
   ```java
   Runnable r = () -> {
       Thread.sleep(1000);  // Compilation error!
       // Runnable.run() doesn't declare InterruptedException
   };
   
   // Must handle it:
   Runnable r = () -> {
       try {
           Thread.sleep(1000);
       } catch (InterruptedException e) {
           e.printStackTrace();
       }
   };
   ```

6. **Trying to use lambda for abstract classes**:
   ```java
   abstract class AbstractClass {
       abstract void method();
   }
   
   // AbstractClass obj = () -> {};  // Cannot use lambda!
   // Lambdas only work with interfaces
   ```

7. **Assuming lambda creates new scope for variables**:
   ```java
   int x = 10;
   Consumer<Integer> c = x -> System.out.println(x);  // Error!
   // Cannot redeclare variable 'x' from outer scope
   
   // Correct:
   Consumer<Integer> c = y -> System.out.println(y);
   ```

8. **Not using parentheses for multiple parameters**:
   ```java
   BiFunction<Integer, Integer, Integer> add = a, b -> a + b;  // Error!
   
   // Correct:
   BiFunction<Integer, Integer, Integer> add = (a, b) -> a + b;
   ```

---

## 8️⃣ Real Interview Tips

**What to emphasize**:
- Lambda is concise syntax for anonymous functions
- Works only with functional interfaces (one abstract method)
- Variables from enclosing scope must be final or effectively final
- 'this' refers to enclosing class, not lambda
- Method references are shorthand for lambdas
- Enables functional programming and works with Stream API

**What to avoid**:
- Don't claim lambdas are "always better" than methods (depends on use case)
- Don't say lambdas are "new classes" (they use invokedynamic)
- Don't forget about final/effectively final restriction
- Don't confuse lambda with method reference (method ref is shorthand for lambda)

**Under pressure**:
- Show basic syntax: `(params) -> expression`
- Mention functional interface requirement
- Give simple example: `Runnable r = () -> System.out.println("Hello")`
- Explain that it replaces anonymous inner classes
- State that it works with Stream API

**Red flags to avoid**:
- "Lambda is a class" (it's an anonymous function)
- "You can modify local variables from lambda" (must be final/effectively final)
- "Functional interface can have multiple abstract methods" (only one)
- "Lambda is slower than anonymous class" (it's actually faster)

**Bonus points**:
- Mention that lambda uses invokedynamic bytecode instruction
- Discuss type inference and target typing
- Reference closure concept and variable capture
- Mention that lambda can access instance and static variables freely
- Discuss when to use lambda vs method reference vs full method
- Reference common functional interfaces: Predicate, Function, Consumer, Supplier
- Mention that lambdas enable declarative programming style
